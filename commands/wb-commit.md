# wb-commit


## Objective

Ensure that **every commit** follows the **Semantic Commits** pattern (compatible with **semantic-release**) and that commits are made **by context** (grouping by module/layer/folder).
Also **prohibit push to `main` branch** without explicit authorization.

---

## Rule (to paste in Cursor / Rules)

**Suggested name:** `commit-policy-semantic-context`

### 1) Before committing (mandatory)

1. **Read the `AGENTS.md` file** (if it exists) and extract:
   - Convention of layers/folders/modules
   - Naming patterns

2. **Detect the context** from:
   - Project folder structure (e.g., `src/domain/`, `lib/services/`, `Core/Domain/`, `components/`, `api/`, etc.)
   - Namespaces/packages (e.g., `next_CNPJ.Core.Domain`, `com.example.service`)
   - Common patterns of the language/framework

3. **Group changed files by context**:
   - Identify the main folder/module of each file
   - Group by similar context (same parent folder or namespace)

---

### 2) Mandatory commit format (semantic-release)

Each commit message **MUST** follow:

`<type>(<scope>): <subject>`

* **Allowed types:** `feat | fix | docs | style | refactor | perf | test | build | ci | chore | revert`
* **scope:** mandatory and must reflect the detected **context/module**
  - Generic examples: `domain`, `service`, `api`, `db`, `auth`, `utils`, `component`, `model`, `controller`, `validator`
  - Use the main folder/module name (e.g., if file is in `Core/Domain/`, scope is `domain`)
  - For projects without clear structure, use `core` or the main package name
* **subject:** short, objective, no period, in **imperative mood**, **always in English**

**Examples:**
* `feat(domain): add loyalty card entity`
* `fix(service): correct mongo connection retry strategy`
* `docs(api): document auth headers for endpoints`
* `test(validator): add unit tests for CNPJ rules`
* `refactor(utils): simplify normalization logic`

---

### 3) Context-based commits

**Golden rule:** *one commit = one context (when possible)*

**Process:**

1. **Group changes by context/folder/module**
2. **Make separate commits by context**:
   - Order by dependency (innermost layers first, interfaces last)
   - If there's no clear dependency, order alphabetically
3. **Don't mix different types in the same context**:
   - `refactor(domain)` separate from `feat(domain)`
   - `fix(service)` separate from `test(service)`

**Exceptions:**
- If all changes are from the same context and type, can be a single commit
- Formatting/linter changes can be grouped in `style` or `chore`

---

### 4) When to use BREAKING CHANGE

If there's a contract/API break:

* Use `!` in the type: `feat(api)!: rename endpoint for ...`
* And add footer:
  ```
  BREAKING CHANGE: <short explanation in English>
  ```

---

### 5) Mandatory checklist before commit

Before executing the commit, validate:

* [ ] Message is in semantic format (`type(scope): subject`)
* [ ] Scope represents detected context/module
* [ ] Subject is in **English** and in imperative mood
* [ ] Commit files belong to the same context (or are related changes)
* [ ] Doesn't include "accidental" changes (format/linters) mixed unnecessarily

---

### 6) Prohibition: push to `main` without authorization

**Security rules:**

* **NEVER** execute `git push origin main` automatically.
* If the user asks to push to main, **require explicit confirmation** with the phrase:
  * `AUTORIZO PUSH NA MAIN`
* If this phrase doesn't exist, push **only to feature branch**.

**Default behavior:**

1. Create branch following pattern:
   - `feat/<context>-<summary>` (e.g., `feat/domain-loyalty-card`)
   - `fix/<context>-<summary>` (e.g., `fix/service-connection-retry`)
   - `refactor/<context>-<summary>`
2. Push to the branch:
   ```bash
   git push -u origin <branch>
   ```

---

## Expected agent output when preparing commits

Whenever there are changes, the agent must respond with:

1. **Context analysis:**
   - Files grouped by context/module
   - Detected contexts (based on folders/namespaces)

2. **Commit plan:**
   - List of suggested commits in format `type(scope): subject` (all in English)
   - Execution order

3. **Ready commands:**
   - `git add <context paths>`
   - `git commit -m "type(scope): subject"`
   - (repeat for each context)

4. **Explicit warning:**
   - "⚠️ **I will not push to `main` branch without explicit authorization**"
   - Branch suggestion and push command

---

## Mini-template (generate automatically)

**Context analysis:**
- `domain/` → files: `file1.cs`, `file2.cs`
- `service/` → files: `file3.ts`, `file4.ts`

**Commit plan:**
* `feat(domain): add new feature`
* `fix(service): correct bug`

**Execution:**
```bash
git add <context1 paths>
git commit -m "feat(domain): add new feature"

git add <context2 paths>
git commit -m "fix(service): correct bug"
```

**Push:**
```bash
git checkout -b feat/domain-new-feature
git push -u origin feat/domain-new-feature
```

⚠️ **I will not push to `main` branch without explicit authorization**
```

---

## Practical Examples

### Example 1: Multiple Contexts

**Changed files:**
- `src/domain/Order.cs`
- `src/domain/OrderItem.cs`
- `src/services/OrderService.cs`
- `src/api/OrdersController.cs`
- `tests/domain/OrderTests.cs`

**Context analysis:**
- `domain/` → `Order.cs`, `OrderItem.cs`
- `service/` → `OrderService.cs`
- `api/` → `OrdersController.cs`
- `test/` → `OrderTests.cs`

**Commit plan:**
1. `feat(domain): add order and order item entities`
2. `feat(service): implement order processing logic`
3. `feat(api): add orders endpoint`
4. `test(domain): add unit tests for order entity`

**Execution:**
```bash
git add src/domain/Order.cs src/domain/OrderItem.cs
git commit -m "feat(domain): add order and order item entities"

git add src/services/OrderService.cs
git commit -m "feat(service): implement order processing logic"

git add src/api/OrdersController.cs
git commit -m "feat(api): add orders endpoint"

git add tests/domain/OrderTests.cs
git commit -m "test(domain): add unit tests for order entity"
```

### Example 2: Single Context, Multiple Types

**Changed files:**
- `src/services/PaymentService.cs` (new feature)
- `src/services/PaymentService.cs` (bug fix)
- `tests/services/PaymentServiceTests.cs` (new tests)

**Context analysis:**
- `service/` → `PaymentService.cs` (feat + fix)
- `test/` → `PaymentServiceTests.cs`

**Commit plan:**
1. `feat(service): add payment processing with retry logic`
2. `fix(service): correct payment validation error handling`
3. `test(service): add unit tests for payment service`

**Execution:**
```bash
# Commit feature changes
git add src/services/PaymentService.cs
git commit -m "feat(service): add payment processing with retry logic"

# Commit fix separately (even in same file)
git add src/services/PaymentService.cs
git commit -m "fix(service): correct payment validation error handling"

# Commit tests
git add tests/services/PaymentServiceTests.cs
git commit -m "test(service): add unit tests for payment service"
```

### Example 3: Breaking Change

**Changed files:**
- `src/api/UsersController.cs` (renamed endpoint)

**Context analysis:**
- `api/` → `UsersController.cs`

**Commit plan:**
1. `feat(api)!: rename user endpoint from /users to /accounts`

**Execution:**
```bash
git add src/api/UsersController.cs
git commit -m "feat(api)!: rename user endpoint from /users to /accounts

BREAKING CHANGE: The /users endpoint has been renamed to /accounts. 
All clients must update their API calls to use the new endpoint path."
```

### Example 4: Node.js/TypeScript Project

**Changed files:**
- `src/controllers/order.controller.ts`
- `src/services/order.service.ts`
- `src/models/order.model.ts`
- `tests/controllers/order.controller.test.ts`

**Context analysis:**
- `model/` → `order.model.ts`
- `service/` → `order.service.ts`
- `controller/` → `order.controller.ts`
- `test/` → `order.controller.test.ts`

**Commit plan:**
1. `feat(model): add order model with validation`
2. `feat(service): implement order business logic`
3. `feat(controller): add order endpoints`
4. `test(controller): add integration tests for order endpoints`

## Troubleshooting

### Problem: Cannot determine context from file structure

**Solution:**
- Use namespace/package name as scope
- If no clear structure, use `core` as scope
- Check `AGENTS.md` for project conventions

**Example:**
```bash
# If file is in root or unclear structure
git commit -m "feat(core): add utility function for date formatting"
```

### Problem: Mixed changes in same file (feat + fix)

**Solution:**
- Make separate commits for different types
- Use `git add -p` to stage specific changes
- Or commit all changes with the primary type and note in subject

**Example:**
```bash
# Stage only feature changes
git add -p src/services/PaymentService.cs
git commit -m "feat(service): add payment retry logic"

# Stage only fix changes
git add -p src/services/PaymentService.cs
git commit -m "fix(service): correct validation error"
```

### Problem: User wants to push to main

**Solution:**
- Require explicit authorization phrase: `AUTORIZO PUSH NA MAIN`
- If phrase not provided, create feature branch and push there
- Always warn user about main branch protection

**Example:**
```
User: "push to main"

Agent: "⚠️ I cannot push to main branch without explicit authorization.
Please confirm by typing: AUTORIZO PUSH NA MAIN

Alternatively, I can create a feature branch and push there:
git checkout -b feat/your-feature
git push -u origin feat/your-feature"
```

### Problem: Commit message validation fails

**Common errors:**
- Missing scope: `feat: add feature` ❌ → `feat(scope): add feature` ✅
- Wrong language: `feat(api): adicionar endpoint` ❌ → `feat(api): add endpoint` ✅
- Not imperative: `feat(api): added endpoint` ❌ → `feat(api): add endpoint` ✅
- Period in subject: `feat(api): add endpoint.` ❌ → `feat(api): add endpoint` ✅

**Solution:**
- Always use format: `type(scope): subject`
- Subject must be in English and imperative mood
- No period at end of subject
- Scope must reflect context/module

## Best Practices

1. **One context per commit**: Group related changes by module/folder
2. **Separate types**: Don't mix `feat` and `fix` in same commit
3. **Dependency order**: Commit innermost layers first (domain → service → api)
4. **Clear subjects**: Use descriptive but concise subjects
5. **English only**: All commit messages in English
6. **Imperative mood**: "add", "fix", "update" (not "added", "fixed", "updated")
7. **No periods**: Subject should not end with period
8. **Branch protection**: Never push to main without explicit authorization

## Additional Resources

- [Conventional Commits Specification](https://www.conventionalcommits.org/)
- [Semantic Release Documentation](https://semantic-release.gitbook.io/)
- See `.cursor/rules/conventional-commits.md` for detailed commit type definitions
- See `.cursor/rules/ai-commit-guidelines.md` for AI-specific commit guidelines

