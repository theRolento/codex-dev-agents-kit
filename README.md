# Codex Development Agents Kit

Version 1.0.0, released on July 15, 2026.

This kit adds project-local programming agents to software repositories without exposing them in repositories that contain your agent tools.

The agents work across technology stacks, including:

- SvelteKit
- Flutter and Dart
- Python
- repositories that combine these stacks
- other stacks with documented project conventions

## Quick start

Install the core agents in a target repository before reviewing the optional files.

| Source in this kit | Destination | Required | Purpose |
|---|---|---:|---|
| `project/.codex/agents/` | `<target-repository>/.codex/agents/` | Yes | Project-local agent definitions |
| `project/AGENTS.md` | `<target-repository>/AGENTS.md` | Yes | Cross-role routing and coordination rules |
| `project/optional-agents/*.toml` | `<target-repository>/.codex/agents/` | No | Deep exploration or Git publishing |
| `global/AGENTS.md` | `~/.codex/AGENTS.md` | No | Personal development instructions across repositories |
| `global/config.toml.reference` | `~/.codex/config.toml` | No | Reference values for personal Codex configuration |

Do not place the `project/` directory itself inside the target repository. Copy its `.codex/` directory and `AGENTS.md` file to the repository root.

### 1. Copy the core configuration

Run these commands from the target repository root. Replace the first path with the location of this kit:

```bash
KIT=/path/to/codex-dev-agents-kit
mkdir -p .codex/agents
cp -R "$KIT/project/.codex/." .codex/
```

The target repository now contains:

```text
<target-repository>/
|-- .codex/
|   `-- agents/
|       |-- code_explorer.toml
|       |-- quick_implementer.toml
|       |-- implementer.toml
|       `-- code_reviewer.toml
`-- ...
```

### 2. Add the routing rules

If the target repository has no `AGENTS.md`:

```bash
cp "$KIT/project/AGENTS.md" ./AGENTS.md
```

If it already has `AGENTS.md`, copy the `Project-local programming agents` section from `project/AGENTS.md` into the existing file. Keep the repository's current instructions.

### 3. Decide whether Git should track the installation

Commit `.codex/` and `AGENTS.md` when everyone who uses the repository should receive the agents.

For a local-only installation, add these entries to the target repository's `.gitignore` before staging the files:

```gitignore
# Local Codex agent configuration
/.codex/
/AGENTS.md
```

Ignore `/AGENTS.md` only when the file exists solely for this kit. Do not ignore a shared `AGENTS.md` that the repository already tracks. Git does not apply new ignore rules to tracked files; remove them from the Git index yourself if you choose to convert an existing installation to local-only.

### 4. Start a new Codex session

Start a Codex session from the target repository and test read-only exploration:

```text
Use the agents to analyze this repository's structure. Do not modify files.
```

The parent agent should select `code_explorer`. Use `/agent` to inspect agent activity when testing the setup.

## Architecture

```text
~/.codex/                  # Optional personal configuration
|-- AGENTS.md
`-- config.toml

workspace/
|-- target-project-a/
|   |-- AGENTS.md
|   `-- .codex/
|       `-- agents/
|-- target-project-b/
|   |-- AGENTS.md
|   `-- .codex/
|       `-- agents/
|-- agent-tool-a/          # No project installation
`-- agent-tool-b/          # No project installation
```

Install the project files in software repositories that need the agents. Do not install them in repositories that contain agent tools.

The files under `<repository>/.codex/agents/` define custom Codex subagents and describe their normal routing. The repository-root `AGENTS.md` supplies cross-role exceptions and coordination rules.

## Terminology

- **Parent agent:** the Codex agent that receives the user's request and owns requirements, decisions, integration, verification, and the final response.
- **Subagent:** a custom agent that the parent agent starts for a bounded task.
- **Project-local agent:** a subagent defined under one repository's `.codex/agents/` directory.
- **Working-tree review:** a review of staged, unstaged, and relevant untracked changes.
- **Comparison review:** a review of committed work against a branch, tag, or commit.
- **Commit message:** a Conventional Commits-compliant description of a Git commit. A request for a message does not authorize staging or committing.

## Included agents

| Agent | Model | Reasoning effort | Sandbox mode | Availability |
|---|---|---:|---|---|
| `code_explorer` | `gpt-5.6-luna` | `high` | `read-only` | Core |
| `quick_implementer` | `gpt-5.6-luna` | `high` | `workspace-write` | Core |
| `implementer` | `gpt-5.6-sol` | `low` | `workspace-write` | Core |
| `code_reviewer` | `gpt-5.6-sol` | `low` | `read-only` | Core |
| `deep_code_explorer` | `gpt-5.6-luna` | `xhigh` | `read-only` | Optional |
| `commit_publisher` | `gpt-5.6-luna` | `high` | `workspace-write` | Optional |

### GPT-5.6 model family

This release targets OpenAI's current [GPT-5.6 Codex model family](https://learn.chatgpt.com/docs/models): Sol, Terra, and Luna.

- The implementation and review roles use `gpt-5.6-sol`.
- The exploration, focused-change, and Git publishing roles use `gpt-5.6-luna`.
- The supplied agents do not select `gpt-5.6-terra`. Users who prefer Terra for an everyday role can change that agent's `model` field.

The kit does not pin the parent model. Select a model and reasoning effort in the Codex CLI session through `/model`.

## One reviewer without `$code-review`

This kit does not require or invoke the external `$code-review` skill.

The `code_reviewer` prompt incorporates the relevant parts of that workflow:

- working-tree, comparison, combined, and supplied-diff review modes
- validation of the Git comparison target and review scope
- requirement checks when a reliable specification exists
- checks against documented repository standards
- separation of confirmed defects, requirement mismatches, documented-standard violations, heuristic concerns, and unresolved questions
- project rules taking precedence over generic heuristics
- checks for missing requirements, partial implementations, incorrect behavior, regressions, and unnecessary scope

The reviewer remains one read-only subagent and does not start more reviewers. It checks Correctness and Risk, Spec, Standards, Tests, and UI when applicable, then reports concrete findings followed by a compact scope and risk summary.

You can uninstall `$code-review` without losing the checks listed above.

## Integration with `$devibify`

Install [`$devibify`](https://github.com/theRolento/devibify) separately when you want the agents to apply its UI workflow. Use it for work that changes visible UI:

- `quick_implementer` uses it for small UI changes.
- `implementer` uses it for larger UI work and its final quality pass.
- `code_reviewer` uses it as a read-only diagnostic guide.
- The parent agent uses it for UI planning, implementation, or review.

The kit uses the existing implementation agents for UI work instead of adding a separate UI agent.

Apply UI guidance in this order:

1. The user's requirements
2. The project's existing design system and conventions
3. Project-specific UI instructions
4. `$devibify` defaults and checklists

## Repository layout

```text
codex-dev-agents-kit/
|-- .gitignore
|-- README.md
|-- CHANGELOG.md
|-- SOURCES.md
|-- THIRD-PARTY-NOTICES.md
|-- global/
|   |-- AGENTS.md
|   `-- config.toml.reference
|-- project/
|   |-- AGENTS.md
|   |-- .codex/
|   |   `-- agents/
|   |       |-- code_explorer.toml
|   |       |-- quick_implementer.toml
|   |       |-- implementer.toml
|   |       `-- code_reviewer.toml
|   `-- optional-agents/
|       |-- deep_code_explorer.toml
|       `-- commit_publisher.toml
|-- examples/
|   `-- OPTIONAL-PROJECT-GUIDANCE.md
`-- guides/
    |-- ADOPTION.md
    |-- PROMPTS.md
    `-- WORKFLOWS.md
```

## Optional configuration

### Personal global instructions

The project installation is self-contained: its routing and role safeguards work without the files under `global/`.

Compare `global/AGENTS.md` with your existing `~/.codex/AGENTS.md` and merge the rules you want to apply across repositories. Do not overwrite personal instructions without reviewing the differences.

The global browser-verification rules use each target repository's local Playwright package and install only its required browser revision. Docker users should configure a writable, persistent Playwright browser cache; a global Playwright package is neither required nor used as a fallback.

The global instructions keep concise Svelte, Flutter, and Python guidance for mixed-stack repositories. Verification starts with the smallest relevant configured check and expands only when change risk or repository instructions require it.

`global/config.toml.reference` contains reference values for the MultiAgent V2 feature block. Merge values that your Codex CLI version still requires into `~/.codex/config.toml`. The reference is not a complete personal configuration and does not set the parent model.

For Codex CLI 0.146.0, the reference forces Multi-Agent V2, uses the `agents` tool namespace, keeps spawn metadata visible, allows one parent plus four concurrent subagents, and changes the default `wait_agent` timeout from 30 seconds to 10 minutes. The V2 value `max_concurrent_threads_per_session = 5` counts the parent; the legacy `[agents] max_threads = 4` form counts only subagents.

The project installation intentionally has no `.codex/config.toml` concurrency block. It inherits the global V2 limit when installed and otherwise uses the CLI default. Add a project-local V2 override only when a repository genuinely needs a different total thread limit.

The configured-agent `fork_turns` rule lives in `global/AGENTS.md`. Do not replace `root_agent_usage_hint_text` with only that rule: in Codex CLI 0.146.0, a configured value replaces the complete built-in root-agent hint instead of appending to it.

Do not copy `global/AGENTS.md` into a target repository. Do not copy `project/AGENTS.md` into `~/.codex/`.

### Optional agents

Set `KIT` to this kit's path if you opened a new shell:

Install the deep explorer:

```bash
KIT=/path/to/codex-dev-agents-kit
cp "$KIT/project/optional-agents/deep_code_explorer.toml" .codex/agents/
```

Install the Git publishing agent:

```bash
cp "$KIT/project/optional-agents/commit_publisher.toml" .codex/agents/
```

The `commit_publisher` performs Git mutations. Leave it uninstalled when you want to control staging, commits, and pushes yourself.

### Agent-tool repository isolation

Do not copy `project/AGENTS.md` or `project/.codex/agents/` into repositories that contain your agent tools.

## Everyday prompts

You do not need to specify a model, reasoning effort, sandbox mode, `agent_type`, `fork_turns`, or thread parameters.

```text
Use the agents when they help, and implement this change: <description>.
```

```text
Use code_explorer to trace the flow, then fix the problem.
```

```text
Use code_reviewer to review all current changes. Fix concrete problems and verify the result.
```

```text
Use code_reviewer to compare this work with main and check it against the requirements I provided.
```

```text
Use the agents and $devibify to implement and verify this UI.
```

```text
Do not use subagents for this change. Handle it in the parent agent.
```

See `guides/PROMPTS.md` and `guides/WORKFLOWS.md` for more examples.

## Project-specific `AGENTS.md` guidance

The supplied `project/AGENTS.md` contains cross-role routing exceptions and coordination rules. Agent definitions provide their normal selection guidance. Add project details when they remain stable and are hard to infer, such as:

- exact commands that project files do not reveal
- generated directories that agents must not edit
- architectural boundaries
- separate toolchains in a mixed repository
- external service setup
- platform or deployment constraints

See `examples/OPTIONAL-PROJECT-GUIDANCE.md`.

## Verification

Parse every supplied TOML file and the global reference:

```bash
python3 - <<'PY'
from pathlib import Path
import tomllib

files = sorted(Path("project").rglob("*.toml"))
files.append(Path("global/config.toml.reference"))
for path in files:
    tomllib.loads(path.read_text())
print("Parsed %d configuration files." % len(files))
PY
```

For a runtime check, start a new Codex session in a trusted repository and run:

```text
Use the agents to analyze this repository's structure. Do not modify files.
```

Use `/agent` to inspect the initial test run or troubleshoot agent routing.
