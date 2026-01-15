# Senior Planner Plugin

A comprehensive Claude Code plugin that deploys a team of senior engineers to conduct in-depth planning interviews and produce detailed Engineering Specifications with complete TDD test plans.

## Overview

Building complex features requires more than a simple task list. You need:
- Deep understanding across multiple domains (technical, UX, security, performance)
- Thoughtful architectural decisions with trade-off analysis
- Comprehensive test specifications written BEFORE implementation (TDD)
- Complete engineering spec that serves as single source of truth

This plugin provides a structured interview process with **9 specialized senior engineers** covering every critical domain.

## Senior Engineering Team

Your team includes:

1. **Code Rule Reader** - Discovers project-specific coding standards
2. **Technical Architect** - System design, data models, API contracts
3. **UX Engineer** - User workflows, interface design, accessibility
4. **Security Engineer** - Auth, data protection, compliance (GDPR/SOC2)
5. **Lead Test Engineer** - TDD strategy, comprehensive test cases with RED→GREEN→REFACTOR guides
6. **Test Coverage Verifier** - Validates tests match user intent, identifies gaps
7. **Performance Engineer** - Scalability, caching, optimization, SLOs
8. **Integration Engineer** - External dependencies, deployment strategy
9. **Technical Writer** - Synthesizes all findings into SPEC.md

## What You Get

### Engineering Specification (`.claude/SPEC.md`)

A comprehensive 14-section specification:
1. **Problem Statement** - Background, goals, non-goals
2. **Solution Design** - Architecture, key decisions with rationale
3. **API/Interface Design** - Contracts, request/response formats
4. **Data Models** - Database schema, entities, relationships
5. **User Experience** - Workflows, UI components, error handling
6. **Technical Implementation** - Stack, components, code organization
7. **Security & Compliance** - Auth, encryption, audit logging
8. **Performance & Scalability** - SLOs, caching strategy, optimization
9. **Test Specification** - Complete TDD test cases with implementation guides
10. **Integration & Deployment** - Dependencies, rollout strategy, rollback
11. **Trade-offs & Alternatives** - What we chose and why
12. **Implementation Phases** - Breakdown of work into phases
13. **Risks & Mitigations** - Risk assessment with mitigation strategies
14. **References** - Supporting documentation

### TDD Test Specification

Detailed test cases for Test-Driven Development:
- **Given/When/Then** format for every test
- **Example test code** showing exact implementation
- **RED → GREEN → REFACTOR** workflow guides
- **Step-by-step instructions** for junior developers
- **Comprehensive coverage** across unit, integration, E2E tests
- **Mocking strategies** for external dependencies
- **Coverage goals** by component criticality

### Test Coverage Verification

Quality assurance that tests match your intent:
- **Gap analysis** - Identifies missing test scenarios
- **User validation** - Confirms tests verify what you care about
- **Clarification questions** - Resolves ambiguities before implementation
- **Confidence report** - Sign-off that coverage is complete

## Installation

### Prerequisites

- Claude Code installed and running
- Git repository (recommended, not required)

### Installation Methods

#### Method 1: Install from Marketplace (Recommended)

The simplest way to install this plugin:

```bash
# 1. Add the marketplace
/plugin marketplace add tmdgusya/roach-claude-plugins

# 2. Install the plugin
/plugin install senior-planner@roach-claude-plugins
```

That's it! The plugin will be available immediately.

> **Note**: This plugin is part of the `roach-claude-plugins` marketplace, which contains multiple plugins.

#### Method 2: Install from Local Directory

When developing or testing locally:

```bash
# From the directory containing senior-planner folder
claude plugin install ./senior-planner --scope user

# Or navigate into the plugin directory first
cd senior-planner
claude plugin install . --scope user
```

#### Method 3: Install from Git URL (Alternative)

Install the entire marketplace repository directly:

```bash
# HTTPS
claude plugin install https://github.com/tmdgusya/roach-claude-plugins.git --scope user

# SSH
claude plugin install git@github.com:tmdgusya/roach-claude-plugins.git --scope user

# Specific branch or tag
claude plugin install https://github.com/tmdgusya/roach-claude-plugins.git#develop --scope user
```

> **Note**: This installs all plugins in the marketplace, not just `senior-planner`.

### Installation Scopes

Choose where to install the plugin based on your needs:

| Scope | Command Flag | Settings File | Use Case |
|-------|-------------|---------------|----------|
| **User** (default) | `--scope user` | `~/.claude/settings.json` | Personal plugins across all projects |
| **Project** | `--scope project` | `.claude/settings.json` | Team plugins shared via Git |
| **Local** | `--scope local` | `.claude/settings.local.json` | Project-specific, not committed to Git |

**Examples:**

```bash
# Install for yourself only (default)
claude plugin install ./senior-planner --scope user

# Install for entire team (committed to Git)
claude plugin install ./senior-planner --scope project

# Install for this project only (not committed)
claude plugin install ./senior-planner --scope local
```

### Verify Installation

After installation, verify the plugin is available:

```bash
# Check installed plugins
/plugin

# The /senior-planning command should now be available
/senior-planning --help
```

### Plugin Management

**View and manage plugins:**
```bash
/plugin                    # Open interactive plugin manager
```

**Common commands:**
```bash
# Disable plugin temporarily
/plugin disable senior-planner@roach-claude-plugins

# Re-enable plugin
/plugin enable senior-planner@roach-claude-plugins

# Uninstall plugin
/plugin uninstall senior-planner@roach-claude-plugins

# Update marketplace (pulls latest plugin versions)
/plugin marketplace update roach-claude-plugins
```

### Troubleshooting Installation

**"Plugin not found" error:**
```bash
# Check if plugin is in the list
/plugin

# View any loading errors
# Open /plugin interface and check the "Errors" tab
```

**"Command not available" after installation:**
```bash
# Restart Claude Code or reload the session
# The plugin should be automatically discovered

# Verify the plugin loaded successfully
/plugin  # Check "Installed" tab
```

**Plugin conflicts:**
- If another plugin has a `/senior-planning` command, you may need to use the namespaced version
- Use `/senior-planner:senior-planning` to explicitly call this plugin's command

## Usage

### Basic Usage

```bash
# Start senior planning interview
/senior-planning Add user authentication with OAuth

# Or simply
/senior-planning
```

The team will then guide you through a comprehensive interview process.

### What Happens

**Phase 1: Discovery** (5 min)
- Understand feature requirements
- Explore existing codebase patterns
- Discover project coding standards (parallel)

**Phase 2: Technical Interview** (15-20 min)
- System architecture and component design
- Data models and database schema
- API contracts and technology choices

**Phase 3: UX Interview** (10-15 min)
- User workflows and journeys
- Interface design and components
- Accessibility and error states

**Phase 4: Security Interview** (10 min)
- Authentication and authorization
- Data protection and compliance
- Threat modeling and audit logging

**Phase 5: TDD Test Specification** (20-30 min)
- Comprehensive test cases for all components
- RED → GREEN → REFACTOR implementation guides
- Mocking strategy and test data

**Phase 6: Test Coverage Verification** (10-15 min)
- Validate tests match user intent
- Identify and address coverage gaps
- Confirm confidence in test completeness

**Phase 7: Performance Interview** (10 min)
- Performance SLOs and scale requirements
- Caching strategy and optimization
- Monitoring and alerting

**Phase 8: Integration Interview** (10 min)
- External dependencies and APIs
- Deployment strategy and rollback
- Migration and backward compatibility

**Phase 9: Spec Synthesis** (10 min)
- Consolidate all findings
- Resolve conflicts
- Create `.claude/SPEC.md`

**Total Time**: 60-90 minutes of comprehensive planning

### Example Session

```
You: /senior-planning Add password reset feature

Senior Planner: I'll guide you through comprehensive planning with our senior engineering team.

[Discovery Phase]
Senior Planner: I understand you want to add password reset. Let me explore your codebase...
[Searches for similar features, reads auth patterns]

[Code Standards Phase - Parallel]
Code Rule Reader: Discovering project coding standards...
[Finds .claude/AGENTS.md, reads conventions]

[Technical Interview]
Tech Interviewer: For the password reset feature, which data approach fits best?
  ☐ Token in database (secure, requires DB)
  ☐ Signed JWT token (stateless, requires key management)
  ☐ Time-limited hash (simple, less secure)
  ☑ Token in database (you select)

[Continues with detailed technical questions...]

[UX Interview]
UX Interviewer: When a reset link expires, what should users see?
  ☐ Generic error message
  ☐ Clear expiration notice with re-request option
  ☑ Clear notice + re-request (you select)

[Continues with UX questions...]

[Security Interview]
Security Engineer: For rate limiting password resets, what's appropriate?
  ☐ 3 attempts per hour (strict)
  ☐ 10 attempts per hour (moderate)
  ☑ 10 attempts per hour (you select)

[Continues with security questions...]

[TDD Test Specification]
Test Engineer: Creating comprehensive test cases for password reset...
[Generates 25 detailed test cases with RED→GREEN→REFACTOR guides]

[Test Coverage Verification]
Coverage Verifier: I notice tests don't cover the scenario where user's email changes mid-reset. Should we test this?
  ☑ Yes, this could happen (you select)
[Adds test case for email change scenario]

[Performance Interview]
Performance Engineer: What response time is acceptable for "send reset email"?
  ☐ < 100ms (instant)
  ☑ < 500ms (fast, email sends async) (you select)

[Integration Interview]
Integration Engineer: If email service is down, what happens?
  ☐ Fail and show error
  ☑ Queue for retry, show "check email soon" (you select)

[Spec Synthesis]
Spec Writer: Synthesizing all findings into SPEC.md...
[Creates comprehensive specification]

Senior Planner: ✓ Planning Complete!

SPEC.md created at .claude/SPEC.md with:
- Complete technical architecture
- Detailed UX flows
- Security requirements
- 25 TDD test cases with implementation guides
- Performance targets
- Integration strategy

Ready to implement using TDD! Start with Test Case 1.
```
## When to Use

### ✅ Use Senior Planner For:

- **Complex features** requiring architectural decisions
- **Features touching multiple systems** (frontend + backend + database + external APIs)
- **Security-sensitive features** (auth, payments, PII handling)
- **Features with unclear requirements** (need clarification through interviews)
- **Features requiring scale considerations** (performance critical)
- **Team-developed features** (shared understanding needed)
- **Features with compliance requirements** (GDPR, SOC2, HIPAA)

### ❌ Don't Use Senior Planner For:

- **Simple bug fixes** - Just fix it
- **Trivial changes** - (typo fixes, small tweaks)
- **Well-defined, small tasks** - Clear implementation, no planning needed
- **Exploratory work** - Use exploration agents instead
- **Pure research questions** - Use research tools

## Key Features

### Comprehensive Domain Coverage

- **9 specialized interviewers** each expert in their domain
- **Sequential interviews** build on each other (later interviews use earlier context)
- **Cross-domain synthesis** resolves conflicts, fills gaps

### Test-Driven Development Focus

- **Tests written BEFORE code** following TDD best practices
- **RED → GREEN → REFACTOR** cycle guides for every test
- **Example test code** showing exact implementation
- **Junior-friendly** - step-by-step instructions anyone can follow

### Test Coverage Verification

- **Quality check** on test specifications
- **User validation** ensures tests match intent
- **Gap identification** catches missing scenarios
- **Confidence sign-off** before implementation

### Project-Aware

- **Discovers coding standards** from AGENTS.md, CLAUDE.md files
- **Follows existing patterns** found in codebase
- **Respects conventions** (naming, architecture, testing)

### Active User Participation

- **You make decisions** - not passive planning
- **AskUserQuestion** throughout - 3-4 questions per interview round
- **Multiple rounds** per interviewer until complete
- **Your preferences** shape the final spec

## Benefits

### For Solo Developers

- **Senior-level guidance** for complex decisions
- **Comprehensive checklist** - don't forget important aspects
- **Test-first mindset** - TDD guides reduce bugs
- **Documentation** - SPEC.md serves as implementation reference

### For Teams

- **Shared understanding** - entire team reviews same SPEC.md
- **Consistent approach** - everyone follows same architecture
- **Onboarding tool** - new team members read SPEC.md
- **Communication aid** - non-technical stakeholders read Problem Statement and UX sections

### For Quality

- **Fewer bugs** - comprehensive test cases catch issues early
- **Better architecture** - thoughtful design upfront
- **Security built-in** - security interview catches vulnerabilities
- **Performance considered** - scalability planned from start

### Time Savings

- **60-90 min planning** saves hours/days of rework
- **TDD prevents bugs** - less debugging time
- **Clear spec** - less "what should I build?" confusion
- **Team alignment** - less back-and-forth discussions

## Requirements

- Claude Code installed
- Git repository (recommended, not required)
- Time commitment: 60-90 minutes for comprehensive planning

## Configuration

No configuration needed! The plugin works out of the box.

### Optional: Project Coding Standards

Create `.claude/AGENTS.md` or `CLAUDE.md` to document project-specific conventions:

```markdown
# Project Coding Standards

## Architecture
- Use layered architecture: Controller → Service → Repository
- All database access via Prisma ORM

## Testing
- Co-locate test files with source
- Use Jest for unit tests
- 80% coverage minimum

## Naming
- Components: PascalCase
- Functions: camelCase
- Files: kebab-case

[... more conventions ...]
```

The code-rule-reader agent will automatically discover and apply these standards.

## Output Files

### Primary Output

- `.claude/SPEC.md` - Engineering Specification (single source of truth)

### Optional (if in plan mode)

- `.claude/plans/*.md` - Implementation plan file (references SPEC.md)

## Integration with Claude Code

### With Plan Mode

If you run `/senior-planning` while in plan mode:
- SPEC.md provides the **design** (what and why)
- Plan file provides the **execution** (how and when)
- Both work together

### Standalone

If you run `/senior-planning` outside plan mode:
- SPEC.md is the primary deliverable
- Contains both design and implementation phases
- Can create plan file later if needed

## Troubleshooting

### "Plugin not found"

```bash
# Verify installation
claude plugin list

# Reinstall if needed
claude plugin install ./senior-planner --scope user
```

### "Agents not triggering"

Agents trigger based on descriptions. Make sure you:
- Use `/senior-planning` to start (not individual agent names)
- Let the orchestration skill manage agent delegation

### "Interview taking too long"

- Interviews are thorough by design (60-90 min typical)
- Can't skip interviews - each builds on previous
- Time investment prevents bugs and rework

### "Too many questions"

- Each interviewer asks 3-4 questions per round, multiple rounds
- Questions are necessary for comprehensive spec
- Your answers drive better implementation
- Skip if feature is truly simple (use regular planning)

## Examples

### Example 1: Authentication Feature

```
Input: /senior-planning Add OAuth2 social login
Output: SPEC.md with:
- OAuth flow architecture
- Token management strategy
- Security considerations (CSRF, state validation)
- 30 test cases for auth flows
- Integration with Google/GitHub/Facebook
- Deployment strategy (feature flags)
```

### Example 2: Payment Processing

```
Input: /senior-planning Add Stripe payment processing
Output: SPEC.md with:
- Payment flow (cart → checkout → confirm → receipt)
- Stripe API integration details
- PCI compliance considerations
- Webhook handling for async events
- 40 test cases including edge cases
- Idempotency and retry logic
- Rollback strategy for failed payments
```

### Example 3: Real-time Chat

```
Input: /senior-planning Add real-time chat feature
Output: SPEC.md with:
- WebSocket architecture
- Message persistence strategy
- Scaling considerations (Redis pub/sub)
- Typing indicators, read receipts
- 35 test cases for messaging scenarios
- Performance targets (< 50ms message delivery)
- Deployment strategy (gradual WebSocket rollout)
```

## Contributing

Contributions welcome! Areas for improvement:

- Additional specialized interviewers (e.g., Database Architect, DevOps Engineer)
- Enhanced test coverage verification
- Integration with CI/CD for test execution
- Support for additional project conventions files

## License

MIT License

## Acknowledgments

Built on Claude Code's agent and skill system, leveraging:
- Agent delegation for specialized interviews
- AskUserQuestion for user participation
- Sequential workflow orchestration
- TodoWrite for progress tracking

## Version History

### v1.0.0 (Current)
- Initial release
- 9 specialized interviewer agents
- TDD test specification with RED→GREEN→REFACTOR guides
- Test coverage verification
- Project coding standards discovery
- Complete Engineering Spec output

---

**Philosophy**: "Measure twice, cut once" - Comprehensive planning enables confident, bug-free implementation.

Ready to plan your next feature? Run `/senior-planning` and let the senior team guide you! 🚀
