# JIRA Ticket Field Format

Tickets have **three separate field targets** — do not merge them into one blob.

---

## description field

Contains only the Summary line and user story. Nothing else.

```
**Summary:** [One sentence restating the ticket's goal — what will be true when this is done]

**Description:** As a [user type], I want to [action] so that [benefit].
```

---

## Acceptance Criteria field (AC_FIELD_ID)

Pass as a separate field in `additional_fields`. Plain text, one BDD statement per line.

```
- Given [initial context or precondition], when [user or system action], then [expected outcome].
- Given [initial context], when [alternate or edge case], then [expected system behavior].
```

Include 2–4 BDD criteria. Each must be independently verifiable and unambiguous.

**Fallback:** If `AC_FIELD_ID` is null, append this section to `description` under the heading `**Acceptance Criteria**`.

---

## Definition of Done field (DOD_FIELD_ID)

Pass as a separate field in `additional_fields`. Plain text checkbox list.

```
- [ ] Implementation complete and code reviewed by a peer
- [ ] Unit and integration tests written; coverage maintained at 100%
- [ ] Tested by [QA / developer / PO] covering [specific scenarios from AC above]
- [ ] [Feature-specific item — e.g. "Error states handled and displayed to user", "API contract documented"]
- [ ] Accepted by [Product Owner / stakeholder]
```

**Fallback:** If `DOD_FIELD_ID` is null, append this section to `description` under the heading `**Definition of Done**`.

---

## Field Guidance

### User Types (common examples — adapt as needed)

- `corrector` — internal user processing ESG data corrections
- `reviewer` — user approving or rejecting corrections in the review queue
- `administrator` — user managing system configuration or permissions
- `API consumer` — downstream service or integration

### BDD Criteria Tips

- **Given** sets up state: user is logged in, a file has been uploaded, the queue is empty
- **When** is the triggering action: user clicks submit, API returns 500, filter is applied
- **Then** is the observable outcome: modal closes, error banner appears, record count updates
- Write one criterion per observable behavior — don't bundle multiple outcomes in one bullet
- Include at least one happy path and one error/edge case

### Definition of Done — project-specific items to consider

- `[ ] 100% test coverage maintained (Vitest + Istanbul)`
- `[ ] Zod schema updated for new API response shape`
- `[ ] Server action added/updated in src/actions/`
- `[ ] MSW handler added to src/mocks/handlers.ts for new endpoint`
- `[ ] No disabled tests (vitest/no-disabled-tests enforced)`
- `[ ] ESLint and Prettier pass (npm run lint && npm run prettier-format)`
- `[ ] Type-checked (npm run tsc)`

---

## Example — Story

**description field:**

```
**Summary:** Users can filter the review queue by submission type using a dropdown.

**Description:** As a reviewer, I want to filter the review queue by submission type so that I can focus on the correction category I am responsible for.
```

**Acceptance Criteria field:**

```
- Given the review queue is loaded with mixed submission types, when I select "Utility Usage" from the type dropdown, then only records with submission type "Utility Usage" are displayed.
- Given a submission type filter is active, when I clear the filter, then all records are displayed again and the dropdown resets to its default state.
- Given no records match the selected filter, when the filtered results load, then an empty state message is shown rather than an error.
```

**Definition of Done field:**

```
- [ ] Implementation complete and code reviewed by a peer
- [ ] Unit and integration tests written; coverage maintained at 100%
- [ ] Tested by QA covering: filter applies correctly, filter clears, empty state renders
- [ ] MSW handler added for filtered ReviewQueue POST endpoint
- [ ] ESLint and Prettier pass
- [ ] Accepted by Product Owner
```

---

## Example — Sub-task

**description field:**

```
**Summary:** Add aria-hidden="true" to decorative icons in ReviewQueueCard so screen readers skip them.

**File:** src/components/ReviewQueue/ReviewQueueCard.tsx (~lines 119–122, 220, 257)
**WCAG:** 1.1.1 Non-text Content

**Description:** As a screen reader user, I want decorative icons in the ReviewQueueCard to be hidden from the accessibility tree so that I hear only the adjacent meaningful text without redundant icon announcements.
```

**Acceptance Criteria field:**

```
- Given a ReviewQueueCard is rendered, when a screen reader traverses ArrowsPointingInIcon or ArrowRightIcon, then no announcement is made for those icons.
- Given an icon is the sole conveyor of meaning (no adjacent text), when encountered by a screen reader, then an aria-label or visually-hidden text alternative is present instead of aria-hidden.
```

**Definition of Done field:**

```
- [ ] Implementation complete and code reviewed by a peer
- [ ] Unit and integration tests written; coverage maintained at 100%
- [ ] Verified with VoiceOver/NVDA that icons produce no announcement
- [ ] WCAG 1.1.1 Non-text Content compliance confirmed
- [ ] ESLint and Prettier pass
- [ ] Accepted by Product Owner
```
