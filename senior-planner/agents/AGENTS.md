# PROJECT KNOWLEDGE BASE - AGENTS

**Generated**: 2025-01-16
**Location**: `senior-planner/agents/`

## OVERVIEW
Collection of 11 specialized senior engineer interviewer agents that build a comprehensive engineering specification through domain-specific interviews. Task classification (Phase 1.8) determines optimal interview plan, skipping irrelevant interviews to save 20-40 minutes while maintaining quality.

## STRUCTURE
- **Discovery**: `code-rule-reader.md` (standards), `tech-interviewer.md` (architecture)
- **Orchestration**: `task-classifier.md` (execution planning) ← NEW
- **Interface**: `ux-interviewer.md` (workflows/UI) [CONDITIONAL]
- **Compliance**: `security-interviewer.md` (auth/protection) [MANDATORY]
- **Quality**: `tdd-test-engineer.md` (TDD/test cases), `test-coverage-verifier.md` (validation) [MANDATORY]
- **Operations**: `performance-interviewer.md` (SLOs) [CONDITIONAL], `integration-interviewer.md` (deployment) [CONDITIONAL]
- **Synthesis**: `spec-writer.md` (SPEC.md generation), `implementation-planner.md` (execution plan) [MANDATORY]
- **Learning**: `wrap-agent.md` (knowledge capture)

## WHERE TO LOOK
- **Agent Definitions**: `agents/*.md` (Markdown files with behavior instructions)
- **Orchestration**: `skills/senior-planning/SKILL.md` (Controls sequential execution)
- **User Interaction**: Look for `AskUserQuestion` patterns within each interviewer agent

## CONVENTIONS
- **Interview-Driven**: Agents probe for requirements using iterative question rounds
- **Specific Options**: Always provide 3-4 concrete technical choices with trade-offs
- **Deep Questioning**: Avoid yes/no questions; focus on rationale, scale, and edge cases
- **Sequential Context**: Later agents (e.g., TDD) depend on output from earlier ones (e.g., Tech/UX)
- **Discovery First**: `code-rule-reader` must run before other agents to find project-specific rules

## ANTI-PATTERNS
- **Shallow Interviewing**: Stopping after one round of questions without probing trade-offs
- **Manual Skipping**: Skipping agents without classification analysis (use `task-classifier` to determine optimal plan)
- **Over-Skipping**: Skipping mandatory agents (tech, security, TDD, coverage, spec-writer, implementation-planner are always required)
- **Skipping Classification**: Running all interviews without first using `task-classifier` to optimize (wastes time on irrelevant questions)
- **Assuming Standards**: Implementing without discovering existing patterns via `code-rule-reader`
- **Isolation**: Interviewers must reference previous summaries to maintain feature cohesion
