---
name: changelog-generator
description: Automatically creates user-facing changelogs from git commits by analyzing commit history, categorizing changes, and transforming technical commits into clear, customer-friendly release notes. Turns hours of manual changelog writing into minutes of automated generation.
argument-hint: [title] [timeframe]
---

# Changelog Generator

This skill transforms technical git commits into polished, user-friendly changelogs that your customers and users will actually understand and appreciate.

## When to Use This Skill

- Preparing release notes for a new version
- Creating weekly or monthly product update summaries
- Documenting changes for customers
- Writing changelog entries for app store submissions
- Generating update notifications
- Creating internal release documentation
- Maintaining a public changelog/product updates page

## What This Skill Does

1. **Scans Git History**: Analyzes commits from a specific time period or between versions
2. **Categorizes Changes**: Groups commits into logical categories (features, improvements, bug fixes, breaking changes, security)
3. **Translates Technical → User-Friendly**: Converts developer commits into customer language
4. **Formats Professionally**: Creates clean, structured changelog entries
5. **Filters Noise**: Excludes internal commits (refactoring, tests, etc.)
6. **Follows Best Practices**: Applies changelog guidelines and your brand voice

## Arguments

The skill accepts two optional positional arguments: `[title]` and `[timeframe]`.

```
/changelog-generator "Direct Data Control" "past 7 days"
```

| Argument | Description | Fallback |
|---|---|---|
| `title` | Product/project name used in the changelog header | Inferred from `git remote get-url origin` or repo directory name; falls back to `Release Notes` |
| `timeframe` | Natural language description of the period to cover (e.g. `"past 7 days"`, `"since v2.4.0"`, `"March 1–15"`) | Defaults to commits since the last git tag |

Either argument can be omitted or supplied alone:

```
/changelog-generator "Direct Data Control"
```
```
/changelog-generator "" "past 7 days"
```

If a `title` is provided, use it as the changelog header:
```
## Direct Data Control — Release Notes
```

## Output

After generating the changelog, write it to a file in the repository root named using the current date:

```
changelog-YYYY-MM-DD.md
```

For example: `changelog-2024-03-10.md`

Confirm to the user that the file was written and its path.

## Formatting

Always apply the conventions from `style.md` (co-located with this skill) when generating changelogs. This includes section order, heading levels, dividers, bold entry titles, and the summary footer.

## How to Use

### Basic Usage

From your project repository:

```
Create a changelog from commits since last release
```

```
Generate changelog for all commits from the past week
```

```
Create release notes for version 2.5.0
```

### With Specific Date Range

```
Create a changelog for all commits between March 1 and March 15
```

### With Custom Guidelines

`style.md` is applied automatically. You can also reference it explicitly:

```
Create a changelog for commits since v2.4.0, following @style.md
```

## Example

**User**: `/changelog-generator "CorrectorAPI" "past 7 days"`

**Output**:
```markdown
## 📋 CorrectorAPI — Release Notes
### 📅 Week of March 10, 2024

### 🐛 Bug Fixes

- **Image Upload Failure** — Fixed an issue where large images failed to upload, ensuring files of any size are accepted reliably.
- **Timezone in Scheduled Posts** — Resolved confusion where scheduled posts used the wrong timezone, so posts now go out exactly when intended.
- **Notification Badge Count** — Corrected an inaccurate unread count on the notification badge.

### ✨ New Features

- **Team Workspaces** — Create separate workspaces for different projects and invite team members to keep work organized.
- **Keyboard Shortcuts** — Press `?` to view all available shortcuts and navigate faster without a mouse.

### 🔧 Improvements

- **Faster File Sync** — Files now sync 2× faster across devices.
- **Full-Content Search** — Search now includes file contents, not just titles.

### 💡 Summary: 
- X bug fix(es)
- X new feature(s)
- X improvement(s)
- X automated deployments over the past [period].
```

**Inspired by:** Manik Aggarwal's use case from Lenny's Newsletter

## Tips

- Run from your git repository root
- Specify date ranges for focused changelogs
- Use `style.md` (co-located with this skill) for consistent formatting — it is applied automatically.
- Review and adjust the generated changelog before publishing
- Save output directly to CHANGELOG.md

## Related Use Cases

- Creating GitHub release notes
- Writing app store update descriptions
- Generating email updates for users
- Creating social media announcement posts
