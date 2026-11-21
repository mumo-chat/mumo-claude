# mumo — Claude Code plugin

**Multi-model responses + cross-model reactions. Want more rounds? Context carries automatically. Stop when you have what you need.**

Claude, GPT, Gemini, Grok, DeepSeek, Kimi, and more, in parallel. Each model answers in full and then reacts to the others' claims in its own words — typed cross-model reactions, not a merged verdict — for architecture, plan review, pricing, and strategy calls worth checking across labs.

## When to use

Reach for mumo when:

- **Architecture and design reviews** — "Postgres or MongoDB for our event store?" "Is this migration safe under concurrent writes?"
- **Plan and pre-launch pressure tests** — "What would we regret 6 months in?" "Where's the biggest risk in this spec?"
- **Contested product or pricing decisions** — "Should this feature be paid or free?" "Which of these positionings actually converts?"
- **Strategy with real cost** — anywhere a confidently-wrong single model could push a team in the wrong direction for a week.

The bundled skill auto-triggers on this category and stays quiet on factual lookups, routine refactoring, and code generation from a clear spec.

## What's in the box

- **MCP server** — `https://mumo.chat/api/mcp/oauth`, connected with a browser sign-in. Tools: `create_deliberation`, `ask_a_model`, `wait_for_round`, `append_round`, `get_session`, `list_sessions`, `list_models`, `share_session`, `get_credit`.
- **Auto-triggering skill** — `skills/mumo/SKILL.md` tells the agent *when* to reach for the panel so you don't have to

## Install

Inside Claude Code, add this repo as a marketplace and install from it — this always gives you the current release:

```
/plugin marketplace add mumo-chat/mumo-claude
/plugin install mumo@mumo
```

## Sign in

The plugin connects with a browser sign-in. No API key is needed.

1. In Claude Code, run `/mcp`, select `mumo`, and choose **Authenticate**.
2. Your browser opens to **Connect Claude Code to mumo**. Sign in to mumo if asked (or create an account; every account gets free credit each month), confirm your account, and click **Continue**.
3. On the consent screen, click **Allow access**. The screen shows an address on this computer (`localhost`): that is Claude Code waiting for the sign-in to finish.
4. When the browser says the sign-in succeeded, return to Claude Code.

Check the connection with a free call: ask Claude Code to *list mumo's available models*. Claude Code asks before the first use of each mumo tool; choose "always allow" to stop the prompts.

To disconnect later, remove **Claude Code** under **Settings → API Keys → Connected apps** at [mumo.chat/settings/api-keys](https://mumo.chat/settings/api-keys).

### Using an API key instead

For CI or an unattended agent, skip the plugin and add the key server directly, with a key from [Settings → API Keys](https://mumo.chat/settings/api-keys) (keys start with `mmo_live_`):

```
claude mcp add --transport http mumo https://mumo.chat/api/mcp \
  --header "Authorization: Bearer mmo_live_..."
```

This adds the tools without the plugin's skill.

### Upgrading from 0.8.0 or earlier

Earlier versions asked for an API key at install. From 0.8.1 the plugin signs in instead, so after updating, sign in once with the steps above. The key you entered is no longer used by the plugin; it stays valid on your account until you revoke it.

## Using the panel

Invoke the skill explicitly if the agent doesn't reach for it on its own:

- "Run this by a mumo panel"
- "Get me a second opinion from mumo on…"
- "Ask mumo about…"

### Try it first

Ask Claude Code something the panel will want to engage with:

> Postgres or MongoDB for our event store given 50k events/day, a Postgres-experienced team, and a 3-month runway? What would we regret 6 months in?

The first round returns each model's response plus the models' reactions to each other's claims, showing where the panel agrees and where it splits. You can stop there, or `append_round` with your own typed reactions (KEEP / EXPLORE / CHALLENGE / CORE / SHIFT) — moderator attention on what mattered and why.

See [mumo.chat/install](https://mumo.chat/install) for setup and [mumo.chat/docs/mcp](https://mumo.chat/docs/mcp) for the tool reference. The canonical skill lives in [`skills/mumo/SKILL.md`](skills/mumo/SKILL.md) — that's what Claude Code loads.

## Links

- Product — https://mumo.chat
- MCP reference — https://mumo.chat/docs/mcp
- REST API — https://mumo.chat/docs/api
- Issues — https://github.com/mumo-chat/mumo-claude/issues
- Privacy — https://mumo.chat/legal/privacy
- Terms — https://mumo.chat/legal/terms

## License

MIT
