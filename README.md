# mumo — Claude Code + Cowork plugin

**Multi-model responses + cross-model reactions. Want more rounds? Context carries automatically. Stop when you have what you need.**

Claude, GPT, Gemini, Grok, Qwen, GLM, Kimi in parallel. For contested decisions — architecture, plan review, pricing, strategy — where a single model might be confidently wrong.

Works in both Claude Code (developer audience) and Claude Cowork (knowledge-worker audience). Same plugin, same skill, different distribution marketplaces.

## When to use

Reach for mumo when:

- **Architecture and design reviews** — "Postgres or MongoDB for our event store?" "Is this migration safe under concurrent writes?"
- **Plan and pre-launch pressure tests** — "What would we regret 6 months in?" "Where's the biggest risk in this spec?"
- **Contested product or pricing decisions** — "Should this feature be paid or free?" "Which of these positionings actually converts?"
- **Strategy with real cost** — anywhere a confidently-wrong single model could push a team in the wrong direction for a week.

The bundled skill auto-triggers on this category and stays quiet on factual lookups, routine refactoring, and code generation from a clear spec.

## What's in the box

- **MCP server** — `https://mumo.chat/api/mcp`. Tools: `create_deliberation`, `wait_for_round`, `append_round`, `get_session`, `list_sessions`, `list_models`, `get_credit`.
- **Auto-triggering skill** — `skills/mumo/SKILL.md` tells the agent *when* to reach for the panel so you don't have to

## Install — Claude Code

Inside Claude Code:

```
/plugin marketplace add anthropics/claude-plugins-official
/plugin install mumo
```

## Install — Claude Cowork

Open Claude Desktop → **Cowork** tab → **Customize** → **Browse plugins** → search for `mumo` → **Install**. Or browse the full catalog at [claude.com/plugins](https://claude.com/plugins/).

> **Note:** Claude Code and Cowork have separate plugin panels backed by different marketplaces. Installing in one surface does **not** auto-install in the other — install in each separately.

## API key setup

The plugin prompts for your mumo API key at install time and stores it securely in your system keychain.

1. Sign up at https://mumo.chat and create a platform key at [Settings → API Keys](https://mumo.chat/settings/api-keys) (keys start with `mmo_live_`)
2. Paste it when the plugin prompts you during install/enable

If you need to update the key later, reinstall or reconfigure the plugin. For manual or dev setups, you can also export `MUMO_API_KEY` as an environment variable.

## Using the panel

Invoke the skill explicitly if the agent doesn't reach for it on its own:

- "Run this by a mumo panel"
- "Get me a second opinion from mumo on…"
- "Ask mumo about…"

### Try it first

Ask Claude Code or Cowork something the panel will want to engage with:

> Postgres or MongoDB for our event store given 50k events/day, a Postgres-experienced team, and a 3-month runway? What would we regret 6 months in?

The first round returns each model's prose plus a cross-model claim map showing where the panel agrees and where it splits. You can stop there, or `append_round` with typed snippets (KEEP / EXPLORE / CHALLENGE / CORE / SHIFT) — moderator attention on what mattered and why.

See [mumo.chat/install](https://mumo.chat/install) for setup and [mumo.chat/docs/mcp](https://mumo.chat/docs/mcp) for the tool reference. The canonical skill lives in [`skills/mumo/SKILL.md`](skills/mumo/SKILL.md) — that's what Claude Code loads.

## Links

- Product — https://mumo.chat
- MCP reference — https://mumo.chat/docs/mcp
- REST API — https://mumo.chat/docs/api
- Issues — https://github.com/mumo-chat/mumo-claude/issues

## License

MIT
