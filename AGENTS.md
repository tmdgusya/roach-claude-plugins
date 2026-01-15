# PROJECT KNOWLEDGE BASE

**Generated**: 2025-01-16 02:18
**Commit**: N/A
**Branch**: N/A

## OVERVIEW
Claude Code plugin "Senior Planner" - Provides virtual senior engineering team for comprehensive feature planning via structured interviews across 9 domains. Generates Engineering Specifications with complete TDD test plans.

## STRUCTURE
```
senior-planner/
├── .claude-plugin/
│   └── plugin.json              # Plugin metadata
├── skills/
│   └── senior-planning/
│       └── SKILL.md             # Main orchestrator workflow
├── agents/
│   ├── code-rule-reader.md       # Discovers project conventions
│   ├── tech-interviewer.md       # Technical architecture
│   ├── ux-interviewer.md        # UX/interface design
│   ├── security-interviewer.md    # Security/compliance
│   ├── tdd-test-engineer.md     # TDD test specifications
│   ├── test-coverage-verifier.md # Test coverage validation
│   ├── performance-interviewer.md # Performance/scalability
│   ├── integration-interviewer.md  # Deployment/integration
│   ├── spec-writer.md            # SPEC.md synthesis
│   └── wrap-agent.md             # Wrapper/orchestration
└── README.md                     # Plugin documentation
```

## WHERE TO LOOK
| Task | Location | Notes |
|------|----------|-------|
| Plugin metadata | `.claude-plugin/plugin.json` | Name, version, description |
| Main workflow | `skills/senior-planning/SKILL.md` | Sequential interview orchestration |
| Agent definitions | `agents/*.md` | 10 specialized interviewer agents |
| Installation/usage | `README.md` | Complete user guide |
| Coding standards (optional) | `.claude/AGENTS.md` or `CLAUDE.md` in target projects | Discovered by code-rule-reader |

## CONVENTIONS
- **Markdown-first**: All plugin content in .md files (no executable code)
- **Standard Claude plugin structure**: Follows `.claude-plugin/` + `skills/` + `agents/` pattern
- **Sequential workflow**: Agents execute sequentially (not parallel) to build context progressively
- **User-driven**: Active participation via AskUserQuestion throughout interviews
- **Output location**: `.claude/SPEC.md` in user's project directory
- **No build required**: Plugin installed directly, no compilation
- **Todo tracking**: Uses TodoWrite for progress tracking during interviews

## ANTI-PATTERNS (THIS PROJECT)
- **NEVER run agents in parallel**: Sequential execution is required for context building
- **NEVER skip interviews**: Each domain builds on previous interviews
- **NEVER assume conventions**: Always discover project standards via code-rule-reader first
- **DON'T write test specs in code**: Tests written as markdown specs (TDD guides), not executable tests

## UNIQUE STYLES
- **Interview-driven planning**: All specifications come from interactive interviews, not generation
- **RED→GREEN→REFACTOR guides**: Test specifications include step-by-step TDD implementation instructions
- **Comprehensive 14-section spec**: SPEC.md covers all engineering aspects (problem → implementation → testing → deployment → risks)
- **Test coverage verification**: Separate agent validates tests match user intent before implementation

## COMMANDS
```bash
# Install plugin
claude plugin install ./senior-planner --scope user

# Start planning session
/senior-planning [feature description]

# View installed plugins
/plugin
```

## NOTES
- Plugin works out-of-box, no configuration required
- Optional project conventions via `.claude/AGENTS.md` or `CLAUDE.md` files
- Planning takes 60-90 minutes by design (comprehensive interviews)
- Output is `.claude/SPEC.md` - serves as single source of truth for implementation
- Test specifications enable TDD workflow but don't include executable tests
- Use for complex features, not simple bug fixes
