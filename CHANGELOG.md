# Changelog

## 0.4.0 — 2026-06-19

Skill triggering + prompt-voice updates.

- Triggering: dropped the "contested" gate — the description now leads with pre-implementation review (esp. anything touching auth, security, tokens, payments, data exposure, or migrations), not "contested decisions only."
- `when_to_use` rewritten to match — review-before-implementation framing with the same high-stakes domains.
- Skip rule now keys on "no ambiguity over the correctness of your approach" (was "tasks where the user has already specified exactly what to build").
- Author-bias counter (When to use): if you authored the plan or code under review, that's a reason FOR a panel — the author is the worst-positioned reviewer of their own work.
- New "Prompt voice" section: write the prompt first-person as the operator, not "You are X" case-study framing.
- Surface `claim_map_url` after each round so the user can open the claim map directly.

## 0.3.1 — 2026-05-22

README update.

## 0.3.0 — 2026-05-22

New home and substantial skill content updates. The plugin moves to `mumo-chat/mumo-claude` and the bundled `SKILL.md` picks up the recap capabilities + long-kernel guidance that landed in the canonical mumo skill since 0.2.2.

- **Repo migration.** `plugin.json` and `marketplace.json` `repository` fields now point at `mumo-chat/mumo-claude`. Existing installs continue to work; new installs resolve against the new repo.
- **Recap capabilities** in `SKILL.md` — new opt-in `recap_round` (per-round LLM-curated summary, accepted on `create_deliberation` AND `append_round`) and `recap_session` (session-level synthesis, `append_round` only, cascade-backfills round recaps on prior rounds). New `references/recap.md` covers the mechanics, cost behavior, and cascade rule.
- **`recommended_client_action` 5-value branching table** in the basic loop — agents now branch on the structured 5-value enum (`proceed_with_complete_result` / `proceed_with_partial_result` / `poll_again` / `retry` / `abandon`) the server emits, rather than parsing prose.
- **New long-kernel sections** — `Verifying the call actually fired` (recover when an autonomous loop fabricates a tool-call result), `Recovery: lost session context` (when the agent loses session/round IDs mid-conversation), `Reading the claim map` (CORE / KEEP / EXPLORE / CHALLENGE / SHIFT semantics), `Deliberation is advisory` (decision belongs to agent + user, not the panel).
- **`After each round` restructured** as a workflow choice — recap vs prose synthesis path depending on whether `recap_round` was set on the call.
- **`references/` renamed from `reference/`** (plural) so the directory name matches the link targets in `SKILL.md` body.
- **`operating-notes.md`** picks up two new sections: `/progress` for diagnostic polling (ETag-based no-change semantics, per-model partial-round forensics) and `Platform meta in edge cases` (when model-comparative observations or budget context are problem context vs platform meta).

No runtime/API changes. The MCP server, seven tools, and `userConfig` API-key flow are identical to 0.2.2.

## 0.2.2 — 2026-05-05

Removed `agents/mumo-moderator.md`. The agent positioned itself as the moderator role, but moderation is exactly what should stay with the primary agent — it owns the conversational context, the user's intent, and the cross-round steering decisions. A subagent that ran a "complete brief" panel was a thin operational helper at best, and the framing risked the primary agent over-delegating.

- `agents/` directory removed.
- README updated to drop the moderator subagent line.

## 0.2.1 — 2026-05-04

Plugin architecture — mumo now installs and behaves like a native Claude Code capability.

- **API key via keychain**: `userConfig` with `sensitive: true` prompts for the key at install time and stores it in the system keychain. No more `export MUMO_API_KEY` in your shell.
- **No permission prompts**: `allowed-tools` in SKILL.md pre-approves all seven MCP tools. Deliberations run without interruption.
- **Moderator agent**: `agents/mumo-moderator.md` — a dedicated subagent with tool restrictions, high effort, and the mumo skill preloaded. Can run deliberations in an isolated context.
- **Auto-triggering**: `when_to_use` frontmatter gives Claude richer context for deciding when to reach for mumo without being asked.
- **Inline invocation**: `argument-hint` enables passing a question directly when invoking the skill.

## 0.2.0 — 2026-05-04

Architecture rewrite. SKILL.md becomes a lean kernel; detailed guidance moves to on-demand files.

- `skills/mumo/SKILL.md` — rewritten as a compact kernel: deliberation loop, snippet-as-attention doctrine, non-forwarding test, continuation/stop rules, playbook index, user-preferences section. Synthesis guidance deferred to reference.
- `skills/mumo/playbooks/` — four cognitive-shape playbooks: `contested-decision`, `design-review`, `uncertainty-expansion`, `red-team`. Loaded at most one per session when the shape clearly fits.
- `skills/mumo/reference/` — five reference docs: `claim-maps`, `snippets`, `model-selection`, `synthesis`, `operating-notes`. Loaded on demand for extended mechanics.
- `plugin.json` bumped to 0.2.0, homepage changed to `/install`.
- `server.json` simplified to tool list + description, bumped to 0.2.0.
- `wait_for_round` added to README tool list (was missing since 0.1.3).

## 0.1.3 — 2026-04-24

- Added `get_credit` as the sixth MCP tool — fetches the caller's wallet: effective balance, per-bucket breakdown (free / subscription / refill with reset timing + rollover cap + subscription status), autorefill state, and FIFO debit order. Mirrors the new `GET /api/credit` REST endpoint. README tool list and SKILL.md tool map updated to reflect the sixth tool.

## 0.1.2 — 2026-04-23

Skill content update, no runtime behavior change. Mirrors the MCP doc demotion shipping on mumo.chat.

- `skills/mumo/SKILL.md` — dropped the promoted "Modes" (autonomous) and "Surfacing to humans" (distill) sections. Both remain valid API inputs but aren't default-path on MCP, so agents no longer see them as suggested surfaces.
- Default workflow opener no longer passes `rounds: 1` — `single_shot` labeling was artificially constraining for agent loops where rounds unfold organically via `append_round`.
- Tool map renamed the `get_session` row from "Inspect state / poll autonomous" to "Read session state."

## 0.1.1 — 2026-04-22

Listing refresh. No runtime behavior change.

- `plugin.json` description rewritten to the value-prop hero: *"Multi-model responses + cross-model reactions. Want more rounds? Context carries automatically. Stop when you have what you need."* Same string in `server.json` to keep the description sources consistent.
- README gains a bolded hero matching the manifest and a "When to use" block before install.
- Model list now explicitly names all seven providers (Claude, GPT, Gemini, Grok, Qwen, GLM, Kimi) instead of "Claude, GPT, Gemini, Grok, Qwen, and more."

## 0.1.0 — 2026-04-21

Initial release.

- `.claude-plugin/plugin.json` manifest for Claude Code + Cowork distribution.
- `.mcp.json` wiring remote streamable-HTTP MCP server at `https://mumo.chat/api/mcp` with `${MUMO_API_KEY}` bearer auth.
- `server.json` metadata for MCP Registry (`registry.modelcontextprotocol.io`) submission.
- `skills/mumo/SKILL.md` — auto-triggering skill.
