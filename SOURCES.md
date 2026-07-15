# Sources and references

The following sources informed the kit's design and review workflow.

## OpenAI Codex

- [Custom subagents](https://learn.chatgpt.com/docs/agent-configuration/subagents)
- [AGENTS.md hierarchy](https://learn.chatgpt.com/docs/agent-configuration/agents-md)
- [Basic configuration](https://learn.chatgpt.com/docs/config-file/config-basic)
- [Advanced configuration](https://learn.chatgpt.com/docs/config-file/config-advanced)
- [Codex models](https://learn.chatgpt.com/docs/models)

## Referenced skills

- Matt Pocock's [`code-review` skill](https://github.com/mattpocock/skills/tree/main/skills/engineering/code-review) and its [`SKILL.md` source](https://raw.githubusercontent.com/mattpocock/skills/main/skills/engineering/code-review/SKILL.md)
- theRolento's [`devibify` repository](https://github.com/theRolento/devibify) and its [`SKILL.md` source](https://raw.githubusercontent.com/theRolento/devibify/main/SKILL.md)

The kit does not include or require `code-review`. Its reviewer prompt adapts general ideas from that workflow without copying the skill.

The kit does not include `devibify`. Agents invoke it for visible UI work when the user has installed it.
