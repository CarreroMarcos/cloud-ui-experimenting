# AGENTS.md

## 1. Monorepo Architecture & Directory Scope
Organize code strictly by domain across workspace directories:
- **`terraform/` (IaC):** Cloud infrastructure, modules, and configurations.
- **`services/` or `backend/` (Python 3.13):** Backend services and utilities managed exclusively via `uv`.
- **`frontend/` or `web/` (Vanilla Web):** Static web assets built with native web standards (HTML5, modern CSS, ES Modules).

---

## 2. Tooling & Canonical Commands

### Python 3.13 (`uv`)
- **Dependency Management:** Manage dependencies exclusively using `uv add <package>` and `uv sync`.
- **Execution:** Run scripts, tools, and entrypoints through `uv run <command>`.
- **Code Quality:** Format and lint with `uv run ruff format .` and `uv run ruff check .`.
- **Type Checking:** Verify types using `uv run basedpyright` (or `uvx basedpyright`), leveraging Python 3.13 typing syntax (PEP 695 `type` aliases, type parameters).
- **Dead Code Detection:** Identify unused objects with `uv run vulture . --min-confidence 80` (or `uvx vulture . --min-confidence 80`).
- **Testing:** Execute test suites with `uv run pytest`.

### Terraform (IaC)
- **Formatting & Validation:** Run `terraform fmt -check` and `terraform validate` on all changes.
- **Execution Bounds:** Restrict autonomous operations to inspection and planning (`terraform plan`). Present plans clearly to the user, reserving `terraform apply` and state changes for manual user execution.

### Vanilla Frontend (HTML / CSS / JavaScript)
- **Native Standards:** Rely on native browser capabilities: semantic HTML5, modern CSS features (CSS nesting, custom properties, container queries), and native ES Modules (`<script type="module">`).
- **Local Preview:** Serve static assets using lightweight local tooling (e.g., `uv run python -m http.server 8000` or IDE preview).

---

## 3. Engineering Practices

### Simplicity & Direct Solutions
- **Direct Implementation:** Choose the simplest, most readable solution that fulfills immediate requirements.
- **Standard Libraries First:** Rely on native browser APIs (Fetch, DOM methods) and the Python standard library before introducing external packages.
- **Pragmatic DRY:** Value clear, localized code. Accept minor repetition when it avoids convoluted or premature abstractions.
- **Targeted Error Handling:** Focus error handling on external boundaries (user input, file I/O, network requests), keeping internal deterministic paths clean and direct.

### Proactive Planning & Autonomy
- **Evidence-Based Validation:** Prove correctness with concrete test or command outputs. Surface tradeoffs, edge cases, and uncertainties openly.
- **Confident Progression:** State working assumptions clearly and proceed with implementation. Pause for user alignment when changes impact core system architecture, data safety, or user-defined scope.
- **Context Awareness:** Review existing conventions, helper utilities, and project files before adding new structures.

---

## 4. Verification Loop
- **Backend & Logic:** Practice test-driven validation for business rules and calculations. Maintain a passing test suite with `uv run pytest`.
- **Frontend:** Verify layout, interactivity, and clean browser console output.
- **Infrastructure:** Confirm syntax and resource definitions with `terraform fmt` and `terraform validate`.
- **Diff Review:** Inspect `git diff` prior to completion to verify clean, focused modifications.
