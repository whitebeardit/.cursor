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
