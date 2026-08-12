# AGENTS.md

## Project-local programming agents

- When the user asks to use agents, select the appropriate project-local roles. Their definitions supply model, reasoning, sandbox, and role metadata.
- Keep small or tightly coupled work in the parent. Delegate only bounded work that benefits from independent exploration, implementation, review, or safe parallelism.
- The parent agent owns requirements, architectural and product decisions, task decomposition, integration, final verification, and the final response.

### Routing exceptions

- Let the available role descriptions guide normal selection.
- Use `deep_code_explorer` only when normal exploration is insufficient or the question crosses several technology boundaries.
- Use `code_reviewer` when requested or when change risk justifies an independent pass, not after every trivial edit.
- Use `commit_publisher` only when it is installed and the user explicitly asks Codex to commit, or to commit and push. A request for a commit message is not authorization to perform Git mutations.

### UI skill

- Use `$devibify` when installed for substantive visible UI implementation or review. User requirements and the existing design system take precedence; skip it for backend, non-visual, and mechanical UI work.

### Coordination

- Prefer agents for read-heavy or independent work; never spawn them merely to fill available concurrency.
- Give each writing agent a bounded objective and disjoint ownership. It must preserve unrelated changes.
- All agents except an explicitly authorized `commit_publisher` must avoid staging, committing, pushing, branch operations, remote synchronization, and history changes.
- Wait for required delegated work, inspect and integrate its result, and reuse reliable findings. The parent resolves incomplete or unverified handoffs.
