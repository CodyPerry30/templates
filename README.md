# Document Templates for Human & AI Collaboration

> **Structured templates for seamless product development, technical specification, and AI-assisted coding workflows**

This repository provides three comprehensive document templates designed to bridge human planning and AI execution. Whether you're defining product requirements, architecting technical solutions, or onboarding AI assistants to your codebase, these templates create a common language that works for both humans and AI.

## Templates Overview

| Template | Purpose | Key Contents | Best For |
|----------|---------|--------------|----------|
| [**PRD-TEMPLATE.md**](templates/PRD-TEMPLATE.md) | Product Requirements Document | Problem statement, user personas, success metrics, scope definition, risks | Product managers, stakeholders defining what to build and why |
| [**SPEC-TEMPLATE.md**](templates/SPEC-TEMPLATE.md) | Technical Specification | Data models, API contracts, functional requirements, testing criteria, code standards | Engineers and architects designing how to build it |
| [**CLAUDE-TEMPLATE.md**](templates/CLAUDE-TEMPLATE.md) | AI Assistant Project Guide | Tech stack, commands, architecture, code style, boundaries, workflows | AI assistants (Claude, Cursor, etc.) and developers needing quick project context |
| [**CLAUDE_TEMPLATE_V2.md**](templates/docs/CLAUDE_TEMPLATE_V2.md) | Modular AI Project Guide | References external standards docs, concise project config, modular approach | Teams wanting separation of concerns between project config and coding standards |
| [**STANDARDS-TEMPLATE.md**](templates/docs/STANDARDS-TEMPLATE.md) | Standards Document Template | Configurable template for creating domain-specific standards | Teams creating custom coding standards for their tech stack |

## Standards Library

The `templates/docs/standards/` directory contains a library of reusable coding standards documents. These are designed to work with both **CLAUDE-TEMPLATE.md** and **CLAUDE_TEMPLATE_V2.md** by referencing them in the Code Style section.

### Available Standards

| Standard | Description |
|----------|-------------|
| [**api-standards.md**](templates/docs/standards/api-standards.md) | REST API conventions, endpoint naming, request/response patterns, error handling |
| [**csharp-standards.md**](templates/docs/standards/csharp-standards.md) | C# coding conventions, naming, patterns, async/await, LINQ usage |
| [**cicd-standards.md**](templates/docs/standards/cicd-standards.md) | CI/CD pipeline configuration, build/deploy stages, environment management |
| [**database-standards.md**](templates/docs/standards/database-standards.md) | Database design, migration patterns, query optimization, naming conventions |
| [**documentation-standards.md**](templates/docs/standards/documentation-standards.md) | Code comments, README structure, API documentation, changelog format |
| [**git-standards.md**](templates/docs/standards/git-standards.md) | Branching strategy, commit messages, PR guidelines, code review process |
| [**security-standards.md**](templates/docs/standards/security-standards.md) | Authentication, authorization, secrets management, secure coding practices |
| [**testing-standards.md**](templates/docs/standards/testing-standards.md) | Unit testing, integration testing, E2E testing, test naming, coverage goals |
| [**typescript-standards.md**](templates/docs/standards/typescript-standards.md) | TypeScript/React conventions, type patterns, component structure, hooks |

### How to Use the Standards Library

1. **Copy relevant standards** to your project's `docs/` directory
2. **Reference them in your CLAUDE.md** using the Code Style section pattern:

```markdown
## Code Style
[Read `docs/csharp-standards.md` when modifying or creating any C# files.]
[Read `docs/typescript-standards.md` when modifying TS or TSX files.]
```

3. **Customize as needed** for your project's specific requirements
4. **Create new standards** using **STANDARDS-TEMPLATE.md** for domains not covered

## Quick Start

### How to Use These Templates

1. **Copy the template** you need to your project directory
2. **Customize** the sections with your project-specific information
3. **Integrate** into your workflow:
   - Add `CLAUDE.md` to your repository root for AI assistance
   - Reference `PRD.md` in project planning meetings
   - Use `SPEC.md` as implementation blueprint for development

### When to Use Each Template

- **Starting a new feature?** → Begin with **PRD-TEMPLATE.md** to clarify goals and requirements
- **Ready to implement?** → Use **SPEC-TEMPLATE.md** to define technical architecture
- **Working with AI assistants?** → Create **CLAUDE.md** to give AI context about your project
- **Want modular standards?** → Use **CLAUDE_TEMPLATE_V2.md** with separate standards files for cleaner separation of concerns
- **Creating custom standards?** → Use **STANDARDS-TEMPLATE.md** to create domain-specific coding standards

### CLAUDE.md Template Comparison

| Aspect | CLAUDE-TEMPLATE.md | CLAUDE_TEMPLATE_V2.md |
|--------|-------------------|----------------------|
| **Structure** | All-in-one document | Modular with external references |
| **Standards** | Embedded inline | References separate standards files |
| **Best for** | Smaller projects, quick setup | Larger teams, shared standards across repos |
| **Maintenance** | Update in one place | Update standards independently |

### Suggested Workflow Sequence

```
1. PRD-TEMPLATE.md  →  Define the problem and user needs
2. SPEC-TEMPLATE.md →  Design the technical solution
3. CLAUDE.md        →  Enable AI-assisted implementation
```

## Template Relationships

These templates are designed to work together as a complete development workflow:

```
┌─────────────────┐
│  PRD Template   │  "What to build and why?"
│  (Problem)      │  • User needs, business goals
└────────┬────────┘  • Success metrics, scope
         │
         ▼
┌─────────────────┐
│  SPEC Template  │  "How to build it?"
│  (Solution)     │  • Technical design, data models
└────────┬────────┘  • API contracts, testing plan
         │
         ▼
┌─────────────────┐
│ CLAUDE Template │  "Onboard AI to execute"
│ (Execution)     │  • Project context, commands
└─────────────────┘  • Patterns, boundaries
```

**The Flow:**
- **PRD** captures stakeholder requirements and validates the problem is worth solving
- **SPEC** translates requirements into a concrete technical design
- **CLAUDE** provides AI assistants with the context needed to implement the spec following project conventions

## Examples

The `examples/` directory demonstrates the complete PRD → SPEC → CLAUDE workflow using real-world projects:

### Smart Task Search Project (PRD & SPEC)
- **[examples/prd.md](examples/prd.md)** - Product Requirements Document showing how to define a feature (smart task search with natural language)
- **[examples/spec.md](examples/spec.md)** - Technical Specification translating those requirements into implementation details

These two examples work together to show how product vision flows into technical design for the same feature.

### Corrector Project (CLAUDE.md)
- **[examples/claude.md](examples/claude.md)** - AI assistant guide for a Next.js application demonstrating:
  - How to document a tech stack with detailed context
  - Comprehensive command reference for development workflows
  - Architecture overview with directory structure
  - Code style guidelines and boundaries for AI assistants
  - Platform-specific notes and gotchas

Together, these examples show how the templates become living documents that help both humans and AI understand project context quickly.

## Best Practices

### Keeping Templates Current

- **Update as you build:** When you add dependencies, update commands, or change architecture patterns, reflect these in your templates
- **Version control:** Commit template updates alongside code changes so they stay synchronized
- **Regular reviews:** During sprint planning or retrospectives, verify templates match current reality

### Customization Guidance

- **Remove what you don't need:** These templates are comprehensive by design—delete sections that don't apply to your project
- **Add project-specific sections:** Include domain-specific information (e.g., "Deployment Pipeline" for DevOps-heavy projects)
- **Use concrete examples:** Replace placeholder text with actual code snippets, command outputs, or real user stories

### Common Pitfalls to Avoid

- **Don't let templates become stale:** Out-of-date documentation is worse than no documentation
- **Avoid over-engineering:** For simple projects, a streamlined CLAUDE.md might be all you need
- **Don't skip the "why":** Especially in PRDs and specs, explain rationale for decisions—it helps both humans and AI make aligned choices
- **Keep boundaries clear:** In CLAUDE.md, explicit guardrails (✅ Always, ⚠️ Ask first, 🚫 Never) prevent AI from making risky changes

### Working with Standards Files

- **Mix and match:** Only include the standards files relevant to your tech stack (e.g., skip `csharp-standards.md` for a JavaScript-only project)
- **Customize for your team:** Standards files are starting points—adjust rules, add project-specific conventions, or remove what doesn't apply
- **Share across repositories:** Keep standards in a central location and reference them from multiple projects to ensure consistency
- **Version your standards:** When standards change significantly, consider versioning (e.g., `api-standards-v2.md`) to allow gradual migration

## Repository Structure

```
templates/
├── README.md             # You are here
├── examples/
│   ├── prd.md            # PRD example: Smart Task Search feature
│   ├── spec.md           # SPEC example: Smart Task Search implementation
│   └── claude.md         # CLAUDE.md example (Corrector project)
├── templates/
│   ├── CLAUDE-TEMPLATE.md    # AI assistant project guide template
│   ├── PRD-TEMPLATE.md       # Product requirements document template
│   ├── SPEC-TEMPLATE.md      # Technical specification template
│   └── docs/
│       ├── CLAUDE_TEMPLATE_V2.md   # Modular AI project guide (references external standards)
│       ├── STANDARDS-TEMPLATE.md   # Template for creating custom standards docs
│       └── standards/
│           ├── api-standards.md          # REST API conventions
│           ├── csharp-standards.md       # C# coding standards
│           ├── cicd-standards.md         # CI/CD pipeline standards
│           ├── database-standards.md     # Database and migration standards
│           ├── documentation-standards.md # Documentation standards
│           ├── git-standards.md          # Git workflow and code review
│           ├── security-standards.md     # Security and auth standards
│           ├── testing-standards.md      # Testing conventions
│           └── typescript-standards.md   # TypeScript/React standards
├── agents/               # Reserved for future: specialized sub-agents
├── commands/             # Reserved for future: reusable AI command snippets
└── skills/               # Reserved for future: specialized AI skill modules
```

### Reserved Directories

- **`agents/`**: Planned for specialized sub-agents that can be invoked for specific workflows or domains
- **`commands/`**: Planned for reusable command definitions that can be referenced across projects
- **`skills/`**: Planned for specialized AI skill modules (e.g., testing workflows, deployment procedures)

These directories are placeholders for future expansion of the template ecosystem.

## Contributing

Have suggestions for improving these templates? Found a section that's confusing or missing?

- **Open an issue** describing the improvement or clarification needed
- **Submit a pull request** with template enhancements or additional examples
- **Share your experience** using these templates in your projects

### Future Vision

We're exploring:
- **Pre-built commands** in `commands/` for common AI workflows (e.g., "create API endpoint", "add test coverage")
- **Skill modules** in `skills/` for specialized AI capabilities (e.g., database migrations, CI/CD pipelines)
- **Template variants** for specific tech stacks or project types (e.g., mobile apps, microservices)

---

**Last Updated:** January 2026
