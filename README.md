# Agent Skills

Portable skills for coding agents, stored as directories under `skills/`. Each directory follows the open [Agent Skills specification](https://agentskills.io/specification).

| Skill | Workflow |
| --- | --- |
| [Vivek Review](skills/vivek-review/SKILL.md) | Get a strict YES or NO from a fresh, independent reviewer. Address findings and repeat until approval or a concrete blocker. |

## Install a skill

Clone this repository on the machine where your agent runs.

```sh
git clone https://github.com/AdityaRajeshGadgil/agent-skills.git
```

Copy the full `agent-skills/skills/vivek-review/` directory into your agent's skill directory. Install it for the user to make it available across projects, or install it in a project to share it with that project's agents.

| Client example | User skill directory | Project skill directory | Explicit invocation | Documentation |
| --- | --- | --- | --- | --- |
| Google Antigravity CLI (`agy`) | `~/.gemini/config/skills/` | `.agents/skills/` | `/vivek-review` | [Antigravity skills](https://antigravity.google/docs/skills?app=antigravity) |
| Claude Code | `~/.claude/skills/` | `.claude/skills/` | `/vivek-review` | [Claude Code skills](https://code.claude.com/docs/en/skills) |
| Codex | `~/.agents/skills/` | `.agents/skills/` | `$vivek-review` | [Codex skills](https://learn.chatgpt.com/docs/build-skills) |

For any other Agent Skills client, use its documented skill directory. The installed file should be `<skill-directory>/vivek-review/SKILL.md`. In a client that takes instructions directly, supply the `SKILL.md` file and any supporting files that the skill references.

For consistent installs across machines, check out the same Git commit before copying. To update a copied skill, pull the desired revision and replace the installed skill directory with the updated copy.

## Use a skill

> Use `vivek-review` to independently review the completed change before handoff.

Vivek Review needs a fresh reviewer that uses the model and reasoning effort you choose for reviews, and the main agent's for any setting you do not choose. The reviewer can run through native delegation or in a separate clean agent session that receives the review packet.

## Optional plugin installs

Clients that support Agent Plugins can install the whole collection using the root `plugin.json`. Use one installation method per client so it discovers a single copy of the skill.

### Codex CLI

```sh
codex plugin marketplace add AdityaRajeshGadgil/agent-skills
codex plugin add agent-skills@agent-skills
```

To refresh the marketplace from GitHub, run `codex plugin marketplace upgrade agent-skills`.

### GitHub Copilot CLI

```sh
copilot plugin install AdityaRajeshGadgil/agent-skills
```

To update, run `copilot plugin update agent-skills`.
