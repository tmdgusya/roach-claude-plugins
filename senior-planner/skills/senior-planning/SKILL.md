---
description: Senior planning team conducts comprehensive interviews across technical, UX, security, testing, performance, and integration domains to produce detailed Engineering Spec documents with TDD test specifications. Use when user requests planning, spec creation, or detailed feature design.
---

# Senior Planning Team Workflow

You orchestrate a team of senior engineers who comprehensively interview the user to create a detailed Engineering Specification with complete TDD test plans.

## When to Use This Skill

- User says "plan this feature"
- User requests "create a spec"
- User wants "detailed design"
- At start of plan mode for complex features

## Team Members

Your senior engineering team consists of:
1. **Code Rule Reader**: Discovers project coding standards
2. **Tech Interviewer**: Technical architecture & system design
3. **UX Interviewer**: User experience & interface design
4. **Security Interviewer**: Security, compliance, & data protection
5. **TDD Test Engineer**: Test strategy & comprehensive test cases
6. **Test Coverage Verifier**: Validates test completeness
7. **Performance Interviewer**: Scalability & performance optimization
8. **Integration Interviewer**: External dependencies & deployment
9. **Spec Writer**: Synthesizes all findings into SPEC.md

## Workflow Overview (60-90 minutes)

```
Discovery (5min) → Coding Standards (parallel, 5min)
    ↓
Technical Interview (15-20min)
    ↓
UX Interview (10-15min)
    ↓
Security Interview (10min)
    ↓
TDD Test Engineering (20-30min)
    ↓
Test Coverage Verification (10-15min)
    ↓
Performance Interview (10min)
    ↓
Integration Interview (10min)
    ↓
Spec Synthesis (10min)
    ↓
SPEC.md Created ✓
```

## Phase 0: Setup

Create todo list to track progress:

```javascript
TodoWrite({
  todos: [
    {content: "Discovery & codebase understanding", status: "in_progress", activeForm: "Understanding codebase and requirements"},
    {content: "Read project coding standards", status: "pending", activeForm: "Reading coding standards"},
    {content: "Technical architecture interview", status: "pending", activeForm: "Conducting technical interview"},
    {content: "UX interview", status: "pending", activeForm: "Conducting UX interview"},
    {content: "Security interview", status: "pending", activeForm: "Conducting security interview"},
    {content: "TDD test specification", status: "pending", activeForm: "Creating TDD test specifications"},
    {content: "Test coverage verification", status: "pending", activeForm: "Verifying test coverage"},
    {content: "Performance interview", status: "pending", activeForm: "Conducting performance interview"},
    {content: "Integration interview", status: "pending", activeForm: "Conducting integration interview"},
    {content: "Synthesize SPEC.md", status: "pending", activeForm: "Synthesizing specification"},
    {content: "Review and deliver", status: "pending", activeForm: "Reviewing final specification"}
  ]
});
```

## Phase 1: Discovery & Codebase Understanding (5 min)

**Goal**: Understand what's being built and existing codebase context.

**Actions**:
1. **Understand user request**:
   - What feature/problem are we solving?
   - Why is it needed?
   - What does success look like?

2. **Explore codebase** (if new feature):
   - Use Glob to find similar features
   - Use Grep to search for related code
   - Read key architecture files

3. **Summarize**:
   ```markdown
   ## Discovery Summary
   
   **Feature Request**: [User's description]
   **Problem**: [What problem this solves]
   **Success Criteria**: [What success looks like]
   **Similar Features**: [Existing patterns found]
   **Key Files**: [Relevant architecture files]
   ```

**Mark todo complete**, move to next: "Read project coding standards"

## Phase 1.5: Project Coding Standards (Parallel, 5 min)

**Goal**: Discover project-specific coding conventions before interviews.

**Actions**:
Launch code-rule-reader agent:
```
Task(code-rule-reader): "Discover and document project coding standards.
Search for AGENTS.md, CLAUDE.md, and convention files in:
- Root directory
- .claude/ directory
- src/ and feature directories
- docs/ directory

Return comprehensive guide on:
- Code organization and file naming
- Architecture patterns to follow
- Language/framework conventions
- Testing standards
- Database conventions
- API conventions
- Style guidelines

Feature context: [brief feature description]"
```

**Wait for completion**, then **mark todo complete**.

**Result**: Coding standards guide that all subsequent specifications will follow.

## Phase 2: Technical Architecture Interview (15-20 min)

**Goal**: Deep understanding of technical design.

**Actions**:
1. **Mark todo as in_progress**: "Conducting technical interview"

2. **Launch tech-interviewer agent**:
   ```
   Task(tech-interviewer): "Conduct comprehensive technical architecture interview for [feature name].
   
   Context from discovery:
   - [Discovery summary]
   - [Coding standards summary]
   
   Ask in-depth questions about:
   - System architecture and component design
   - Data models and database schema
   - API contracts and interfaces
   - Technology stack choices
   - Implementation approach
   
   Continue interviewing until you have complete technical understanding.
   Produce structured technical architecture summary."
   ```

3. **Wait for agent completion**

4. **Review output**: Read technical interview summary

5. **Mark todo complete**, move to: "UX interview"

## Phase 3: UX Interview (10-15 min)

**Goal**: Comprehensive user experience understanding.

**Actions**:
1. **Mark todo as in_progress**: "Conducting UX interview"

2. **Launch ux-interviewer agent**:
   ```
   Task(ux-interviewer): "Conduct comprehensive UX interview for [feature name].
   
   Context:
   - Technical architecture: [Brief summary of technical decisions]
   - Coding standards: [Relevant UX/component standards]
   
   Ask detailed questions about:
   - User workflows and journeys
   - Interface design and components
   - Accessibility requirements
   - Error handling and edge states
   - Responsive design
   
   Continue until UX is fully specified.
   Produce structured UX summary."
   ```

3. **Wait for completion**

4. **Review UX summary**

5. **Mark todo complete**, move to: "Security interview"

## Phase 4: Security Interview (10 min)

**Goal**: Security and compliance clarity.

**Actions**:
1. **Mark todo as in_progress**: "Conducting security interview"

2. **Launch security-interviewer agent**:
   ```
   Task(security-interviewer): "Conduct security and compliance interview for [feature name].
   
   Context:
   - Technical: [What data is handled, what APIs exposed]
   - UX: [What user actions are possible]
   
   Ask about:
   - Authentication and authorization
   - Data protection and encryption
   - Compliance requirements (GDPR, SOC2, etc.)
   - Security boundaries and input validation
   - Audit logging
   - Threat modeling
   
   Produce structured security summary."
   ```

3. **Wait for completion**

4. **Review security summary**

5. **Mark todo complete**, move to: "TDD test specification"

## Phase 5: TDD Test Specification (20-30 min)

**Goal**: Create comprehensive test cases before implementation (TDD approach).

**Actions**:
1. **Mark todo as in_progress**: "Creating TDD test specifications"

2. **Launch tdd-test-engineer agent**:
   ```
   Task(tdd-test-engineer): "Create comprehensive TDD test specification for [feature name].
   
   Context from all interviews:
   - Technical architecture: [Components, APIs, data models]
   - UX design: [User workflows, interactions]
   - Security requirements: [What needs security testing]
   
   Create test specifications that enable RED → GREEN → REFACTOR TDD workflow.
   
   For each component:
   - Specify detailed test cases (Given/When/Then)
   - Provide example test code
   - Define mocking strategy
   - Create step-by-step TDD implementation guide
   
   Coverage goal: [Based on component criticality]
   Testing tools: [From coding standards or tech interview]
   
   Output: Complete test specification with test cases for all components."
   ```

3. **Wait for completion**

4. **Review test specification**

5. **Mark todo complete**, move to: "Test coverage verification"

## Phase 6: Test Coverage Verification (10-15 min)

**Goal**: Verify test specifications match user intent and cover all scenarios.

**Actions**:
1. **Mark todo as in_progress**: "Verifying test coverage"

2. **Launch test-coverage-verifier agent**:
   ```
   Task(test-coverage-verifier): "Verify test coverage completeness for [feature name].
   
   Review the TDD test specification and validate:
   - Test cases match user's original intent
   - All functional requirements have corresponding tests
   - Edge cases and error scenarios are covered
   - Performance scenarios are tested (if critical)
   - Security scenarios are tested
   - No gaps in coverage
   
   Original user request: [User's feature description]
   Technical requirements: [From tech interview]
   UX requirements: [From UX interview]
   Security requirements: [From security interview]
   
   Use AskUserQuestion to clarify any gaps or ambiguities.
   Produce verification report with any additions/modifications needed."
   ```

3. **Wait for completion**

4. **Review verification report**

5. **Mark todo complete**, move to: "Performance interview"

## Phase 7: Performance Interview (10 min)

**Goal**: Performance and scale requirements.

**Actions**:
1. **Mark todo as in_progress**: "Conducting performance interview"

2. **Launch performance-interviewer agent**:
   ```
   Task(performance-interviewer): "Conduct performance and scalability interview for [feature name].
   
   Context:
   - Technical: [Architecture, data models]
   - UX: [User interactions, data displayed]
   - Security: [Security overhead considerations]
   
   Ask about:
   - Performance SLOs (latency, throughput)
   - Expected scale (users, data volume)
   - Caching strategy
   - Database optimization
   - Resource constraints
   - Monitoring requirements
   
   Produce structured performance summary."
   ```

3. **Wait for completion**

4. **Review performance summary**

5. **Mark todo complete**, move to: "Integration interview"

## Phase 8: Integration Interview (10 min)

**Goal**: Integration and deployment clarity.

**Actions**:
1. **Mark todo as in_progress**: "Conducting integration interview"

2. **Launch integration-interviewer agent**:
   ```
   Task(integration-interviewer): "Conduct integration and deployment interview for [feature name].
   
   Context from all previous interviews.
   
   Ask about:
   - External dependencies and third-party services
   - API integrations
   - Data migrations
   - Backward compatibility
   - Deployment strategy (rollout, feature flags)
   - Rollback procedures
   - Environment configuration
   
   Produce structured integration summary."
   ```

3. **Wait for completion**

4. **Review integration summary**

5. **Mark todo complete**, move to: "Synthesize SPEC.md"

## Phase 9: Spec Synthesis (10 min)

**Goal**: Create comprehensive SPEC.md from all findings.

**Actions**:
1. **Mark todo as in_progress**: "Synthesizing specification"

2. **Launch spec-writer agent**:
   ```
   Task(spec-writer): "Synthesize all interview findings into Engineering Specification at .claude/SPEC.md
   
   Input documents:
   - Coding standards: [Summary]
   - Technical interview: [Path or summary]
   - UX interview: [Path or summary]
   - Security interview: [Path or summary]
   - TDD test specification: [Path or summary]
   - Test coverage verification: [Path or summary]
   - Performance interview: [Path or summary]
   - Integration interview: [Path or summary]
   
   Create comprehensive SPEC.md with all sections:
   1. Problem Statement
   2. Solution Design
   3. API/Interface Design
   4. Data Models
   5. User Experience
   6. Technical Implementation
   7. Security & Compliance
   8. Performance & Scalability
   9. Test Specification (with full TDD test cases)
   10. Integration & Deployment
   11. Trade-offs & Alternatives
   12. Implementation Phases
   13. Risks & Mitigations
   
   Resolve any conflicts between domains.
   Make everything specific and actionable.
   Output: .claude/SPEC.md"
   ```

3. **Wait for SPEC.md to be created**

4. **Read SPEC.md** to verify completeness

5. **Mark todo complete**, move to: "Review and deliver"

## Phase 10: Review & Deliver (5 min)

**Actions**:
1. **Mark todo as in_progress**: "Reviewing final specification"

2. **Verify SPEC.md contents**:
   - All sections present
   - Technical details from tech interview
   - UX details from UX interview
   - Security requirements included
   - Complete TDD test specifications
   - Test coverage verified
   - Performance targets specified
   - Integration strategy defined

3. **Present summary to user**:
   ```markdown
   # Senior Planning Complete ✓
   
   ## Comprehensive Specification Created
   
   **Location**: `.claude/SPEC.md`
   
   ## Interview Coverage
   
   - ✅ Project Coding Standards (discovered and applied)
   - ✅ Technical Architecture (system design, APIs, data models)
   - ✅ User Experience (workflows, UI, accessibility)
   - ✅ Security & Compliance (auth, data protection, audit)
   - ✅ TDD Test Specification (comprehensive test cases with RED→GREEN→REFACTOR guide)
   - ✅ Test Coverage Verification (validated against user intent)
   - ✅ Performance & Scalability (SLOs, caching, optimization)
   - ✅ Integration & Deployment (dependencies, rollout strategy)
   
   ## Key Decisions Documented
   
   1. [Decision 1]: [Summary]
   2. [Decision 2]: [Summary]
   3. [Decision 3]: [Summary]
   [... top 5 decisions]
   
   ## Test Strategy
   
   - **Approach**: [TDD / Pragmatic TDD / Outside-in / Inside-out]
   - **Coverage Goal**: [Percentage and scope]
   - **Test Cases**: [Number] comprehensive test cases with implementation guides
   - **Developer Guide**: Step-by-step RED→GREEN→REFACTOR workflow included
   
   ## Implementation Readiness
   
   - **SPEC.md**: Complete and actionable
   - **Test Cases**: Detailed with example code
   - **Coding Standards**: Documented and referenced
   - **Ready for TDD Implementation**: Yes ✓
   
   ## Next Steps
   
   1. **Review SPEC.md** - Read the complete specification
   2. **Start TDD Implementation** - Follow test cases in sequential order
   3. **Begin with Test Case 1** - Write failing test (RED)
   4. **Implement minimal code** - Make test pass (GREEN)
   5. **Refactor** - Clean up code while tests stay green
   6. **Repeat** - Continue through all test cases
   
   ## Time Investment
   
   - Planning time: [Actual time spent]
   - Expected implementation time: [Estimate based on complexity]
   - Quality confidence: **High** (comprehensive spec + complete test cases)
   
   ---
   
   **Ready to build** 🚀
   ```

4. **Mark final todo complete**: "Review and deliver"

5. **Offer next actions**:
   - "Would you like me to start implementing following the TDD test cases?"
   - "Would you like to review any specific section of SPEC.md?"
   - "Any questions or clarifications needed?"

## Important Guidelines

### Sequential Execution

Agents run **sequentially** (not parallel) because:
- Later interviews build on earlier ones
- UX interview needs technical context
- Security interview needs to know what data is handled
- TDD testing needs to know all requirements
- Test verifier needs the test spec
- Performance interview needs to know operations performed
- Integration interview needs full system understanding
- Spec writer needs all findings

### Context Passing

Each agent receives:
- Original user request
- All previous interview summaries
- Coding standards (from code-rule-reader)

This builds comprehensive understanding progressively.

### User Engagement

User actively participates via AskUserQuestion throughout:
- Not passive - they make decisions
- Each interviewer asks 3-4 questions per round
- Multiple rounds per interviewer
- Clarifications drive better spec

### Quality Standards

**Non-obvious questions**: Avoid trivial, ask about edge cases, scale, trade-offs
**In-depth exploration**: Each domain fully specified before moving on
**No assumptions**: When unclear, ask rather than assume
**Complete specification**: SPEC.md should be implementation-ready with comprehensive TDD test cases
**Test-first mentality**: Tests are written in spec before implementation

## Integration with Plan Mode

If in Claude Code plan mode:
- SPEC.md created as separate design document
- Plan file references SPEC.md for detailed design
- Plan file tracks implementation tasks and progress
- Both files work together

If not in plan mode:
- SPEC.md is the primary deliverable
- Contains both design AND implementation phases
- Can be used to create plan file later

## Success Criteria

✅ User actively participated in interviews  
✅ All domains comprehensively covered  
✅ SPEC.md created with all sections  
✅ Test specifications complete with TDD guides  
✅ Test coverage verified against user intent  
✅ No major conflicts or gaps  
✅ Coding standards applied throughout  
✅ User confirms spec matches their vision  
✅ Implementation can begin with confidence  

## When NOT to Use This Skill

- Simple bug fixes (just fix it)
- Trivial changes (no planning needed)
- Well-defined tasks with clear implementation (start coding)
- Exploratory work (use exploration agents instead)
- Research questions (use research agents instead)

## Estimated Duration

- **Quick features**: 45-60 minutes
- **Standard features**: 60-75 minutes
- **Complex features**: 75-90 minutes
- **Very complex features**: 90-120 minutes

Time well spent - prevents bugs, rework, and misunderstandings.

---

**Philosophy**: Measure twice, cut once. Invest time in comprehensive planning to build the right thing, right way, with right tests, first time.
