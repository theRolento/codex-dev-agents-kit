# Adopt the kit in a repository

## 1. Copy the core agents

From the target project's Git root:

```bash
KIT=/path/to/codex-dev-agents-kit
mkdir -p .codex/agents
cp -R "$KIT/project/.codex/." .codex/
```

The core agents are:

```text
code_explorer
quick_implementer
implementer
code_reviewer
```

## 2. Add the routing rules

If the repository has no `AGENTS.md`:

```bash
cp "$KIT/project/AGENTS.md" ./AGENTS.md
```

If `AGENTS.md` exists, merge the `Project-local programming agents` section into it.

The routing rules work in single-stack and mixed-stack repositories.

## 3. Decide whether Git should track the installation

Commit `.codex/` and `AGENTS.md` when the repository's users should share the agents.

For a local-only installation, add these entries to the target repository's `.gitignore` before staging the files:

```gitignore
# Local Codex agent configuration
/.codex/
/AGENTS.md
```

Ignore `/AGENTS.md` only when the file exists solely for this kit. Keep an existing shared `AGENTS.md` tracked. New ignore rules do not affect files that Git already tracks.

## 4. Review the optional global configuration

The project-local agents work without the files under `global/`.

Merge any personal rules you want from `global/AGENTS.md` into `~/.codex/AGENTS.md`. Review the differences instead of overwriting your existing file.

`global/config.toml.reference` contains reference values, not a complete `~/.codex/config.toml`. Merge values that your Codex CLI version still requires.

Do not copy `global/AGENTS.md` into the target repository. Do not copy `project/AGENTS.md` into `~/.codex/`.

## 5. Keep `$devibify` separate

The kit does not copy third-party skills.

When [`$devibify`](https://github.com/theRolento/devibify) is installed in the user's Codex environment, the agents use it for visible UI work. The core workflow requires no other external skill.

The kit does not require `$code-review`. Removing that skill does not reduce the checks built into `code_reviewer`.

## 6. Add stable project context

The routing rules work without extra project guidance. Add details that remain stable and that agents cannot infer from project files, such as:

- an unusual directory map
- exact commands for each component
- generated files that agents must not edit
- architectural boundaries
- external services or local prerequisites
- build or platform constraints

See `examples/OPTIONAL-PROJECT-GUIDANCE.md`.

## 7. Install optional agents where needed

Install `deep_code_explorer`:

```bash
cp "$KIT/project/optional-agents/deep_code_explorer.toml" .codex/agents/
```

Install `commit_publisher`:

```bash
cp "$KIT/project/optional-agents/commit_publisher.toml" .codex/agents/
```

## 8. Start a new session

After changing `AGENTS.md`, `.codex/config.toml`, an agent definition, or a skill, start a new Codex session if the current session does not detect the change.

## 9. Select the parent model

Use `/model` to select a model and reasoning effort for the current task.

The supplied agents target the GPT-5.6 Sol, Terra, and Luna family. Their default role assignments use Sol or Luna. To use Terra for a role, change that agent's `model` field to `gpt-5.6-terra`.

## 10. Run smoke tests

Test read-only exploration:

```text
Use the agents to analyze this repository's structure. Do not modify files.
```

Test a working-tree review:

```text
Use code_reviewer to review the current changes. Do not modify files.
```

Test a comparison review:

```text
Use code_reviewer to compare this branch with main. Do not modify files.
```

Test UI review:

```text
Use code_reviewer and $devibify to review the current UI changes. Do not modify files.
```

Use `/agent` to inspect the initial test run or troubleshoot agent routing.

## 11. Keep agent-tool repositories isolated

Do not copy `project/AGENTS.md` or `project/.codex/agents/` into repositories that contain your agent tools.

User-installed skills remain available throughout the Codex environment. This kit invokes them through routing rules found in repositories where you install the project-local configuration.
