# Workflows

## 1. Small task

```text
Parent agent
  -> implements the change
  -> verifies the result
```

Keep a small task in the parent agent when delegation would add no useful separation or independent verification.

## 2. Unclear execution flow

```text
Parent agent
  -> code_explorer
  -> makes the architectural decision
  -> quick_implementer or implementer
  -> performs final verification
```

Prompt:

```text
Use code_explorer to trace the flow, then fix the problem.
```

## 3. Understood multi-file feature

```text
Parent agent
  -> defines the objective, boundaries, and file ownership
  -> implementer
  -> integrates and verifies the change
  -> code_reviewer when the risk warrants independent review
```

Prompt:

```text
Use the agents to implement <feature>. Keep architectural decisions in the parent agent and verify the integrated result.
```

## 4. Mixed Flutter, SvelteKit, and Python repository

```text
Parent agent
  -> code_explorer for the Flutter component
  -> code_explorer for the SvelteKit component
  -> code_explorer for an independent Python component
  -> compares contracts between components
  -> assigns non-overlapping files to implementation agents
  -> verifies each toolchain
```

Prompt:

```text
Use separate agents to analyze the mobile, web, and backend components. Compare their contracts, then implement the change without assigning the same files to multiple agents.
```

## 5. Review local changes

```text
Parent agent
  -> code_reviewer in working-tree mode
  -> evaluates the findings
  -> fixes confirmed problems or delegates a bounded fix
  -> reruns the relevant checks
```

Prompt:

```text
Use code_reviewer to review all current changes. Fix concrete findings and rerun the relevant tests.
```

## 6. Compare a branch with main

```text
Parent agent
  -> code_reviewer in comparison mode
  -> validates main
  -> reviews main...HEAD and the commits in that range
  -> checks correctness, standards, and available requirements
  -> returns prioritized findings
```

Prompt:

```text
Use code_reviewer to compare this branch with main. The specification is in docs/spec.md.
```

## 7. Review a branch and local changes

Use combined mode when the review must cover committed branch work and local changes.

```text
Parent agent
  -> code_reviewer in combined mode
  -> keeps the committed range separate from working-tree changes
  -> fixes confirmed problems
  -> verifies the result
```

Prompt:

```text
Use code_reviewer to review all work against main, including relevant staged, unstaged, and untracked files.
```

## 8. UI work with `$devibify`

```text
Parent agent
  -> code_explorer when the flow is unclear
  -> quick_implementer or implementer with $devibify
  -> verifies the result in a browser or on the target platform
  -> code_reviewer with $devibify when independent review would help
```

Prompt:

```text
Use the agents and $devibify to implement and verify this UI: <description>.
```

## 9. Deep exploration

```text
Parent agent
  -> deep_code_explorer
  -> makes the decision and plan
  -> implementer or parent agent
  -> runs a focused review
```

Use `deep_code_explorer` when normal exploration cannot trace the behavior or when the flow crosses several technology boundaries.

## 10. Assisted manual commit

Manual workflow:

```text
Parent agent
  -> performs final verification
  -> suggests a commit message
  -> user creates the commit and pushes it
```

Optional publishing workflow:

```text
Parent agent
  -> performs final verification
  -> commit_publisher after an explicit user request
```

Do not install `commit_publisher` in repositories where Codex must not perform Git mutations.
