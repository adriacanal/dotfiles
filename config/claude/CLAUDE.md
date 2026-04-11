# Global AI Guidelines

## General

Do not tell me I am right all the time. Be critical. We're equals. Try to be neutral and objective.

Do not excessively use emojis.

Prefer using browser agent skill over using playwright directly.

## Writing Docs / README

Never use dashes (— or -) as punctuation in documentation or README files. Rephrase sentences using periods, commas, or parentheses instead.

## Coding Standards

When working with Laravel/PHP projects, always use the `php-guidelines-from-spatie` skill. Also read `@laravel-php-guidelines.md` for the full reference.

## Using GitHub

For questions about GitHub, use the `gh` tool.
Never mention Claude Code in PR descriptions, PR comments, or issue comments.
Do not include a "Test plan" section in PR descriptions.

## Spec-Driven Development (SDD)

For **substantial features**, use the SDD workflow from Gentle AI. The orchestrator delegates all real work to specialized sub-agents. It never writes code directly.

### SDD Phases

```
sdd-init → sdd-explore → sdd-propose → [APPROVAL] → sdd-spec → sdd-design → sdd-tasks → [APPROVAL] → sdd-apply → sdd-verify → sdd-archive
```

To start: describe a substantial feature and the orchestrator will suggest SDD. Or explicitly:
```
> Start SDD for: "Add user notification system with email and in-app channels"
```

To resume from a previous session:
```
> Continue SDD for: "add-notifications"
```

### Sub-Agent Skill Loading

Every SDD sub-agent loads the **skill registry** as Step 1. The registry tells it which coding skills to follow (php-guidelines-from-spatie, etc.) so it produces code matching project conventions even with fresh context.

For smaller tasks that don't need the full SDD pipeline, use the standalone agents (laravel-debugger, laravel-feature-builder, laravel-simplifier, task-planner) instead.

## Engram (Persistent Memory)

When Engram MCP is available, all agents should save discoveries, decisions, and fixes:

- SDD artifacts: `sdd/{change-name}/{phase}` (e.g. `sdd/add-notifications/spec`)
- Knowledge: `knowledge/{topic}` (e.g. `knowledge/n+1-query-user-list-fix`)
- Sessions: `session/{date}-{description}`

Naming uses `topic_key` for upserts (saving with the same key updates, doesn't duplicate).

Before starting work on something that might have prior context, search first:
```
mem_search(query: "{relevant-topic}", project: "{project}")
```

See `skills/_shared/persistence-contract.md` and `skills/_shared/engram-convention.md` for full conventions.

Without Engram, the system falls back to file-based persistence (`.atl/` directory) or ephemeral mode.

## Web Quality

For performance optimization, Core Web Vitals, accessibility (WCAG), and SEO audits, the Addy Osmani web-quality-skills are available. They activate automatically based on the request:

```
> Audit this page for web quality issues
> Optimize performance and fix Core Web Vitals
> Review accessibility and suggest improvements
```

For structured data specifically, use the `schema-markup` skill. For broader SEO, use `seo-audit`.

## Testing

- Always run `php artisan test` (or `./vendor/bin/pest`) before considering backend work complete.
- Always run `npm run type-check` and `npm run lint` before considering frontend work complete.
- Target >85% test coverage for new features.
- Use Pest for PHP testing. Use Vitest for frontend testing when available.
