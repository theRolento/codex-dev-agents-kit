# Changelog

## Unreleased

- Centralizes the Codex CLI 0.146.0 Multi-Agent V2 concurrency and wait defaults in the optional global configuration reference.
- Removes legacy project-local `max_threads` and V1-only `max_depth` settings.
- Documents total-thread counting, repository overrides, configured-agent `fork_turns` requirements, and safe placement of root-agent guidance.
- Uses `trash-put` as the sole recoverable removal command and requires confirmation before permanent deletion when it is unavailable or fails.
- Adds global scope controls against speculative guards, premature abstractions, and unrequested refactors.
- Uses project-local browser tooling, limits browser checks to relevant changes, and avoids dependency changes or speculative system-library installation.
- Condenses global guidance and makes verification, UI skills, and framework documentation proportional to the task while retaining mixed-stack rules.
- Condenses project routing and all core and optional agent prompts while preserving role boundaries, read-only enforcement, Git safeguards, and independent operation from the optional global configuration.
- Makes reviewer output findings-first with a compact scope and risk summary instead of mandatory empty axis sections.
- Promotes `deep_code_explorer` to the core project installation while keeping Git publishing optional.

## 1.0.0 - 2026-07-15

- Initial release of the project-local agent kit.
- Includes four core agents and two optional agents.
- Targets the GPT-5.6 Sol, Terra, and Luna model family, with supplied roles assigned to Sol or Luna.
- Includes routing rules, installation guides, workflow examples, and optional `$devibify` integration.
