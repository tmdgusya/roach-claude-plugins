# PROJECT KNOWLEDGE BASE - AGENTS

**Generated**: 2025-01-16
**Location**: `senior-planner/agents/`

## OVERVIEW
Collection of 10 specialized senior engineer interviewer agents that build a comprehensive engineering specification through domain-specific interviews.

## STRUCTURE
- **Discovery**: `code-rule-reader.md` (standards), `tech-interviewer.md` (architecture)
- **Interface**: `ux-interviewer.md` (workflows/UI)
- **Compliance**: `security-interviewer.md` (auth/protection)
- **Quality**: `tdd-test-engineer.md` (TDD/test cases), `test-coverage-verifier.md` (validation)
- **Operations**: `performance-interviewer.md` (SLOs), `integration-interviewer.md` (deployment)
- **Synthesis**: `spec-writer.md` (SPEC.md generation), `wrap-agent.md` (knowledge capture)

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
- **Skipping Domains**: Each interviewer covers a critical risk area; skipping leads to incomplete specs
- **Assuming Standards**: Implementing without discovering existing patterns via `code-rule-reader`
- **Isolation**: Interviewers must reference previous summaries to maintain feature cohesion
