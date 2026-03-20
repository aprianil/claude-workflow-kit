# Claude Workflow Kit

Compound engineering workflow for AI-assisted development. Plan before work,
manage context, self-review output, compound after shipping.

## Install as a Skill

```bash
npx skills add aprianil/claude-workflow-kit
```

This loads the workflow principles into your agent automatically. The agent will
plan before implementing, self-review its output, and prompt for a compound step
after shipping.

## Or Copy Manually

```
SKILL.md                 — Skill entry point (all principles in one file)
parallel-agents.md       — Parallel agents with git worktrees

.claude/
  CLAUDE.md              — Local rules for your .claude/ directory
  briefs/TEMPLATE.md     — PRD template for planning before implementation

memory/
  feedback_context_engineering.md   — Context window management principles
  feedback_plan_and_compound.md     — Plan-before-work + compound-after-shipping
  project_parallel_agents.md        — Parallel agents workflow (memory format)
```

### Manual setup:

1. Copy `.claude/CLAUDE.md` to your project's `.claude/CLAUDE.md`
2. Copy `.claude/briefs/` to your project's `.claude/briefs/`
3. Copy memory files to your Claude project memory directory
   (`~/.claude/projects/<project-path>/memory/`)
4. Add `.claude/CLAUDE.md` to `.gitignore` (it's personal workflow, not shared)

### What to customize per project:

- The briefs template — add project-specific validation criteria
- The memory files — add project-specific feedback as you work
- CLAUDE.md — add project-specific rules on top of these defaults

## What's In the Workflow

| Phase | What Happens |
| --- | --- |
| **Plan** | Research codebase (file:line references), research external docs, write brief, get human approval |
| **Work** | Implement in fresh context with plan loaded |
| **Review** | Self-review: hardest decision, rejected alternatives, lowest confidence |
| **Compound** | What went wrong → feedback, what pattern emerged → docs, context for later → log |

**The 50/50 rule:** 50% features, 50% system improvements. Each correction
encoded is one less correction next time.

## Sources

- [Compound Engineering](https://every.to/guides/compound-engineering) — Plan → Work → Review → Compound loop, 50/50 rule
- [No Vibes Allowed: Solving Hard Problems in Complex Codebases](https://www.youtube.com/watch?v=rmvDxxNubIg) — Dex Horthy, context engineering, Research → Plan → Implement
