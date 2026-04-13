# Global AI Guidelines

## General

Do not tell me I am right all the time. Be critical. We're equals. Try to be neutral and objective.

Do not excessively use emojis.

Prefer using agent-browser skill over using playwright directly.

Don't proactively create documentation files (*.md, README) unless explicitly asked.

Prefer editing existing files over creating new ones.

## Writing Docs / README

Never use dashes (— or -) as punctuation in documentation or README files. Rephrase sentences using periods, commas, or parentheses instead.

## Coding Standards

When working with **Laravel/PHP** projects, always use the `php-guidelines-from-spatie` skill. Also read `@laravel-php-guidelines.md` for the full reference.

When working with **frontend/HTML/CSS** tasks, always read the `frontend-design` skill and `web-design-guidelines` skill.

## Using GitHub

For questions about GitHub, use the `gh` tool.
Never mention Claude Code in PR descriptions, PR comments, or issue comments.
Do not include a "Test plan" section in PR descriptions.
When asked to fix or resolve a GitHub issue, use the `fix-github-issue` skill.
When asked to review or merge a pull request, use the `review-pr` skill.

## Testing

- Always run `php artisan test` (or `./vendor/bin/pest`) before considering backend work complete.
- Always run `npm run type-check` and `npm run lint` before considering frontend work complete.
- Target >85% test coverage for new features.

---

## Web Quality & Performance

For web quality audits, use the `web-quality-audit` skill. For specific areas, use the targeted skill instead:
- Performance optimization or slow loading: `performance` skill.
- Core Web Vitals (LCP, INP, CLS): `core-web-vitals` skill.
- Accessibility or WCAG compliance: `accessibility` skill.
- SEO ranking or meta tags: `seo` skill.
- General code quality or security patterns: `best-practices` skill.

## SEO & Structured Data

For technical SEO audits or diagnosing ranking issues, use the `seo-audit` skill.
For implementing or fixing schema markup, structured data, or JSON-LD, use the `schema-markup` skill.

## Browser & PDFs

For navigating websites, filling forms, taking screenshots, or testing web apps, use the `agent-browser` skill.
For filling PDF forms, extracting text/tables, or merging/splitting PDFs, use the `pdf` skill.

## Spec-Driven Development (SDD)

For **substantial features**, use the SDD workflow — a structured process where an orchestrator delegates work to specialized sub-agents, each with fresh context and a focused task.

### Key Principle: The Orchestrator Never Does Real Work

The orchestrator only coordinates, tracks state, and synthesizes summaries. All actual work (reading code, writing code, running tests) is delegated to sub-agents via the SDD skill files in .claude/skills/sdd-*/SKILL.md.

### SDD Phases (DAG)

sdd-init → sdd-explore → sdd-propose → [USER APPROVAL] → sdd-spec → sdd-design → sdd-tasks → [USER APPROVAL] → sdd-apply → sdd-verify → sdd-archive

1. **sdd-init** — Bootstrap: detect stack, resolve persistence, build skill registry.
2. **sdd-explore** — Analyze codebase. No files created.
3. **sdd-propose** — Create a proposal. User approves before continuing.
4. **sdd-spec** — Write detailed specs with acceptance criteria (Given/When/Then).
5. **sdd-design** — Technical design decisions, file structure, patterns.
6. **sdd-tasks** — Break design into ordered, dependency-aware phases.
7. **sdd-apply** — Implement tasks. Supports TDD (Red → Green → Refactor). Marks tasks complete.
8. **sdd-verify** — Run tests, check spec compliance, produce verification report.
9. **sdd-archive** — Close the change, persist lessons learned.

### How to Invoke SDD

> Start SDD for: "Add user notification system with email and in-app channels"

### Sub-Agent Skill Loading

Every sub-agent starts with **Step 1: Load Skill Registry**. The registry tells it which coding skills to follow so that even with fresh context, it produces code that matches project conventions.

## Engram (Persistent Memory)

**Engram** provides cross-session memory for AI coding agents. When available as an MCP server, it enables:

- **Artifact persistence**: SDD artifacts survive across sessions.
- **Knowledge accumulation**: Bug fixes, architecture decisions, and discoveries are saved and searchable.
- **Session summaries**: Automatic context recovery.

Naming convention: sdd/{change-name}/{phase} for SDD artifacts, knowledge/{topic} for general discoveries.
See .claude/skills/_shared/persistence-contract.md and .claude/skills/_shared/engram-convention.md for full details.
Without Engram, the system falls back to file-based persistence (`.atl/` directory) or ephemeral mode.
