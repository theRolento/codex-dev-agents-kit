# AGENTS.md

## General

- Follow existing project patterns unless explicitly asked to change them.
- Keep changes minimal, targeted, and production-ready.
- Prefer reuse over reinvention.
- Do not introduce unnecessary dependencies, abstractions, or visual novelty unless clearly justified.
- Return clear errors instead of hiding failures behind fallbacks.
- Do not claim completion until you run the relevant checks. State any skipped checks and the remaining risks.

## Project commands

- Prefer existing project scripts and repository conventions over ad hoc commands.
- Inspect project files before choosing commands.
- Do not invent new scripts unless explicitly asked.
- Assume the workspace and project tooling are Linux-native unless local evidence shows otherwise.
- Use the package manager, environment manager, SDK manager, and task runner already adopted by the project.
- Do not allow `npx`, `npm exec`, or another package runner to download a missing tool implicitly. Use a project-local dependency or an existing project script. Ask before installing a missing tool or dependency.
- If Playwright, Chrome, Chromium, or another browser runtime is missing or mismatched, install the required project-scoped browser or runtime using the repository's documented commands or the relevant local tool, such as `npx --no-install playwright install --with-deps chromium`, instead of sending the task back to a host environment.
- If a command fails because of a true external host dependency, missing service, native binary mismatch unrelated to installable Linux tooling, or another environment issue that cannot be fixed locally, stop after the first clear environment failure, report the exact command and error, and ask the user to run or configure it manually.

### Node, web, and SvelteKit projects

- Check `package.json`, the `packageManager` field, and lockfiles before selecting a package manager or command.
- Prefer existing scripts when available.
- Common examples include:
  - `npm run lint`
  - `npm run format`
  - `npm run check`
  - `npm run test`
  - `npm run build`
- If a relevant script does not exist, use only a project-local configured tool. Do not fetch a tool implicitly through `npx` or `npm exec`.

## UI work

- For user-facing UI work, including web, SvelteKit, Flutter, landing pages, dashboards, components, forms, styling, and UI or UX review, use the `$devibify` skill when installed.
- Apply `$devibify` when generating, refactoring, polishing, or reviewing visible UI. Do not invoke it for backend-only or non-visual changes.
- Treat project-specific UI and UX guidance as a constraint and reconcile it with `$devibify`. The user's requirements and the existing product design system take precedence over generic defaults.
- Treat UI work as system design plus implementation, not just styling.
- Define or infer a UI contract before making non-trivial UI changes.
- Verify responsive behavior, interactive states, loading states, error states, empty states, disabled states, and accessibility basics before finishing when relevant.

## Safe file operations

- Never use `rm` or `rmdir`.
- Prefer moving files or directories to trash instead of permanently deleting them.
- Use an available trash command such as `trash`, `gio trash`, or `trash-put`.
- If no trash utility is available, stop and report that clearly instead of falling back to permanent deletion.
- Do not overwrite or regenerate files outside the requested scope.
- Do not edit generated files manually when the repository provides a generator.

Examples:

- Instead of `rm <file>`, use a trash command.
- Instead of `rm -rf <dir>`, use a trash command.
- Instead of `rmdir <dir>`, use a trash command.

## Git safety

- The user manages GitHub synchronization and branches in VS Code. Perform a Git mutation or remote operation only when the user requests that specific operation.
- Do not run destructive Git commands such as `git reset --hard`, `git checkout --`, `git restore`, `git clean`, force pushes, or history rewrites unless the user explicitly asks for that exact operation.
- Do not revert, overwrite, or discard changes you did not make. If unrelated local changes are present, leave them alone.
- It is fine to inspect Git state with read-only commands such as `git status`, `git diff`, `git log`, and `git show` when relevant.

## Tests and debugging

- When a unit or integration test fails, do not assume the production code is wrong.
- Check whether the failure reveals:
  - a real bug in production code
  - an outdated or incorrect test
  - an environment or setup issue
- Favor fixes that address root causes rather than patching symptoms.
- Add or update tests when behavior changes and a relevant test harness exists.
- Do not create superficial tests that only mirror implementation details or pass without exercising the requested behavior.

## Verification

- Run relevant formatting, linting, analysis, typechecking, tests, and build steps before finishing when those scripts or commands exist.
- Start with the smallest relevant verification, then expand according to the changed surface and repository instructions.
- If verification cannot be completed, say exactly what was not run, why, and what risk remains.
- Do not treat Playwright, Chrome, Chromium, or other browser-runtime installation failures as host-environment blockers by default. In the Linux workspace, try the relevant project-scoped install or verification command before marking verification blocked.
- If verification is still blocked by an environment issue that cannot be fixed in the Linux workspace, do not keep retrying unrelated workarounds. List the commands the user should run manually and mark verification as not completed locally.
- For refactors, pay extra attention to regressions, call-path consistency, state transitions, data migrations, compatibility, and error handling before declaring the work complete.

## Docker and networking

- When running inside Docker, never assume `127.0.0.1` reaches the host machine.
- If the app is running on the host, prefer `http://host.docker.internal:<port>` when supported by the environment.
- If starting a server inside the container, bind it to `0.0.0.0`, not `127.0.0.1`.
- Do not publish common host development ports such as `5173` or `4173` by default from the Codex container when host-side tools may also use them. Prefer high remapped host ports or opt-in port publishing to avoid breaking normal `localhost` workflows outside Docker.

## Long-running processes

- When starting development servers, preview servers, watchers, Playwright reports, or other long-running commands, keep track of the command, working directory, process, and URL or port.
- Do not leave long-running processes active after finishing unless the user asked for them to stay running.
- If a server is needed for browser verification from inside Docker, bind it to `0.0.0.0` and use the configured forwarded port or container-reachable URL.

## Svelte and SvelteKit

- For Svelte or SvelteKit work, consult the official Svelte LLM documentation before making framework-specific changes:
  - `https://svelte.dev/docs/llms`
  - use the relevant `llms.txt` or `llms-full.txt` documentation when helpful
- Respect server-only and client-reachable module boundaries.
- Follow the repository's established patterns for load functions, actions, endpoints, hooks, validation, state, forms, and error handling.
- Do not expose private environment variables, server-only modules, secrets, or privileged data to client bundles.

## Flutter, Dart, and FVM

- In Flutter or Dart projects, use the native toolchain instead of Node-based commands.
- If the project uses FVM, prefer `fvm flutter ...` and `fvm dart ...` over plain `flutter` or `dart`.
- Check project files and existing conventions before choosing commands.
- Respect the repository's existing state-management, navigation, dependency-injection, persistence, serialization, lifecycle, and code-generation patterns.
- Never edit generated Dart files manually when the project provides a generator.
- If Flutter, Dart, or FVM commands fail because SDKs, emulators, device tooling, signing, or platform-specific project files are only configured outside the Linux workspace, stop after the first clear environment failure and provide the exact commands for the user to run manually.
- Common verification commands include:
  - `fvm flutter analyze`
  - `fvm flutter test`
  - `fvm dart format .`
  - `fvm dart analyze`
- If FVM is not configured for the project, use the standard Flutter and Dart commands instead.

## Python

- Check `pyproject.toml`, lockfiles, task-runner configuration, supported Python versions, and repository instructions before choosing commands.
- Use the package and environment manager already adopted by the project, such as `uv`, Poetry, PDM, Hatch, pip-tools, or a project-managed virtual environment.
- Do not switch package managers or introduce a new environment workflow unless explicitly asked.
- Do not install packages globally or use bare `pip` when project-managed commands exist.
- Prefer existing project scripts and configured tools over generic replacements.
- Respect the project's package layout, import boundaries, typing policy, async model, transaction handling, migration workflow, logging, exception handling, and test conventions.
- Do not edit generated migrations, generated clients, compiled schemas, or generated source files manually when a generator exists.
- Common verification commands may include the following only when the corresponding tools are configured:
  - `uv run pytest`
  - `uv run ruff check .`
  - `uv run ruff format --check .`
  - `uv run mypy .`
  - `uv run pyright`
- If the project uses another runner or package manager, use its equivalent commands instead.

## Final response convention

- When code changes are made, include one suggested Conventional Commits-compliant commit message in the final response.
- Format it as `<type>[optional scope]: <description>`.
- If the change is breaking, use `!` before the colon or include a `BREAKING CHANGE:` footer.
- Do not include a commit message when no code changes were made.
