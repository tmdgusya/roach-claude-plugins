# TDD Cycle Template

Use this template when creating Test-Driven Development implementation plans.

## Template Structure

```markdown
## Component: [ComponentName]

**Purpose**: [What this component does]

**File**: `[exact/path/to/implementation.ts]`

**Test File**: `[exact/path/to/test.ts]`

**Dependencies**: [List external dependencies this component needs]

---

### Cycle [N]: [Feature/Scenario Name]

#### RED Phase (Write Failing Test)

**File**: `[exact/path/to/test.ts]`

**Test Name**: `should [expected behavior] when [condition]`

**Test Code**:
```[language]
describe('[ComponentName]', () => {
  describe('[methodName]', () => {
    it('should [expected behavior] when [condition]', async () => {
      // Arrange - Set up test data and dependencies
      [setup code]

      // Act - Execute the functionality being tested
      [execution code]

      // Assert - Verify the expected outcome
      [assertion code]
    });
  });
});
```

**Expected Result**: ❌ Test fails with message: "[expected error message]"

**Why It Fails**: [Explain why - method doesn't exist, returns wrong value, etc.]

---

#### GREEN Phase (Make Test Pass)

**File**: `[exact/path/to/implementation.ts]`

**Implementation Strategy**: [Brief description of minimal approach to make test pass]

**Implementation Code**:
```[language]
// Minimal implementation to make test pass
[code here - should be focused on passing THIS test only]
```

**Expected Result**: ✅ Test passes

**Verification**: Run `[test command]` and confirm test passes

---

#### REFACTOR Phase (Improve Code)

**Improvements to Make**:
1. [Specific improvement - e.g., "Extract validation logic to separate method"]
2. [Specific improvement - e.g., "Add JSDoc documentation"]
3. [Specific improvement - e.g., "Improve variable names"]

**Refactored Code**:
```[language]
// Improved implementation with better structure, naming, documentation
[refactored code here]
```

**Expected Result**: ✅ Test still passes, code is cleaner

**Verification**: Run `[test command]` and confirm test still passes

---

**What's Next**: [Brief note about what the next cycle will test]

---
```

---

## Complete Example: User Authentication

```markdown
## Component: UserAuthenticationService

**Purpose**: Handle user login, logout, and session management

**File**: `src/services/UserAuthenticationService.ts`

**Test File**: `src/services/UserAuthenticationService.test.ts`

**Dependencies**:
- `UserRepository` (database access)
- `PasswordHasher` (bcrypt wrapper)
- `TokenGenerator` (JWT generation)
- `Logger` (application logging)

---

### Cycle 1: User Login - Valid Credentials

#### RED Phase (Write Failing Test)

**File**: `src/services/UserAuthenticationService.test.ts`

**Test Name**: `should return token and user on valid credentials`

**Test Code**:
```typescript
import { UserAuthenticationService } from './UserAuthenticationService';
import { createTestUser, cleanupDatabase } from '../test/helpers';

describe('UserAuthenticationService', () => {
  let authService: UserAuthenticationService;

  beforeEach(async () => {
    await cleanupDatabase();
    authService = new UserAuthenticationService(
      mockUserRepository,
      mockPasswordHasher,
      mockTokenGenerator,
      mockLogger
    );
  });

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
      expect(result.user).toEqual({
        id: expect.any(String),
        email: 'test@example.com',
        name: expect.any(String)
      });
    });
  });
});
```

**Expected Result**: ❌ Test fails with message: "TypeError: authService.login is not a function"

**Why It Fails**: The `UserAuthenticationService` class doesn't exist yet, and neither does the `login` method.

---

#### GREEN Phase (Make Test Pass)

**File**: `src/services/UserAuthenticationService.ts`

**Implementation Strategy**: Create the class with minimum logic to satisfy test - find user, verify password, generate token, return result.

**Implementation Code**:
```typescript
export interface LoginResult {
  success: boolean;
  token?: string;
  user?: UserDTO;
  error?: string;
}

export interface UserDTO {
  id: string;
  email: string;
  name: string;
}

export class UserAuthenticationService {
  constructor(
    private userRepo: UserRepository,
    private passwordHasher: PasswordHasher,
    private tokenGenerator: TokenGenerator,
    private logger: Logger
  ) {}

  async login(email: string, password: string): Promise<LoginResult> {
    // Find user by email
    const user = await this.userRepo.findByEmail(email);
    if (!user) {
      return { success: false, error: 'Invalid credentials' };
    }

    // Verify password
    const isPasswordValid = await this.passwordHasher.compare(
      password,
      user.passwordHash
    );
    if (!isPasswordValid) {
      return { success: false, error: 'Invalid credentials' };
    }

    // Generate JWT token
    const token = this.tokenGenerator.generate({
      userId: user.id,
      email: user.email
    });

    // Return success result
    return {
      success: true,
      token,
      user: {
        id: user.id,
        email: user.email,
        name: user.name
      }
    };
  }
}
```

**Expected Result**: ✅ Test passes

**Verification**: Run `npm test UserAuthenticationService.test.ts` and confirm green output

---

#### REFACTOR Phase (Improve Code)

**Improvements to Make**:
1. Extract user serialization to separate private method (DRY principle)
2. Add logging for failed login attempts (observability)
3. Add JSDoc documentation for public API
4. Extract error response creation to helper method (consistency)

**Refactored Code**:
```typescript
export interface LoginResult {
  success: boolean;
  token?: string;
  user?: UserDTO;
  error?: string;
}

export interface UserDTO {
  id: string;
  email: string;
  name: string;
}

export class UserAuthenticationService {
  constructor(
    private userRepo: UserRepository,
    private passwordHasher: PasswordHasher,
    private tokenGenerator: TokenGenerator,
    private logger: Logger
  ) {}

  /**
   * Authenticate user with email and password
   *
   * @param email - User's email address
   * @param password - User's plain text password
   * @returns LoginResult with token on success, error message on failure
   *
   * @example
   * const result = await authService.login('user@example.com', 'password123');
   * if (result.success) {
   *   console.log('Logged in:', result.user);
   * }
   */
  async login(email: string, password: string): Promise<LoginResult> {
    const user = await this.userRepo.findByEmail(email);
    if (!user) {
      this.logger.warn('Login failed: user not found', { email });
      return this.createErrorResult('Invalid credentials');
    }

    const isPasswordValid = await this.passwordHasher.compare(
      password,
      user.passwordHash
    );
    if (!isPasswordValid) {
      this.logger.warn('Login failed: invalid password', { userId: user.id });
      return this.createErrorResult('Invalid credentials');
    }

    const token = this.tokenGenerator.generate({
      userId: user.id,
      email: user.email
    });

    this.logger.info('User logged in successfully', { userId: user.id });

    return {
      success: true,
      token,
      user: this.serializeUser(user)
    };
  }

  /**
   * Convert domain User entity to DTO for API responses
   * @private
   */
  private serializeUser(user: User): UserDTO {
    return {
      id: user.id,
      email: user.email,
      name: user.name
    };
  }

  /**
   * Create consistent error result
   * @private
   */
  private createErrorResult(message: string): LoginResult {
    return {
      success: false,
      error: message
    };
  }
}
```

**Expected Result**: ✅ Test still passes, code is more maintainable

**Verification**: Run `npm test UserAuthenticationService.test.ts` - all tests green

---

**What's Next**: Cycle 2 will test the error case - invalid password handling

---

### Cycle 2: User Login - Invalid Password

[Continue with next RED→GREEN→REFACTOR cycle...]

---
```

---

## Guidelines

### RED Phase
1. **Write test FIRST** - never write implementation code before the test
2. **Test should be complete** - use Arrange/Act/Assert pattern
3. **Expected failure is specific** - know exactly what error message you expect
4. **Test ONE thing** - each test should verify a single behavior
5. **Use realistic test data** - avoid `foo/bar/baz`, use domain-appropriate examples

### GREEN Phase
1. **Minimal implementation** - write ONLY enough code to pass THIS test
2. **No premature optimization** - don't add features not covered by current test
3. **Focus on passing** - make it work first, make it clean in REFACTOR
4. **Exact file path** - always specify where code should be written
5. **Verify it passes** - explicitly state the expected outcome

### REFACTOR Phase
1. **Keep tests green** - run tests after each refactoring step
2. **Specific improvements** - list concrete changes, not vague goals
3. **Common refactorings**:
   - Extract duplicate code
   - Improve naming
   - Add documentation
   - Simplify complex logic
   - Add error handling
   - Improve type safety
4. **Show the result** - provide the refactored code, don't just describe it

---

## Cycle Sequencing

### Order cycles by dependency and complexity:

1. **Start with happy path** - simplest success case first
2. **Add error cases** - one error condition per cycle
3. **Add edge cases** - boundary conditions, null/empty values
4. **Add integration** - interaction with external dependencies

### Example sequence for UserAuthenticationService:
1. ✅ Valid login (happy path)
2. ❌ Invalid password
3. ❌ Non-existent user
4. ❌ Locked account
5. ✅ Case-insensitive email
6. ⚠️ Rate limiting (security)
7. ✅ Session creation
8. ⚠️ Concurrent login handling

---

## For Implementers (GLM 4.7)

When following these cycles:

1. **Read the entire cycle** before starting
2. **Follow the exact order**: RED → GREEN → REFACTOR
3. **Copy the test code** from RED phase into the test file
4. **Run the test** - it MUST fail initially
5. **Copy the implementation code** from GREEN phase
6. **Run the test again** - it MUST pass
7. **Apply the refactoring** from REFACTOR phase
8. **Run tests one more time** - ensure still passing
9. **Commit** with message: "feat: [cycle name] - TDD cycle complete"
10. **Move to next cycle**

**Do not**:
- ❌ Skip ahead to implementation without failing test
- ❌ Add features not covered by current test
- ❌ Refactor during GREEN phase
- ❌ Skip REFACTOR phase ("I'll clean it up later" never works)

---

## File Naming Conventions

- **Implementation**: `[ComponentName].ts` (PascalCase)
- **Test file**: `[ComponentName].test.ts` (matches implementation name)
- **Integration test**: `[feature].integration.test.ts`
- **E2E test**: `[feature].e2e.test.ts`

---

## Test Commands

Adjust for your project:

```bash
# Run single test file
npm test UserAuthenticationService.test.ts

# Run in watch mode
npm test -- --watch

# Run with coverage
npm test -- --coverage

# Run specific test by name
npm test -- -t "should return token and user"
```

---

**Purpose**: This template provides explicit RED→GREEN→REFACTOR cycles with exact file paths and complete code examples, enabling cheaper models like GLM 4.7 to implement features using TDD without requiring architectural decision-making during execution.
