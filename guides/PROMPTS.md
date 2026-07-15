# Practical prompts

Write everyday prompts in plain language. The routing rules select models, reasoning effort, sandbox modes, agent types, and thread settings.

## Automatic selection

```text
Use the agents when they help, and implement this change: <description>.
```

```text
Analyze the problem, use the appropriate agents, and continue until you have verified the solution.
```

## No delegation

```text
Do not use subagents for this change. Handle it in the parent agent and verify the result.
```

## Exploration

```text
Use code_explorer to trace <feature>, then fix the problem.
```

```text
Before editing code, use code_explorer to identify the entry points, contracts, state, tests, and affected files.
```

## Deep exploration

```text
Use deep_code_explorer to trace the complete flow across Flutter, SvelteKit, and Python. Then propose and implement the smallest coherent change.
```

## Small change

```text
Use quick_implementer for this bounded change: <description>.
```

```text
Use quick_implementer to fix this component and its test. Keep the change within that scope.
```

## Larger implementation

```text
Use the agents to understand the problem and implement the solution. Keep architectural decisions in the parent agent.
```

```text
The solution is defined. Use implementer to apply it to the affected files, run the relevant tests, and report the result.
```

## Mixed-stack repository

```text
Use separate agents to analyze the mobile, web, and backend components. Compare their contracts, then integrate one coherent solution.
```

```text
Analyze the Flutter and SvelteKit components in parallel. Do not assign the same files to more than one implementation agent.
```

## Review local changes

```text
Use code_reviewer to review all current changes. Fix concrete problems and verify the result.
```

```text
Use code_reviewer to inspect relevant staged, unstaged, and untracked files. Focus on behavioral and documented-standard violations.
```

## Compare with a branch or commit

```text
Use code_reviewer to compare this branch with main. Fix concrete problems and verify the result.
```

```text
Use code_reviewer to review the changes from v1.4.0 through HEAD.
```

## Review against requirements

```text
Use code_reviewer to check correctness, regressions, project standards, and the requirements I provided.
```

```text
Use code_reviewer to compare this work with main. The specification is in docs/specs/import-flow.md. Check for missing requirements, partial implementations, and unnecessary scope.
```

## Review UI

```text
Use code_reviewer and $devibify to review the UI changes. Fix concrete problems without redesigning unrelated parts of the interface.
```

## Implement UI

```text
Use the agents and $devibify to implement this UI: <description>.
```

```text
Use quick_implementer and $devibify to fix this component without changing the rest of the page.
```

## Request a commit message

```text
Analyze the diff and return one Conventional Commits-compliant message. Do not stage, commit, or push.
```

## Publish an optional commit

These prompts require `commit_publisher`.

```text
Verify the changes and use commit_publisher to create a commit containing the files for this feature. Do not push.
```

```text
Verify the changes and use commit_publisher to create a commit and push it to the configured upstream.
```
