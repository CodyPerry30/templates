---
name: jira-tickets
description: Analyze a conversation or task list and create well-structured JIRA tickets via the Atlassian MCP server. Supports Story/Sub-task hierarchy, BDD acceptance criteria, story points, and preview-before-create workflow. All tickets are created in the backlog — epic assignment is handled by the PM.
invocation: jira-tickets
argument-hint: [project-key (ESG)] [Paste a task list, describe a feature, or point me at something earlier in the conversation.]
arguments:
  - name: project-key
    description: JIRA project key (e.g. ESG). Defaults to ESG if omitted.
    required: true
  - description: Paste a task list, describe a feature, or point me at something earlier in the conversation.
    required: true
user-invocable: true
---

# Skill: jira-tickets

Break down a conversation, task list, or planning doc into well-structured JIRA tickets and create them via the Atlassian MCP server.

> **Important:** This skill NEVER creates new Epics and NEVER links stories to epics. Epic assignment is the PM's responsibility and is handled outside this workflow.
>
> **Important:** This skill NEVER adds tickets to the active sprint. All tickets are created in the backlog (no sprint field is set).

## Invocation

`/jira-tickets [project-key]`

Example: `/jira-tickets ESG`

Default project: **ESG** — used when no `project-key` argument is supplied.

---

## Workflow

### Step 0 — Confirm project key and resolve current user (ALWAYS first)

Run both of these in parallel before any other work:

1. **Confirm the target project:**
   - If `project-key` argument was supplied → use it directly, no prompt needed.
   - If `project-key` was NOT supplied → ask the user:

     > "Which JIRA project should I create these tickets in? (default: **ESG**)"

     Wait for the user's response. If they reply with just "yes", "ok", or press enter with no key, use **ESG**.

2. **Resolve the current user** — call `atlassianUserInfo` to get the invoking user's Atlassian account ID.
   - Extract the `account_id` field from the response (it is a long alphanumeric string, e.g. `62b1ded1cebad33432f6f32e`).
   - Store this exact value as `CURRENT_USER_ACCOUNT_ID`.
   - This will be passed as the `assignee_account_id` parameter on every `createJiraIssue` call in Step 5.
   - **Important:** `assignee_account_id` requires the raw Atlassian `accountId` string — never a display name or email address. If the conversation names a different assignee, call `lookupJiraAccountId` with their name or email to resolve their `accountId` before creating any tickets.

### Step 1 — Analyze the conversation

Scan everything in the current conversation (user messages, task lists, specs, plans) and extract discrete work items. For each item note:

- The core action or deliverable
- Any stated priority cues ("urgent", "blocking", "nice-to-have")
- Dependencies or ordering constraints between items
- Which user type benefits and why (for the user story)
- Implied complexity (lines of code, number of components, unknown risk)

**Hierarchy is always Story → Sub-task.** Epics are never created or linked.

- **Flat Stories** — fewer than ~5 items OR all items belong to a single feature/sprint
- **Story → Sub-task** — any single item large enough to decompose further into child tasks

### Step 2 — Discover JIRA context

1. Project key is already confirmed from Step 0 — no need to call `getVisibleJiraProjects`.

2. Call `getJiraProjectIssueTypesMetadata` with the confirmed project key to get valid issue type IDs.

3. Call `getJiraIssueTypeMetaWithFields` for the **Story** issue type to discover:
   - Custom field ID for story points (often `customfield_10016` or `customfield_10028` — verify from response)
   - Custom field ID for **Acceptance Criteria** (look for field names containing "Acceptance Criteria" or "acceptance_criteria")
   - Custom field ID for **Definition of Done** (look for field names containing "Definition of Done" or "definition_of_done")
   - Custom field ID for labels (often `labels`)
   - Any required fields the project enforces
   - Store these as `AC_FIELD_ID` and `DOD_FIELD_ID` (may be null if not found in this project)

### Step 3 — Draft all tickets

Use the format from `ticket-format.md` (embedded below) for **every** ticket's description body — Stories **and** Sub-tasks alike. Sub-tasks must never use a bare text blurb; they must use the full structured template just like stories.

**Summary line format:** `[Context Prefix]: [Concise action title in present tense]`

Example summaries:

- `DDC: Add infinite scroll to review queue`
- `DDC: Validate CSV structure before submission`

**Per-ticket fields to populate:**

| Field                 | Guidance                                                                                                                                                                                                                                                                                                                                                                 |
| --------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `summary`             | Context prefix + action title (≤80 chars)                                                                                                                                                                                                                                                                                                                                |
| `description`         | **Only** the Summary line + user story Description per ticket-format.md — do NOT include Acceptance Criteria or Definition of Done here                                                                                                                                                                                                                                  |
| `AC_FIELD_ID`         | Acceptance Criteria BDD statements (if field was discovered). **Must be Atlassian Document Format (ADF) JSON** — see ADF format note below. Omit if `AC_FIELD_ID` is null.                                                                                                                                                                                               |
| `DOD_FIELD_ID`        | Definition of Done checklist (if field was discovered). **Must be Atlassian Document Format (ADF) JSON** — see ADF format note below. Omit if `DOD_FIELD_ID` is null.                                                                                                                                                                                                    |
| `issuetype`           | Story / Sub-task / Task — from discovered IDs. **Never Epic.**                                                                                                                                                                                                                                                                                                           |
| `story_points`        | Fibonacci: 1, 2, 3, 5, 8, 13. Estimate from: lines-of-code scope, unknowns, dependencies. 1=trivial, 3=half-day, 5=full-day, 8=multi-day, 13=needs splitting                                                                                                                                                                                                             |
| `priority`            | Critical / High / Medium / Low — infer from urgency cues in conversation                                                                                                                                                                                                                                                                                                 |
| `labels`              | 1–3 domain tags (e.g. `frontend`, `api`, `testing`, `auth`, `performance`)                                                                                                                                                                                                                                                                                               |
| `assignee_account_id` | Pass `CURRENT_USER_ACCOUNT_ID` as the top-level `assignee_account_id` parameter **AND** also include `"assignee": {"accountId": "CURRENT_USER_ACCOUNT_ID"}` inside `additional_fields`. Both are required — the top-level param alone does not reliably apply. If a different person is named, call `lookupJiraAccountId` first. **Never pass a display name or email.** |
| `parent`              | Story key for sub-tasks only. **Do NOT set a parent for Stories** — epic assignment is handled by the PM.                                                                                                                                                                                                                                                                |

> **Field routing rule:** If `AC_FIELD_ID` and `DOD_FIELD_ID` were discovered, pass Acceptance Criteria and Definition of Done as separate `additional_fields` entries in `createJiraIssue`. If either field was NOT discovered, fall back to including it in `description` as before.

> **ADF format requirement:** Custom textarea fields in Jira (including AC and DoD) require Atlassian Document Format (ADF) — plain text strings will be rejected with a `Bad Request` error. Always pass these fields as ADF JSON objects in `additional_fields`. Use a `bulletList` for both AC and DoD items:
>
> ```json
> {
>   "version": 1,
>   "type": "doc",
>   "content": [
>     {
>       "type": "bulletList",
>       "content": [
>         {
>           "type": "listItem",
>           "content": [
>             {
>               "type": "paragraph",
>               "content": [{ "type": "text", "text": "Your bullet text here" }]
>             }
>           ]
>         }
>       ]
>     }
>   ]
> }
> ```
>
> Repeat the `listItem` block for each bullet point. Do **not** pass a plain string or markdown — it will fail.

> **Sub-task description guidance:** For sub-tasks, the `**Summary:**` line should include the specific file path and line numbers. The `**Description:**` user story should be scoped to the single focused change. Acceptance Criteria should be 1–2 BDD statements (not 4) since scope is narrow. The Definition of Done checklist remains the same.

**Story point heuristics:**

- **1** — Single-file change, no logic, purely cosmetic
- **2** — Small isolated change, clear requirements, no API work
- **3** — Single component or action, well-understood, low risk
- **5** — Multiple components, API integration, or non-trivial state
- **8** — Cross-cutting feature, multiple files, some unknowns
- **13** — Split this ticket — too large for one story

### Step 4 — Preview (REQUIRED before any MCP create calls)

Render a tree summary like:

```
## Tickets to Create in [PROJECT_KEY]

(All stories go to the backlog — epic assignment handled by PM)

  ├── Story: DDC: [Title] (3 pts · High)
  │   ├── Sub-task: DDC: [Title] (1 pt)
  │   └── Sub-task: DDC: [Title] (2 pts)
  └── Story: DDC: [Title] (5 pts · Medium)
  └── Story: DDC: [Title] (3 pts · Low)
```

Then ask:

> "Ready to create these [N] tickets in [PROJECT_KEY]? Reply **yes** to create all, list ticket numbers to skip (e.g. `skip 2, 4`), or **cancel** to abort."

Do NOT call any `createJiraIssue` or `createIssueLink` MCP tools until the user confirms.

### Step 5 — Create tickets

Execute in dependency order:

1. **Create Stories first** — do NOT set a `parent` field on stories. They land in the backlog unlinked.
2. **Create Sub-tasks** — set `parent` field to the Story key from step above.
3. **Never set a sprint field** — all tickets must land in the backlog. Do not pass `sprint`, `customfield_10020`, or any sprint-related field in any `createJiraIssue` call.
4. After all creates, report:

```
## Created Tickets

| Key | Type | Summary | Points |
|-----|------|---------|--------|
| ESG-102 | Story | DDC: ... | 3 |
| ESG-103 | Sub-task | DDC: ... | 1 |
```

---

## Ticket Description Format (ticket-format.md embedded)

The `description` field contains **only** the Summary line and user story. Acceptance Criteria and Definition of Done go into their own dedicated Jira fields (`AC_FIELD_ID` / `DOD_FIELD_ID`).

**description field:**

```
**Summary:** [One sentence restating the ticket's goal]

**Description:** As a [user type], I want to [action] so that [benefit].
```

**Acceptance Criteria field (AC_FIELD_ID):**

```
- Given [initial context], when [user/system action], then [expected outcome].
- Given [initial context], when [edge case or error], then [expected system behavior].
(2–4 BDD criteria total; each must be independently verifiable)
```

**Definition of Done field (DOD_FIELD_ID):**

```
- [ ] Implementation complete and code reviewed
- [ ] Unit and integration tests written, coverage maintained at 100%
- [ ] Tested by [QA/dev/PO] with [specific scenarios listed above]
- [ ] [Any feature-specific DoD item — e.g. "API contract documented", "Error states handled"]
- [ ] Accepted by [Product Owner / stakeholder]
```

If `AC_FIELD_ID` or `DOD_FIELD_ID` are null (not found in project), include those sections in `description` as a fallback.

Adapt the user type, action, and DoD items to the specific work item. Do not use placeholder text — every field must be filled with context-specific content drawn from the conversation.

---

## Error Handling

- If a custom story points field cannot be found → omit story points from the create call and note it in the final report
- If a ticket creation fails with `"Operation value must be an Atlassian Document"` → the AC or DoD field was passed as plain text instead of ADF JSON. Retry the ticket with proper ADF format (see ADF format note in Step 3). Do NOT skip the ticket.
- If a ticket creation fails for any other reason → report the error, continue with remaining tickets, summarize all failures at the end
- If the user says "skip N" → omit those tickets from the creation run but still create the rest in correct dependency order
- **NEVER create an Epic issue type**