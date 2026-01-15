# SENIOR-PLANNER PLUGIN

**Generated**: 2025-01-16 02:18

## OVERVIEW
Root directory of Senior Planner Claude Code plugin - virtual engineering team for comprehensive feature planning.

## STRUCTURE
```
senior-planner/
├── .claude-plugin/
│   └── plugin.json              # Plugin metadata (name, version, keywords)
├── skills/senior-planning/
│   └── SKILL.md                 # Main workflow orchestrator
├── agents/                      # 10 specialized interviewer agents
├── README.md                    # Complete user guide
```

## WHERE TO LOOK
| Task | Location | Notes |
|------|----------|-------|
| Plugin metadata | `.claude-plugin/plugin.json` | Name, version, description |
| Main workflow | `skills/senior-planning/SKILL.md` | Sequential interview orchestration |
| Agent definitions | `agents/*.md` | Domain-specific interviewer agents |
| Installation guide | `README.md` | Setup, usage, examples |

## CONVENTIONS
- **No build required**: Plugin installs directly via `claude plugin install`
- **Markdown-only**: All content in .md files, no executable code
- **Standard Claude plugin structure**: `.claude-plugin/` + `skills/` + `agents/`
- **Command entry**: `/senior-planning` triggers SKILL.md orchestrator
- **Output**: Generates `.claude/SPEC.md` in user's project (not in plugin directory)

## ANTI-PATTERNS
- **NEVER modify agent execution order**: Sequential flow is critical for context building
- **DON'T bypass interviews**: Each agent contributes to comprehensive spec
- **NEVER hardcode project conventions**: Always discover via code-rule-reader agent
