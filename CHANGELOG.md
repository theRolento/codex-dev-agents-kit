# Changelog

## Unreleased

- Centralizes the Codex CLI 0.146.0 Multi-Agent V2 concurrency and wait defaults in the optional global configuration reference.
- Removes legacy project-local `max_threads` and V1-only `max_depth` settings.
- Documents total-thread counting, repository overrides, configured-agent `fork_turns` requirements, and safe placement of root-agent guidance.
- Makes `trash-put` the primary removal tool and prevents a failure from any one trash utility from stopping cleanup before every installed alternative is tried.
- Adds global scope controls against speculative guards, premature abstractions, and unrequested refactors.

## 1.0.0 - 2026-07-15

- Initial release of the project-local agent kit.
- Includes four core agents and two optional agents.
- Targets the GPT-5.6 Sol, Terra, and Luna model family, with supplied roles assigned to Sol or Luna.
- Includes routing rules, installation guides, workflow examples, and optional `$devibify` integration.
