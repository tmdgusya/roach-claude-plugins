---
description: Launch comprehensive senior planning interview process with specialized engineering team
argument-hint: [feature description]
---

# Senior Planning Command

You are starting the **Senior Planning** workflow with a team of 11 specialized senior engineers.

## User's Feature Request

Feature: $ARGUMENTS

## Your Mission

Orchestrate a comprehensive planning interview across all engineering domains to produce a complete Engineering Specification with TDD test plans.

## Team Available

1. **Code Rule Reader** - Discovers project coding standards (runs in parallel during discovery)
2. **Task Classifier** (NEW) - Analyzes feature type and determines optimal interview plan
3. **Tech Interviewer** - Technical architecture & system design (mandatory)
4. **UX Interviewer** - User experience & interface design (conditional)
5. **Security Interviewer** - Security, compliance, & data protection (mandatory)
6. **TDD Test Engineer** - Test strategy & comprehensive test cases (mandatory)
7. **Test Coverage Verifier** - Validates test completeness (mandatory)
8. **Performance Interviewer** - Scalability & performance optimization (conditional)
9. **Integration Interviewer** - External dependencies & deployment (conditional)
10. **Spec Writer** - Synthesizes all findings into SPEC.md (mandatory)
11. **Implementation Planner** - Converts SPEC.md into explicit execution plan (mandatory)
12. **Wrap Agent** - Captures session learnings (invoke when done or user says "wrap up")

## Workflow Instructions

Follow the **senior-planning skill** workflow exactly. The skill is located at `skills/senior-planning/SKILL.md`.

### Key Steps:

**Phase 0: Setup**
- Create TodoWrite to track progress through all phases
- Set feature context: $ARGUMENTS

**Phase 1: Discovery (5 min)**
- Understand the feature request
- Explore codebase for similar patterns
- **In parallel**: Launch `code-rule-reader` agent to discover coding standards

**Phase 1.8: Task Classification (5 min)** ← NEW
- Launch `task-classifier` agent
- Analyzes feature type (Backend / Frontend / Full-Stack / Infrastructure)
- Determines which interviews are needed
- Generates optimized execution plan
- Updates TodoWrite to reflect agents being run

**Phase 2-9: Conditional Interviews**
Launch agents based on classification plan:
1. `tech-interviewer` (15-20 min) [MANDATORY]
2. `ux-interviewer` (10-15 min) [CONDITIONAL - skipped for backend-only]
3. `security-interviewer` (5-10 min) [MANDATORY - brief or full]
4. `tdd-test-engineer` (20-30 min) [MANDATORY]
5. `test-coverage-verifier` (10-15 min) [MANDATORY]
6. `performance-interviewer` (10 min) [CONDITIONAL - skipped if no scale concerns]
7. `integration-interviewer` (10 min) [CONDITIONAL - skipped if no external deps]
8. `spec-writer` (10 min) [MANDATORY]
9. `implementation-planner` (10-15 min) [MANDATORY]

**Phase 10: Deliver**
- Present comprehensive summary
- Show SPEC.md + IMPLEMENTATION_PLAN.md locations
- Explain next steps (start TDD implementation)

## Important Guidelines

### Sequential Execution
- Agents run **one at a time** (except code-rule-reader which runs during discovery)
- Each agent receives context from all previous agents
- Update TodoWrite after each phase completes

### User Engagement
- Users actively participate via AskUserQuestion
- Each interviewer asks 3-4 questions per round, multiple rounds
- User decisions shape the final spec

### Quality Standards
- Non-obvious questions (edge cases, trade-offs, scale)
- In-depth exploration until domain fully specified
- No assumptions - ask rather than guess
- Complete, implementation-ready specification

### Time Commitment
- **With Classification Optimization**: 45-90 minutes (depends on task type)
  - Simple backend-only: 45-65 min (skips UX, perf, integration)
  - Simple frontend-only: 50-75 min (skips integration, may skip perf)
  - Full-stack features: 75-90 min (runs most/all agents)
- **Time Savings**: 20-40 min for clearly typed features by skipping irrelevant interviews
- This is comprehensive planning, not quick planning
- Time investment prevents bugs and rework

## How to Use Task Tool

For each agent, use:
```
Task(subagent_type='[agent-name]', description='[5-word summary]', prompt='[detailed instructions with context]')
```

Example:
```
Task(
  subagent_type='tech-interviewer',
  description='Technical architecture interview',
  prompt='Conduct comprehensive technical architecture interview for [feature].

  Context: [discovery summary]

  Ask about system design, data models, API contracts, technology stack.
  Continue until complete technical understanding.
  Produce structured technical summary.'
)
```

## Success Criteria

✅ Task type classified and optimal interview plan created
✅ All mandatory domain interviews completed (tech, security, TDD, coverage, spec, implementation-planner)
✅ Conditional interviews run based on feature type (UX, perf, integration)
✅ Test specifications comprehensive with TDD guides
✅ Test coverage verified against user intent
✅ SPEC.md created with all sections (detailed or brief based on classification)
✅ IMPLEMENTATION_PLAN.md created with explicit execution steps
✅ User confirms spec matches their vision
✅ Ready for TDD implementation

## Output

**Primary**: `.claude/SPEC.md` - Complete Engineering Specification

**Includes**:
- Problem Statement
- Solution Design
- API/Interface Design
- Data Models
- User Experience
- Technical Implementation
- Security & Compliance
- Performance & Scalability
- Test Specification (with TDD guides)
- Integration & Deployment
- Trade-offs & Alternatives
- Implementation Phases
- Risks & Mitigations
- References

## When User Says "WRAP UP"

If user says "wrap up", "WRAP UP", or session is complete:
- Launch `wrap-agent` to capture session learnings
- Wrap agent analyzes session, identifies documentation gaps
- Generates recommendations for updating AGENTS.md/CLAUDE.md
- Ensures code consistency for future sessions

## Start Now

Begin Phase 0: Create TodoWrite and start discovery phase.

Feature to plan: $ARGUMENTS

Let's build a comprehensive specification! 🚀
