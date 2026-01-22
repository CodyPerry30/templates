# Document Templates for Human & AI Collaboration

> **Structured templates for seamless product development, technical specification, and AI-assisted coding workflows**

This repository provides three comprehensive document templates designed to bridge human planning and AI execution. Whether you're defining product requirements, architecting technical solutions, or onboarding AI assistants to your codebase, these templates create a common language that works for both humans and AI.

## Templates Overview

| Template | Purpose | Key Contents | Best For |
|----------|---------|--------------|----------|
| [**PRD-TEMPLATE.md**](templates/PRD-TEMPLATE.md) | Product Requirements Document | Problem statement, user personas, success metrics, scope definition, risks | Product managers, stakeholders defining what to build and why |
| [**SPEC-TEMPLATE.md**](templates/SPEC-TEMPLATE.md) | Technical Specification | Data models, API contracts, functional requirements, testing criteria, code standards | Engineers and architects designing how to build it |
| [**CLAUDE-TEMPLATE.md**](templates/CLAUDE-TEMPLATE.md) | AI Assistant Project Guide | Tech stack, commands, architecture, code style, boundaries, workflows | AI assistants (Claude, Cursor, etc.) and developers needing quick project context |

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
│   └── SPEC-TEMPLATE.md      # Technical specification template
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
