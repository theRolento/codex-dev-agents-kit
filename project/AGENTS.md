# AGENTS.md

## Project-local programming agents

- This repository enables the custom programming agents declared under `.codex/agents/`.
- When the user asks to use agents, select and coordinate the appropriate project-local agents. Do not require model names, reasoning levels, sandbox modes, spawn parameters, or thread settings.
- Delegate when another agent can improve correctness, isolate lengthy exploration, provide an independent review, or handle separate work in parallel.
- Keep small or tightly coupled tasks in the parent agent.
- The parent agent owns requirements, architectural and product decisions, task decomposition, integration, final verification, and the final response.

### Agent selection

- Use `code_explorer` when the execution path, ownership, dependencies, contracts, or change surface are unclear and discovery spans multiple files.
- Use `deep_code_explorer` only when it is installed and normal exploration is insufficient, or when the task crosses several packages, runtimes, or technology boundaries.
- Use `quick_implementer` for an explicit low-risk change expected to fit in one or two production files plus focused tests when applicable.
- Use `implementer` for a defined non-trivial or multi-file change, bounded debugging, or substantial test work.
- Use `code_reviewer` when the user requests review, after high-risk changes, or when an independent pass can improve confidence. It can review working-tree changes, a supplied diff, or committed work against a branch, tag, or commit.
- Do not run `code_reviewer` after every trivial edit.
- Use `commit_publisher` only when it is installed and the user explicitly asks Codex to commit, or to commit and push. A request for a commit message is not authorization to perform Git mutations.

### UI skill

- For visible UI implementation or review, use `$devibify` when it is installed.
- Apply explicit user requirements and the repository's existing design system before generic skill defaults.
- Do not invoke `$devibify` for backend-only or non-visual work.

### Coordination

- Prefer agents for read-heavy work such as exploration, triage, test analysis, and review.
- Do not spawn agents merely to use available concurrency.
- Do not allow writing agents to modify overlapping files or modules concurrently.
- Give each writing agent a bounded objective and clear ownership. It must preserve unrelated local changes.
- All agents except an explicitly authorized `commit_publisher` must avoid staging, committing, pushing, branch operations, remote synchronization, and history changes.
- Wait for delegated work required by the task, inspect its result, and integrate it in the parent thread.
- Reuse existing findings instead of repeating the same repository discovery or review.
- If a delegated result is incomplete or unreliable, the parent must resolve the gap rather than treating the handoff as verified.
