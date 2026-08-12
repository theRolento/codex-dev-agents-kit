# AGENTS.md

## General

- Follow existing project patterns and keep work, plans, and delegated tasks within the requested scope.
- Implement the smallest complete, production-ready change that satisfies current requirements and verified failure modes.
- Reuse existing code. Avoid unnecessary dependencies, visual novelty, speculative guards, fallback paths, compatibility layers, extension points, and abstractions unless current requirements, observed failures, security, or project patterns justify them.
- Return clear errors instead of hiding failures behind fallbacks.
- Run relevant checks before claiming completion. State skipped checks and remaining risks.

## Custom agent spawning

- When spawning a configured agent role, select its agent type and set `fork_turns = "none"` by default.
- Use a small positive `fork_turns` value only for needed recent context. Never omit it for a configured role because a full-history fork rejects agent-type overrides.
- Let the configured agent TOML determine its model and reasoning effort. Do not pass explicit model or reasoning overrides when the role already defines them.

## Project commands

- Inspect project files, then use existing scripts, repository conventions, and adopted package, environment, SDK, and task runners. Do not invent scripts unless asked.
- Assume the workspace and project tooling are Linux-native unless local evidence shows otherwise.
- Do not allow `npx`, `npm exec`, or another package runner to download a missing tool implicitly. Use a project-local dependency or an existing project script. Ask before installing a missing tool or dependency.
- For browser work, use the repository's existing scripts and project-local browser tooling. Do not invoke a globally installed Playwright package.
- Do not update Playwright to supply a missing browser. If the repository has no browser-testing tool, ask before adding one.
- If a command fails because of a true external host dependency, missing service, native binary mismatch unrelated to installable Linux tooling, or another environment issue that cannot be fixed locally, stop after the first clear environment failure, report the exact command and error, and ask the user to run or configure it manually.

### Node, web, and SvelteKit projects

- Select the package manager from `package.json`, its `packageManager` field, and lockfiles; use package scripts and project-local tools.

## UI work

- Use `$devibify` for substantive UI implementation, refactoring, or UX review. Skip it for backend, non-visual, isolated copy or token, and mechanical UI changes.
- Reconcile `$devibify` with project guidance; user requirements and the product design system take precedence.
- For substantive UI work, define or infer the UI contract and verify relevant responsive behavior, interaction and data states, and accessibility basics.

## Safe file operations

- Treat in-scope trash operations as non-destructive and allowed without extra approval.
- Use `trash-put -- <path>` for pre-existing or user-owned paths and let it select the trash location.
- If `trash-put` is unavailable or fails, keep the paths, report the command and error, and ask before permanent deletion. Do not use another trash utility or emulate a trash directory.
- Permanently delete an exact path only with explicit user authorization; task-created temporary artifacts are exempt after verifying their exact paths and ownership.
- Never use broad, globbed, repository-wide, or ambiguous deletion targets. Recurse only into an exact verified path covered by the preceding rule.
- Do not overwrite or regenerate files outside the requested scope.
- Do not edit generated files manually when the repository provides a generator.

## Git safety

- The user manages GitHub synchronization and branches. Perform Git mutations or remote operations only when the user requests the specific action.
- Do not run destructive Git commands such as `git reset --hard`, `git checkout --`, `git restore`, `git clean`, force pushes, or history rewrites unless the user explicitly asks for that exact operation.
- Preserve unrelated changes. Read-only commands such as `git status`, `git diff`, `git log`, and `git show` are allowed when relevant.

## Tests and debugging

- When a test fails, determine whether production code, the test, or the environment is wrong before editing; fix the root cause.
- Add or update tests for behavior changes when a relevant harness exists. Avoid tests that only mirror implementation details.

## Verification

- Run the smallest relevant configured check; expand only when change risk or repository instructions require broader coverage. Report skipped checks, blockers, and remaining risk.
- Run Playwright or other browser-based checks only when the change can affect browser-visible behavior or the repository instructions require them. Use the smallest relevant browser check.
- If project-local Playwright reports a missing compatible browser, state the Playwright version and required browser revision, then install that browser through the project-local CLI into the configured cache. Do not change `package.json` or a lockfile for a missing browser.
- Install a missing Playwright browser without `--with-deps` first. Use `--with-deps` only after an error identifies missing system libraries.
- If the configured browser cache is not writable or the project-local browser installation fails, report the command and error instead of using a global browser or changing project dependencies.
- For refactors, pay extra attention to regressions, call-path consistency, state transitions, data migrations, compatibility, and error handling before declaring the work complete.

## Docker, networking, and long-running processes

- When running inside Docker, never assume `127.0.0.1` reaches the host machine.
- If the app is running on the host, prefer `http://host.docker.internal:<port>` when supported by the environment.
- Bind container servers to `0.0.0.0`. Avoid publishing common host ports such as `5173` or `4173`; prefer high remapped ports or opt-in publishing.
- Track each long-running command, working directory, process, and URL or port.
- Do not leave long-running processes active after finishing unless the user asked for them to stay running.

## Svelte and SvelteKit

- Consult the official Svelte LLM documentation at `https://svelte.dev/docs/llms` only when framework behavior is uncertain, version-sensitive, or not established by repository patterns.
- Respect server-only and client-reachable module boundaries.
- Follow repository patterns for routing, data loading, actions, endpoints, hooks, validation, state, forms, and errors.
- Do not expose private environment variables, server-only modules, secrets, or privileged data to client bundles.

## Flutter, Dart, and FVM

- In Flutter or Dart projects, use the native toolchain instead of Node-based commands.
- If the project uses FVM, prefer `fvm flutter ...` and `fvm dart ...` over plain `flutter` or `dart`.
- Follow the project's state, navigation, dependency-injection, persistence, serialization, lifecycle, and code-generation workflows. Do not edit generated Dart files.
- If Flutter, Dart, or FVM commands fail because SDKs, emulators, device tooling, signing, or platform-specific project files are only configured outside the Linux workspace, stop after the first clear environment failure and provide the exact commands for the user to run manually.

## Python

- Read `pyproject.toml`, lockfiles, supported Python versions, and task-runner configuration; use the adopted package and environment manager.
- Do not switch managers, introduce a new environment workflow, install packages globally, or use bare `pip` when project-managed commands exist.
- Respect the project's package layout, import boundaries, typing policy, async model, transaction handling, migration workflow, logging, exception handling, and test conventions.
- Do not edit generated migrations, generated clients, compiled schemas, or generated source files manually when a generator exists.

## Final response convention

- After changing code or tracked documentation, suggest exactly one commit message formatted as `<type>[optional scope]: <description>`; use `!` or a `BREAKING CHANGE:` footer when applicable. Omit it when no files changed.
