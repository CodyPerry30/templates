# Changelog Style Guide

This file defines the formatting and tone conventions for changelogs, used by the changelog-generator skill.

## Format

Use the following structure for each release entry:

```markdown
## CorrectorAPI — Release Notes
### 📅 [Date Range or Version]

### 🐛 Bug Fixes

- **[Short Title]** — [User-friendly description of what was fixed and why it matters.]

### ✨ New Features

- **[Short Title]** — [User-friendly description of what was added and the benefit.]

### 🔧 Improvements

- **[Short Title]** — [User-friendly description of what was improved.]

### 📚 Documentation

- **[Short Title]** — [Description of doc changes.]

### ⚙️ Infrastructure

- [Description of deployment, image, or infra changes.]

### 💡 Summary: 
- X bug fix(es)
- X new feature(s)
- X improvement(s)
- X automated deployments over the past [period].
```

## Section Emoji Reference

| Section | Emoji |
|---|---|
| Bug Fixes | 🐛 |
| New Features | ✨ |
| Improvements | 🔧 |
| Documentation | 📚 |
| Infrastructure | ⚙️ |
| Breaking Changes | ⚠️ |
| Security | 🔒 |
| Performance | 🚀 |

## Writing Style

- **Tone:** Clear, professional, and user-facing. Avoid developer jargon.
- **Voice:** Active voice preferred (e.g. "Fixed a bug" not "A bug was fixed").
- **Titles:** Bold, short (3–6 words), describe the outcome not the code change.
- **Descriptions:** One sentence. Explain *what* changed and *why it matters* to the user.
- **Omit:** Internal refactors, test-only changes, linting, and dependency bumps unless they affect behavior.
- **Automated deployments:** Group under Infrastructure; do not list individually.

## Section Rules

- Only include sections that have at least one entry.