# Optional project-specific guidance

The supplied `project/AGENTS.md` contains the routing rules intended for the target repository root. Add the following sections when they provide stable project information that agents cannot infer from the repository.

## Mixed-repository example

```markdown
## Repository map

- `mobile/`: Flutter application managed with FVM.
- `web/`: SvelteKit application.
- `services/jobs/`: Python workers managed with uv.
- `packages/contracts/`: Shared schemas and generated clients.

## Commands

- Flutter install: `cd mobile && fvm flutter pub get`
- Flutter verification: `cd mobile && fvm flutter analyze && fvm flutter test`
- Web install: `cd web && npm ci`
- Web verification: `cd web && npm run check && npm run test && npm run build`
- Python install: `cd services/jobs && uv sync --frozen`
- Python verification: `cd services/jobs && uv run ruff check . && uv run pytest`

## Boundaries

- Do not edit generated files under `packages/contracts/generated/`.
- Update shared schemas before running the existing generators.
- Keep authentication and serialization contracts synchronized across the mobile, web, and Python components.
```

Replace each example command with a script or command that the repository supports.

## Nested instruction files

Add an `AGENTS.md` inside a subdirectory when that component has rules that do not apply to the rest of the repository.

```text
repo/
|-- AGENTS.md
|-- mobile/
|   `-- AGENTS.md
|-- web/
|   `-- AGENTS.md
`-- services/jobs/
    `-- AGENTS.md
```

Flutter example:

```markdown
# Mobile-specific instructions

- Use FVM for every Flutter and Dart command.
- Do not edit `*.g.dart` or `*.freezed.dart` by hand.
- Run `fvm flutter analyze` and focused tests for changed features.
```

SvelteKit example:

```markdown
# Web-specific instructions

- Use the package manager declared in `package.json` and the existing lockfile.
- Keep server-only modules outside client-reachable imports.
- Run the existing check, test, and build scripts affected by the change.
```

Python example:

```markdown
# Python service instructions

- Use uv and the existing `pyproject.toml` configuration.
- Preserve transaction, idempotency, retry, and error-propagation behavior.
- Run focused pytest tests and the configured Ruff or type checks.
```

## Leave conventional projects concise

Skip project-specific guidance when:

- manifests and scripts follow clear conventions
- the repository has no unusual generated files or architectural boundaries
- the global instructions cover the required rules
- a section would repeat framework documentation

Short instruction files require less maintenance and carry less stale guidance.
