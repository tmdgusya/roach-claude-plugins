# Decision Table Template

Use this template when documenting architectural or design decisions.

## Template Structure

```markdown
### Decision: [Decision Area]

**Problem**: [What decision needs to be made?]

**Options Considered**:
1. **[Option A]**: [Description]
   - Pros: [Advantages]
   - Cons: [Disadvantages]
   - Use case: [When this is best]

2. **[Option B]**: [Description]
   - Pros: [Advantages]
   - Cons: [Disadvantages]
   - Use case: [When this is best]

3. **[Option C]**: [Description]
   - Pros: [Advantages]
   - Cons: [Disadvantages]
   - Use case: [When this is best]

**Choice**: [Option B]

**Rationale**: [Why this option? What trade-offs are acceptable? What specific project constraints led to this choice?]

**Alternative Not Chosen**: [Option A]

**Why Not**: [Specific reason this alternative wasn't suitable for this project/feature]

**Implementation Notes**:
- [Specific guidance for implementing this decision]
- [File locations where this pattern should be used]
- [Naming conventions to follow]
- [Related patterns or dependencies]
- [Common pitfalls to avoid]

**Code Example**:
```[language]
// Example demonstrating this pattern in action
// Should be 5-15 lines showing the core pattern usage
// Include comments explaining key aspects

[Concrete code example here]
```
\`\`\`
```

---

## Example: Data Access Pattern

```markdown
### Decision: Data Access Layer Pattern

**Problem**: How should the application interact with the database? We need to choose a pattern that balances simplicity, testability, and maintainability.

**Options Considered**:
1. **Repository Pattern with Domain Entities**
   - Pros: Clean separation between domain and persistence, highly testable, follows DDD principles
   - Cons: More files and boilerplate, learning curve for team
   - Use case: Complex domain logic, multiple data sources, need for high testability

2. **Active Record Pattern**
   - Pros: Simple and intuitive, less boilerplate, faster initial development
   - Cons: Couples domain logic to database, harder to test, tight coupling
   - Use case: Simple CRUD applications, rapid prototyping, small team

3. **Data Mapper Pattern**
   - Pros: Complete separation of concerns, maximum flexibility
   - Cons: Most boilerplate, complex setup, overkill for simple apps
   - Use case: Enterprise applications, multiple persistence strategies

**Choice**: Repository Pattern with Domain Entities

**Rationale**:
- Project has complex business logic that benefits from domain isolation
- Team values testability and has experience with dependency injection
- We anticipate adding caching and read replicas, which Repository pattern supports well
- Trade-off: More initial setup time is acceptable for long-term maintainability

**Alternative Not Chosen**: Active Record Pattern

**Why Not**: While Active Record would be faster initially, the coupling between domain and database would make it harder to introduce caching, switch ORMs, or write isolated unit tests. Our domain logic is complex enough to warrant separation.

**Implementation Notes**:
- Create repository interfaces in `src/domain/repositories/`
- Implement concrete repositories in `src/infrastructure/repositories/`
- Use dependency injection to provide repositories to services
- Each repository should handle a single aggregate root
- Keep domain entities in `src/domain/entities/` - they should have NO database awareness
- Follow naming: `UserRepository`, `OrderRepository` (not `UserRepo` or `UserDAO`)

**Code Example**:
```typescript
// src/domain/repositories/UserRepository.ts
export interface UserRepository {
  findById(id: string): Promise<User | null>;
  findByEmail(email: string): Promise<User | null>;
  save(user: User): Promise<void>;
  delete(id: string): Promise<void>;
}

// src/infrastructure/repositories/PrismaUserRepository.ts
export class PrismaUserRepository implements UserRepository {
  constructor(private prisma: PrismaClient) {}

  async findById(id: string): Promise<User | null> {
    const data = await this.prisma.user.findUnique({ where: { id } });
    return data ? this.toDomain(data) : null;
  }

  async save(user: User): Promise<void> {
    await this.prisma.user.upsert({
      where: { id: user.id },
      create: this.toPersistence(user),
      update: this.toPersistence(user)
    });
  }

  private toDomain(data: PrismaUser): User {
    return new User(data.id, data.email, data.name);
  }

  private toPersistence(user: User): PrismaUserData {
    return { id: user.id, email: user.email, name: user.name };
  }
}

// src/services/UserService.ts
export class UserService {
  constructor(private userRepo: UserRepository) {}

  async createUser(email: string, name: string): Promise<User> {
    const user = new User(generateId(), email, name);
    await this.userRepo.save(user);
    return user;
  }
}
```
\`\`\`
```

---

## Guidelines

1. **Be Specific**: Don't say "better performance" - say "reduces database queries from 10 to 2 per request"

2. **Show Trade-offs**: Every choice has downsides - be honest about them

3. **Provide Context**: Explain why this decision fits THIS project (not just generally good)

4. **Include Code**: Abstract explanations aren't enough - show the pattern in action

5. **Guide Implementation**: Tell developers WHERE to put files and WHAT to name things

6. **Anticipate Questions**: If a developer might ask "why not X?", proactively answer it

---

## When to Use This Template

Use decision tables for:
- ✅ Architectural patterns (Repository vs Active Record, MVC vs Clean Architecture)
- ✅ Technology choices (PostgreSQL vs MongoDB, REST vs GraphQL)
- ✅ Design patterns (Singleton vs Dependency Injection, Factory vs Builder)
- ✅ Data structures (Array vs Set, SQL vs NoSQL schema design)
- ✅ Algorithm choices (Binary search vs Hash map, Eager vs Lazy loading)

Don't use for:
- ❌ Trivial choices (variable names, trivial helper functions)
- ❌ Choices with only one viable option
- ❌ Implementation details that don't affect architecture

---

**Purpose**: This template ensures that cheaper models like GLM 4.7 have explicit guidance on architectural decisions, complete with rationale and concrete code examples, eliminating the need for high-level reasoning during implementation.
