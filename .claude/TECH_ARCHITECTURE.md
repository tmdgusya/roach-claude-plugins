# Technical Architecture: Senior Planner Enhancement for GLM 4.7 Execution

**Date**: 2026-01-16
**Author**: Claude Code Senior Planning Team
**Status**: Architecture Design

---

## 1. Problem Statement

### Background
The senior-planner plugin produces comprehensive Engineering Specifications (SPEC.md) through 10 specialized interviewer agents. However, the output may not be sufficiently explicit for cheaper models like GLM 4.7 to implement production-ready code.

### Goals
- Enable GLM 4.7 to implement features from SPEC.md without high-level reasoning
- Eliminate ambiguous requirements and implicit architectural decisions
- Provide explicit file paths, code examples, and TDD implementation sequences
- Preserve critical details through the spec synthesis process

### Non-Goals
- Changing the sequential interview workflow (works well)
- Replacing human decision-making with automation
- Supporting non-TDD development approaches

---

## 2. Solution Design

### Architecture Overview

```
Current Pipeline:
Discovery → Interviews (x8) → Spec Writer → SPEC.md → [GLM 4.7 struggles here]

Enhanced Pipeline:
Discovery → Interviews (x8) → Spec Writer → Implementation Planner → SPEC.md + IMPLEMENTATION_PLAN.md → [GLM 4.7 succeeds]
                               (enhanced)      (NEW AGENT)
```

### Key Design Decisions

| Decision Area | Choice | Rationale | Alternative | Why Not Alternative |
|---------------|--------|-----------|-------------|---------------------|
| Agent Addition | Add Implementation Planner after spec-writer | Separates concerns: spec-writer synthesizes, planner prescribes | Enhance spec-writer only | Spec-writer is already complex; new agent has focused responsibility |
| Output Format | Structured markdown with required sections | Preserves details while remaining human-readable | JSON schema output | Need human readability for review and approval |
| Decision Format | Decision table + code example template | Makes patterns concrete and actionable | Prose only | GLM 4.7 needs concrete examples, not descriptions |
| Implementation Structure | TDD cycle format with file paths | Aligns with existing TDD focus + adds explicitness | File-level checklist only | TDD cycles ensure test-first workflow |
| Detail Preservation | Required sections + structured extraction | Multi-layer protection against detail loss | Trust spec-writer judgment | User tolerates interview condensation but not spec detail loss |

---

## 3. Component Design

### 3.1 Enhanced Spec-Writer Agent

**Location**: `agents/spec-writer.md`

**Changes**:
1. Add "Required Sections Checklist" that MUST be verified before output
2. Add "Structured Data Extraction" phase to pull decision tables, API examples, file paths
3. Add "Implementation Guidance" section to SPEC.md template

**Required Sections Per Domain**:

**From Tech Interview**:
- ✅ Technical Decisions Table (Decision | Choice | Rationale | Alternative | Code Example)
- ✅ Architecture Diagram (ASCII art)
- ✅ Component Responsibilities (exact role of each)
- ✅ API Signatures (request/response with examples)
- ✅ Data Models (exact schema with types)
- ✅ File Structure (directory tree)
- ✅ Technology Stack Table

**From UX Interview**:
- ✅ User Workflows (step-by-step)
- ✅ UI Component Tree
- ✅ Error States (exact messages)
- ✅ Accessibility Requirements

**From Security Interview**:
- ✅ Security Decisions Table
- ✅ Threat Model (threats + mitigations)
- ✅ Auth/Authorization Rules

**From TDD Interview**:
- ✅ Complete Test Cases (Given/When/Then + code)
- ✅ TDD Workflow Guide (RED→GREEN→REFACTOR)

**From Performance Interview**:
- ✅ Performance SLOs (specific numbers)
- ✅ Caching Strategy (what + where + TTL)

**From Integration Interview**:
- ✅ Dependencies List (library + version + purpose)
- ✅ Deployment Steps (exact sequence)

### 3.2 New Implementation Planner Agent

**Location**: `agents/implementation-planner.md`

**Model**: Opus (requires high-level reasoning to create detailed plans)

**Responsibilities**:
1. **Ambiguity Elimination**: Convert "handle errors appropriately" → "catch DatabaseError, log to logger.error(), return HTTP 500 with {error: 'Database unavailable'}"
2. **File Sequencing**: Create dependency-ordered file creation sequence
3. **TDD Execution Plan**: Detailed RED→GREEN→REFACTOR cycles with exact file paths
4. **Boilerplate Generation**: Provide copy-paste ready code templates

**Input**: `.claude/SPEC.md` (from spec-writer)

**Output**:
- Enhanced sections added to SPEC.md
- Separate `.claude/IMPLEMENTATION_PLAN.md` for step-by-step execution

**Process**:
1. Read SPEC.md thoroughly
2. Identify ambiguities (vague requirements, missing details)
3. Make all implicit decisions explicit
4. Create file-by-file implementation sequence
5. Generate TDD cycles for each component
6. Produce boilerplate code templates
7. Write IMPLEMENTATION_PLAN.md

### 3.3 Decision Table Template (Standard Format)

**Used by**: All interview agents when making architectural/design decisions

**Template Structure**:

```markdown
### Decision: [Decision Area]

**Problem**: [What decision needs to be made?]

**Options Considered**:
1. **[Option A]**: [Description] - [Pros] - [Cons]
2. **[Option B]**: [Description] - [Pros] - [Cons]
3. **[Option C]**: [Description] - [Pros] - [Cons]

**Choice**: [Option B]

**Rationale**: [Why this option? What trade-offs are acceptable?]

**Alternative Not Chosen**: [Option A]
**Why Not**: [Specific reason this alternative wasn't suitable]

**Implementation Notes**:
- [Specific guidance for implementing this decision]
- [File locations, naming conventions, patterns to follow]

**Code Example**:
```typescript
// Example demonstrating this pattern
class UserService {
  // Repository pattern chosen for separation of concerns
  constructor(private userRepo: UserRepository) {}

  async createUser(email: string, password: string): Promise<User> {
    // Validation
    if (!email || !password) {
      throw new ValidationError('Email and password required');
    }

    // Hash password
    const hashedPassword = await bcrypt.hash(password, 10);

    // Save via repository
    const user = await this.userRepo.create({
      email,
      passwordHash: hashedPassword
    });

    return user;
  }
}
```
\`\`\`
```

**Code Example Detail Level**: Signature + core logic (5-15 lines showing pattern usage)

### 3.4 TDD Cycle Format (Standard Format)

**Used by**: Implementation Planner agent when creating execution plan

**Template Structure**:

```markdown
## Component: UserAuthenticationService

**File**: `src/services/UserAuthenticationService.ts`
**Test File**: `src/services/UserAuthenticationService.test.ts`

---

### Cycle 1: User Login - Happy Path

#### RED Phase (Write Failing Test)

**File**: `src/services/UserAuthenticationService.test.ts`

**Test Code**:
```typescript
describe('UserAuthenticationService', () => {
  describe('login', () => {
    it('should return token and user on valid credentials', async () => {
      // Arrange
      const email = 'test@example.com';
      const password = 'Password123!';
      await createTestUser(email, password);

      // Act
      const result = await authService.login(email, password);

      // Assert
      expect(result.success).toBe(true);
      expect(result.token).toMatch(/^eyJ/); // JWT format
      expect(result.user.email).toBe(email);
    });
  });
});
```

**Expected Result**: ❌ Test fails (UserAuthenticationService.login doesn't exist yet)

---

#### GREEN Phase (Make Test Pass)

**File**: `src/services/UserAuthenticationService.ts`

**Implementation Code**:
```typescript
export class UserAuthenticationService {
  constructor(
    private userRepo: UserRepository,
    private passwordHasher: PasswordHasher,
    private tokenGenerator: TokenGenerator
  ) {}

  async login(email: string, password: string): Promise<LoginResult> {
    // Find user
    const user = await this.userRepo.findByEmail(email);
    if (!user) {
      return { success: false, error: 'Invalid credentials' };
    }

    // Verify password
    const isValid = await this.passwordHasher.compare(password, user.passwordHash);
    if (!isValid) {
      return { success: false, error: 'Invalid credentials' };
    }

    // Generate token
    const token = this.tokenGenerator.generate({ userId: user.id });

    // Return success
    return {
      success: true,
      token,
      user: { id: user.id, email: user.email, name: user.name }
    };
  }
}
```

**Expected Result**: ✅ Test passes

---

#### REFACTOR Phase (Improve Code)

**Improvements**:
1. Extract user serialization to separate method
2. Add logging for failed login attempts
3. Add JSDoc documentation

**Refactored Code**:
```typescript
export class UserAuthenticationService {
  constructor(
    private userRepo: UserRepository,
    private passwordHasher: PasswordHasher,
    private tokenGenerator: TokenGenerator,
    private logger: Logger
  ) {}

  /**
   * Authenticate user with email and password
   * @returns LoginResult with token on success, error on failure
   */
  async login(email: string, password: string): Promise<LoginResult> {
    const user = await this.userRepo.findByEmail(email);
    if (!user) {
      this.logger.warn('Login failed: user not found', { email });
      return this.loginError('Invalid credentials');
    }

    const isValid = await this.passwordHasher.compare(password, user.passwordHash);
    if (!isValid) {
      this.logger.warn('Login failed: invalid password', { email });
      return this.loginError('Invalid credentials');
    }

    const token = this.tokenGenerator.generate({ userId: user.id });

    return {
      success: true,
      token,
      user: this.serializeUser(user)
    };
  }

  private loginError(message: string): LoginResult {
    return { success: false, error: message };
  }

  private serializeUser(user: User): UserDTO {
    return {
      id: user.id,
      email: user.email,
      name: user.name
    };
  }
}
```

**Expected Result**: ✅ Test still passes, code is cleaner

---

### Cycle 2: User Login - Invalid Password

[Continue with next RED→GREEN→REFACTOR cycle...]
```

---

## 4. File Structure Changes

```
senior-planner/
├── agents/
│   ├── code-rule-reader.md          [Existing]
│   ├── tech-interviewer.md          [ENHANCE: Add decision table requirements]
│   ├── ux-interviewer.md            [ENHANCE: Add required sections]
│   ├── security-interviewer.md      [ENHANCE: Add decision table requirements]
│   ├── tdd-test-engineer.md         [ENHANCE: Add implementation notes between tests]
│   ├── test-coverage-verifier.md    [Existing - no changes needed]
│   ├── performance-interviewer.md   [ENHANCE: Add required sections]
│   ├── integration-interviewer.md   [ENHANCE: Add required sections]
│   ├── spec-writer.md               [MAJOR ENHANCE: Add required sections checklist]
│   ├── implementation-planner.md    [NEW AGENT]
│   └── wrap-agent.md                [Existing]
├── skills/
│   └── senior-planning/
│       └── SKILL.md                 [ENHANCE: Add Implementation Planner phase]
├── templates/                       [NEW FOLDER]
│   ├── decision-table.md            [NEW: Standard decision template]
│   ├── tdd-cycle.md                 [NEW: Standard TDD cycle template]
│   └── spec-template.md             [NEW: SPEC.md template with required sections]
└── README.md                        [UPDATE: Document new workflow]
```

---

## 5. Technical Implementation Details

### 5.1 Spec-Writer Checklist Mechanism

**Implementation**: Add validation section at end of spec-writer agent prompt

```markdown
## Before Output: Verification Checklist

You MUST verify these items are included in SPEC.md:

### Technical Architecture Section
- [ ] Technical Decisions Table (at least 5 major decisions with code examples)
- [ ] Architecture Diagram (ASCII art showing component relationships)
- [ ] Component Responsibilities (each component's exact role)
- [ ] API Signatures (all endpoints with request/response examples)
- [ ] Data Models (complete schema with field types)
- [ ] File Structure (directory tree showing all major files)
- [ ] Technology Stack Table (all libraries with versions and rationale)

### UX Section
- [ ] User Workflows (step-by-step for each major flow)
- [ ] UI Component Tree
- [ ] Error States (exact error messages)

### Security Section
- [ ] Security Decisions Table
- [ ] Threat Model

### Test Specification
- [ ] Complete TDD Test Cases (all with Given/When/Then + code)
- [ ] TDD Workflow Guide

### Performance Section
- [ ] Performance SLOs (specific numbers: <500ms p95, etc.)

### Integration Section
- [ ] Dependencies List (library + version + purpose)

If ANY checklist item is missing from interview summaries:
1. Note the gap in SPEC.md ("## Gap: [Missing Item]")
2. Add reasonable default based on industry standards
3. Mark for user review
```

### 5.2 Implementation Planner Algorithm

**Process**:

```python
# Pseudo-code for Implementation Planner logic

def create_implementation_plan(spec_md):
    # Phase 1: Read and analyze
    components = extract_components(spec_md)
    test_cases = extract_test_cases(spec_md)
    decisions = extract_decisions(spec_md)

    # Phase 2: Identify ambiguities
    ambiguities = []
    for component in components:
        if has_vague_requirements(component):
            ambiguities.append(make_explicit(component))

    # Phase 3: Create file sequence
    dependency_graph = build_dependency_graph(components)
    file_sequence = topological_sort(dependency_graph)

    # Phase 4: Generate TDD cycles
    tdd_cycles = []
    for component in file_sequence:
        relevant_tests = get_tests_for_component(component, test_cases)
        for test in relevant_tests:
            cycle = create_tdd_cycle(
                component=component,
                test=test,
                pattern=get_pattern_from_decisions(decisions, component)
            )
            tdd_cycles.append(cycle)

    # Phase 5: Generate boilerplate
    boilerplate = generate_file_templates(components, decisions)

    # Phase 6: Write outputs
    write_implementation_plan_md(tdd_cycles, file_sequence, boilerplate)
    enhance_spec_md_with_explicit_guidance(ambiguities)

    return {
        "file_sequence": file_sequence,
        "tdd_cycles": tdd_cycles,
        "boilerplate": boilerplate
    }
```

---

## 6. Quality Assurance

### Success Criteria

**For Spec-Writer Output**:
- ✅ All checklist items present in SPEC.md
- ✅ Every major decision has table + code example
- ✅ All API endpoints have request/response examples
- ✅ File structure shows all major files
- ✅ TDD test cases have complete Given/When/Then + code

**For Implementation Planner Output**:
- ✅ No vague requirements (all "handle X" converted to explicit steps)
- ✅ File sequence respects dependencies (types before services, services before controllers)
- ✅ TDD cycles have exact file paths
- ✅ Boilerplate code is copy-paste ready
- ✅ GLM 4.7 can execute plan without architectural decisions

### Validation Method

**Manual Review**:
1. Read SPEC.md and IMPLEMENTATION_PLAN.md
2. Check: Can a junior developer implement without asking questions?
3. Check: Are all patterns demonstrated with code?
4. Check: Are all file paths explicit?

**Automated Validation** (future enhancement):
- Checklist parser to verify all required sections present
- Code example detector to ensure decisions have examples
- File path validator to check paths are absolute

---

## 7. Implementation Phases

### Phase 1: Foundation (Templates + Standards)
1. Create `templates/` folder
2. Create `decision-table.md` template
3. Create `tdd-cycle.md` template
4. Create `spec-template.md` with required sections

### Phase 2: Enhance Existing Agents
1. Update `tech-interviewer.md` with decision table requirements
2. Update `ux-interviewer.md` with required sections
3. Update `security-interviewer.md` with decision table requirements
4. Update `tdd-test-engineer.md` with implementation notes
5. Update `performance-interviewer.md` with explicit SLO format
6. Update `integration-interviewer.md` with required sections

### Phase 3: Enhance Spec-Writer
1. Add Required Sections Checklist to spec-writer.md
2. Add Structured Data Extraction phase
3. Add Implementation Guidance section to output template
4. Update spec-writer to use new SPEC template

### Phase 4: Create Implementation Planner
1. Create `implementation-planner.md` agent
2. Define input (SPEC.md) and outputs (enhanced SPEC.md + IMPLEMENTATION_PLAN.md)
3. Add ambiguity detection logic
4. Add TDD cycle generation
5. Add boilerplate generation
6. Add file sequencing algorithm

### Phase 5: Update Orchestration
1. Update `skills/senior-planning/SKILL.md`
2. Add Implementation Planner phase after Spec Writer
3. Update todo list template
4. Update time estimates (add 10-15 min for Implementation Planner)

### Phase 6: Documentation
1. Update README.md with new workflow
2. Update AGENTS.md with standards
3. Add examples of decision tables and TDD cycles
4. Document validation checklist

### Phase 7: Testing
1. Run enhanced workflow on sample feature
2. Validate SPEC.md has all required sections
3. Validate IMPLEMENTATION_PLAN.md is GLM 4.7 executable
4. Iterate on templates based on results

---

## 8. Risks & Mitigations

| Risk | Impact | Likelihood | Mitigation |
|------|--------|------------|------------|
| SPEC.md becomes too long (>10k lines) | Hard to read/review | Medium | Use separate IMPLEMENTATION_PLAN.md for detailed steps, keep SPEC.md focused on "what/why" |
| Implementation Planner adds 20+ min to workflow | User fatigue, workflow too slow | Medium | Make Implementation Planner optional via flag, only run for complex features |
| Templates feel rigid, stifle creativity | Agents produce formulaic output | Low | Templates are guidelines, not strict requirements; agents can adapt format |
| Structured extraction misses nuance | Important details lost in data extraction | Medium | Keep narrative explanations alongside structured data (hybrid approach) |
| GLM 4.7 still produces poor code | Architecture changes insufficient | Low | Validate with real GLM 4.7 test, iterate on template detail level |

---

## 9. Alternative Approaches Considered

### Alternative A: Enhance Spec-Writer Only (No New Agent)
**Rejected because**: Spec-writer is already complex with synthesis responsibility. Adding implementation planning would overload single agent. Separation of concerns is cleaner.

### Alternative B: JSON Schema Output
**Rejected because**: Loses human readability. Engineers need to review and approve specs, JSON is harder to read than markdown.

### Alternative C: Multiple Output Files (One Per Domain)
**Rejected because**: Fragments information. Single SPEC.md + IMPLEMENTATION_PLAN.md is easier to reference during implementation.

### Alternative D: LLM-Generated Boilerplate Only
**Rejected because**: Doesn't solve ambiguity problem. Still need explicit decisions and TDD cycles, not just code templates.

---

## 10. Success Metrics

**Quantitative**:
- SPEC.md completeness: 100% of required sections present
- Decision coverage: Every major decision has table + code example
- TDD cycle coverage: Every component has RED→GREEN→REFACTOR plan
- File path explicitness: Every file creation has absolute path
- GLM 4.7 execution success rate: >90% of features implemented correctly (measured via test passage)

**Qualitative**:
- Junior developers can implement from plan without questions
- SPEC.md review time <30 minutes (comprehensive but not overwhelming)
- User confidence in delegating to GLM 4.7 increases

---

## 11. Timeline

**Development**: 3-4 hours
- Templates: 30 minutes
- Agent enhancements: 1.5 hours
- New Implementation Planner: 1 hour
- Testing & iteration: 1 hour

**First production use**: After validation on 2-3 sample features

---

## 12. References

- Existing senior-planner plugin architecture (AGENTS.md, README.md)
- TDD test engineer agent (most detailed example)
- User feedback on GLM 4.7 pain points (ambiguity, architectural decisions)

---

**Status**: ✅ Architecture Design Complete - Ready for Implementation
**Next**: Begin Phase 1 (Templates + Standards)
