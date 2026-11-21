# Changelog

## 0.9.0 — 2026-11-21

Coordinated client release rendered from the mumo-mcp 0.9.0 baseline. It goes with mumo MCP server 1.4.0.

- `wait_for_round` results are text only. The second line is `Action: <value>`, with the same five values as before, and the skill now tells the agent to branch on that line. The third line names the session and round. Under the header, each model that failed, was cut off, hit a cap or was cut short by its provider gets an `Issue:` line.
- New section, "Hand the wait to a subagent". Where the client supports subagents, the agent creates the round itself and has a subagent on the same model wait and read. The subagent hands back the round without the full answers, the points no model reacted to, and a short note on tone. It hands back only a finished round, and says so when it could not read the whole result.
- Refusals start with `Error: <code>`.
- `references/operating-notes.md` and `references/synthesis.md` updated to match.
- Setup text: for a delegated wait, a subagent may need to load the mumo tool with ToolSearch before calling it.

## 0.8.1 — 2026-11-21

- The plugin now connects with a browser sign-in. It no longer asks for an API key at install. After installing or updating, run `/mcp`, select `mumo`, choose **Authenticate**, and allow access in the browser.
- The bundled MCP server address is now `https://mumo.chat/api/mcp/oauth`. The tools, billing and limits are the same.
- If you are updating from an earlier version, sign in once. The API key you entered before is no longer used by the plugin and stays valid on your account until you revoke it.
- To keep using an API key (CI, unattended agents), add the key server directly with `claude mcp add`; the README has the command.
- `SKILL.md` setup text and the README updated to match.

## 0.8.0 — 2026-11-20

Coordinated client release rendered from the mumo-mcp 0.8.0 baseline.

- The skill says "reactions" where it said "claim map", and calls the moderator's quote-plus-type items "your reactions". The `append_round` parameter is still `snippets`, and the `claim_map` field keeps its name.
- Reading the reactions: the read tools print reacted-to claims in four groups. Contested (a model challenged it), Open (a model wants it explored), Pivotal (a model called it central, or it changed a model's frame) and Kept (a model kept it). Agreement between models is not verification.
- The loop: summarize each round for the user and agree before running another. `append_round` needs a `prompt`. Wait with both the `session_id` and the `round_id`, and reuse the returned `round_id` after a timeout. An identical `append_round` call within 24 hours returns the first acknowledgement, so reword the prompt to retry a failed round.
- A family name (`"claude"`, `"gpt"`, `"gemini"`) works as a model. `list_models` is sorted by what your plan can run.
- Request a Takeaway only when the user asks for one.
- `detail: "compact"` on `wait_for_round` and `get_session` shortens long sessions.
- Reference files renamed: `references/claim-maps.md` is now `references/reading-reactions.md`, and `references/snippets.md` is now `references/writing-reactions.md`. The Confidence scores section is removed, because the tools do not print those fields.
- README updated to match.

## 0.7.0 — 2026-11-19

Coordinated client release rendered from the mumo-mcp 0.7.0 baseline.

- New tool: `ask_a_model` puts a question to one model of your choice and returns one answer, with no cross-model reactions or claim map. `SKILL.md` has a new section on when to use it and when to use `create_deliberation` instead.
- `allowed-tools` gains `mcp__plugin_mumo_mumo__ask_a_model`, so asking one model does not trip a permission prompt.
- `SKILL.md` now tells the agent to surface each round's `web_url`. `claim_map_url` still works and carries the same value.
- `server.json` 0.4.0: tool list includes `ask_a_model`.
- README tool list updated.

## 0.6.0 — 2026-09-08

Coordinated client release rendered from the mumo-mcp 0.6.0 baseline.

- `SKILL.md`: `share_session` documented in the tools table; `wait_for_round` taught in its one-id form (`session_id` alone resolves to the round in flight; `round_id` only for an earlier round); intro names the current model families and the typed cross-model reactions.
- `allowed-tools` gains `mcp__plugin_mumo_mumo__share_session` — sharing no longer trips a permission prompt mid-deliberation.
- `server.json` 0.3.0: tool list includes `share_session`.
- `references/operating-notes.md` re-synced with the baseline.
- README tool list and intro updated.

## 0.5.0 — 2026-07-04

Reaction-visibility model + per-round Takeaway.

- New "Reactions are in the ledger, responses are on the record" section: participants never see each other's reactions — each model weaves its own prior reactions into its next response, and the claim map is the moderator's privileged preview. Moderator snippets are the one path that puts ledger content directly in front of the panel.
- Pacing note in "When to continue": positions develop across rounds — a reaction gets woven into the next round's prose, which draws reactions of its own a round later.
- Takeaway: `takeaway: true` (create + append) generates `round_takeaway` (`bottom_line` + `items[]`), the per-round summary. New `references/takeaway.md` (replaces `references/recap.md`).

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
