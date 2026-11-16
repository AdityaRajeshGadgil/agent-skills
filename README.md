# Agent Skills

Reusable agent skills packaged for use across projects and machines.

| Skill | Workflow |
| --- | --- |
| [Vivek Review](skills/vivek-review/SKILL.md) | Get a strict YES or NO from a fresh, independent reviewer. Address findings and repeat until the work is ready to ship or a concrete blocker prevents progress. |

## Install on each machine

Run the install commands in the environment where the agent runs. Replace `OWNER` with this repository's GitHub owner. A user-level install makes the plugin available across that agent user's projects.

### Codex CLI

```sh
codex plugin marketplace add OWNER/agent-skills
codex plugin add agent-skills@agent-skills
```

To refresh the marketplace from GitHub, run `codex plugin marketplace upgrade agent-skills`.

### GitHub Copilot CLI

```sh
copilot plugin install OWNER/agent-skills
```

To update, run `copilot plugin update agent-skills`.

## Use

Ask the agent to “Use `vivek-review` to review the completed change before handoff.” The workflow requires a host that can start a fresh reviewer with the same model and reasoning effort as the agent doing the work.
