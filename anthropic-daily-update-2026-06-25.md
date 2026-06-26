# Anthropic daily briefing — 2026-06-25

_Briefing window: 2026-06-24 11:05 UTC → 2026-06-25 11:05 UTC (~24 hours)._

Prior briefing: `anthropic-daily-update-2026-06-24.md` (window end 2026-06-24 11:05 UTC). No gap detected.

---

## Headline

**Anthropic accused Alibaba of running the largest known distillation attack against Claude in a letter to White House officials and US senators**, first reported by Bloomberg and picked up by Reuters, CNBC, WSJ, and others on the evening of 2026-06-24. Per the reporting, Alibaba-Qwen-affiliated operators ran ~28.8 million exchanges through ~25,000 fraudulent accounts between **2026-04-22 and 2026-06-05**, targeting "Mythos Preview"–level capabilities. Anthropic has not (yet) published a public blog post on this incident — the story is based on a letter obtained by Bloomberg, not a Newsroom item. Below are the day's other concrete shifts.

Three other concrete movements in window:

1. **Claude Code v2.1.190 and v2.1.191 shipped** — v2.1.191 is meaningful (new `/rewind` command, ~37% streaming CPU reduction, several agent/hook fixes).
2. **Python SDK v0.112.0 and TypeScript SDK v0.106.0 shipped** — both add `system.message` streaming events, a new refusal category, and a `User Profile ID` request header (likely tied to the upcoming Jul 8 identity-verification cutover).
3. **Two new Opus 4.8 reliability incidents** — the longest is ~100 min on Jun 24 morning UTC, continuing the Opus 4.8 noise pattern from earlier this week.

No new anthropic.com/news, /engineering, /research posts; no model launches; no pricing changes.

---

## Claude Code

Two releases in window. Both went out Jun 24.

- **v2.1.191** (`anthropics/claude-code`, 2026-06-24 21:58 UTC) — substantive. Highlights from the [CHANGELOG](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md):
  - **New: `/rewind` support for resuming a conversation from before `/clear` was run.** Notable because `/clear` has historically been destructive — `/rewind` gives you a recovery path. Worth checking if you rely on `/clear` mid-session.
  - **Reduced CPU usage during streaming responses by ~37%** by coalescing text updates to 100ms. A measurable client-side perf win for anyone running Claude Code in long sessions or on battery.
  - **Sandbox network permission dialog**: hosts you allow with "Yes" are now remembered for the rest of the session instead of re-prompting on every connection. Long-requested UX fix.
  - **MCP reliability**: capability discovery (`tools/list`, `prompts/list`, `resources/list`) now retries transient network errors with short backoff; OAuth discovery/token retries once; HTTP 404 errors now show the URL and point to your MCP config. Cumulative quality-of-life win if you operate flaky MCP servers.
  - **Hook fix**: hooks with comma-separated matchers (e.g. `"Bash,PowerShell"`) silently never fired — now they do. If you've been wondering why a multi-tool hook seemed dead, this is why.
  - **Background-agent fixes**: stopping an agent from the tasks panel is now permanent (previously it could resurrect); `claude agents` no longer sends builtin slash commands as prompt text to background sessions; pasted-image placeholders no longer leak filesystem paths in job rows.
  - Plus the usual long tail of cursor/terminal/Ghostty/Windows Terminal fixes.
- **v2.1.190** (2026-06-24 15:53 UTC) — "Bug fixes and reliability improvements" only. Skip ahead to v2.1.191.

Most-recent commit on `main` is the v2.1.191 CHANGELOG/feed update.

---

## API & models

### SDK releases (paired)

Both SDKs were cut **2026-06-24 ~18:46 UTC**, exposing the same three underlying API surface changes:

- **Python SDK v0.112.0** ([release notes](https://github.com/anthropics/anthropic-sdk-python/releases/tag/v0.112.0), published 2026-06-24 18:45 UTC)
- **TypeScript SDK v0.106.0** ([release notes](https://github.com/anthropics/anthropic-sdk-typescript/releases/tag/sdk-v0.106.0), published 2026-06-24 18:48 UTC)

Both contain:

1. **`system.message` streaming events** — new server-sent event type. Previously the streaming spec only emitted assistant content blocks and message-level deltas. A `system.message` event suggests the model now emits in-band system-level signals during a stream (e.g. policy notices, refusal-related notes, or runtime advisories that aren't part of the assistant response). If you have a custom streaming parser, you'll need to handle this event type or you may log "unknown event" warnings.
2. **New refusal category** — listed under "Chores" so likely an enum addition rather than a behavior change. If you discriminate on refusal categories in your eval or routing code, regenerate your enums.
3. **`User Profile ID` request header** — new request-header support. Strongly suggestive that the Jul 8 KYC enforcement (identity verification rollout) requires API callers to forward a per-end-user identifier for traceability. Worth verifying with the docs once they update — this is the most actionable item in this release for downstream operators.

Python SDK also includes a memory-tool bug fix (parent directories were being created with the wrong permissions, [#135](https://github.com/anthropics/anthropic-sdk-python/issues/135)).

### Status / reliability

Two new incidents in window, both on Opus 4.8 (continuing the cluster from earlier this week):

- **Elevated error rate on Claude Opus 4.8** — investigating 13:16 UTC, resolved 14:56 UTC on 2026-06-24 (~100 min). [Incident](https://status.claude.com/incidents/8b0rggdfh1hv).
- **Elevated errors on Opus 4.8 Fast** — investigating 18:22 UTC, resolved 18:33 UTC on 2026-06-24 (~11 min). [Incident](https://status.claude.com/incidents/wkzf38t1yh8z).

The Mythos/Fable 5 export-control suspension ([s9w82lp9dcn9](https://status.claude.com/incidents/s9w82lp9dcn9)) remains in effect.

No API spec changes detected beyond what's exposed via the SDK release.

---

## Research & engineering

**Nothing new on [anthropic.com/news](https://www.anthropic.com/news), [/research](https://www.anthropic.com/research), or [/engineering](https://www.anthropic.com/engineering) in window.**

The most-recent Newsroom item is still "[Introducing Claude Tag](https://www.anthropic.com/news/introducing-claude-tag)" (2026-06-23). The most-recent Engineering item is still "[How we contain Claude across products](https://www.anthropic.com/engineering/how-we-contain-claude)" (undated featured), with last dated post 2026-04-23.

Worth flagging: despite the Alibaba accusation reaching mainstream coverage, Anthropic has **not yet published a first-party post about it** — the closest existing first-party reference is the Feb 23 2026 post "[Detecting and preventing distillation attacks](https://www.anthropic.com/news/detecting-and-preventing-distillation-attacks)" (which named DeepSeek, Moonshot, and MiniMax but not Alibaba). If a public Anthropic post drops in the next 24h, it'll be in tomorrow's briefing.

---

## Rumors & Leaks

### Anthropic accuses Alibaba of "largest known" distillation attack (Bloomberg → Reuters → WSJ/CNBC, 2026-06-24 evening)

This is the most material item in window. Compressed details:

- **Source chain**: First reported by **Bloomberg** with exclusive access to a letter Anthropic sent to **U.S. senators and White House officials**. Reuters wire copy hit at **2026-06-24 ~20:30 UTC**; WSJ ("Brazen Campaign") and CNBC followed. [Stocktwits summary](https://stocktwits.com/news-articles/markets/equity/anthropic-writes-to-white-house-accusing-alibaba-of-illicitly-accessing-claude-ai-models/cZKyprTR7Qd) consolidates the Bloomberg-sourced quotes most accessibly given the Reuters domain is paywalled / blocked.
- **Specific claims**:
  - **~28.8 million exchanges** with Claude via **~25,000 fraudulent accounts** between **2026-04-22 and 2026-06-05**.
  - Attributed to operators "affiliated with Alibaba and Alibaba Qwen, Alibaba's AI lab."
  - Method: "**adversarial distillation**" — described as the largest known distillation attack against Anthropic to date (versus the Feb 2026 disclosure of DeepSeek at >150k, Moonshot at >3.4M, MiniMax at >13M exchanges).
  - Target: capabilities consistent with **Mythos Preview**, per Reuters' framing.
  - Anthropic's letter, quoted by Bloomberg: _"These distillation attacks are carried out illicitly, systematically, and at an industrial scale to harvest US AI capabilities across frontier labs and repackage them as their own without incurring the training and R&D costs required to train US frontier models."_
- **Anthropic's ask**: "more stringent enforcement and defensive frameworks to prevent foreign tech labs from systematically stripping intellectual property from domestic AI research firms" (paraphrase from Stocktwits/Bloomberg summary).
- **Alibaba response**: none in any of the reporting reviewed.
- **Market reaction**: BABA stock dropped ~3% on 2026-06-24.
- **HN reaction**: front-page thread "[Anthropic says Alibaba illicitly extracted Claude AI model capabilities](https://news.ycombinator.com/item?id=48664814)" (Reuters URL, posted 2026-06-24 19:48 UTC; **450 points / 785 comments at briefing time**, climbing fast). Top comments cluster around three takes:
  1. Mechanics of the underlying Chinese proxy/reseller ecosystem — high-vote technical comment from `tristanj` explains the "70-90% below API price" Chinese reseller economy and ties identity verification rollout to this same problem.
  2. Hostility toward Anthropic on IP-symmetry grounds ("Anthropic scraped the web ignoring robots.txt, so this is ironic").
  3. Skepticism that any of this is meaningfully different from how labs already train on each other's outputs.
- **Why this matters operationally**:
  - This is the **first time Anthropic has named Alibaba** as a distillation actor. It re-opens the Feb 2026 distillation playbook and extends it to a much larger entity with cloud-provider relationships.
  - The **`User Profile ID` request header** added to today's SDK releases is almost certainly part of the response — pushing per-end-user identifiers through the API to make hydra-cluster attribution easier on the backend, and to tie API access to identity-verified end users when KYC enforcement begins on **2026-07-08**.
  - If you operate a product that intermediates Claude (a SaaS reseller, an agent platform with end-users, etc.), expect Anthropic to require you to forward `User Profile ID` for each end user in the not-too-distant future. Start planning that integration now.
  - Counter-perspective: Anthropic has framed this story to maximize regulatory leverage in DC at exactly the moment they're contesting the Jun 12 Mythos/Fable 5 export-control directive. The "Alibaba > all prior actors combined" framing and the choice to route the disclosure through White House officials and senators (rather than a public blog post) is consistent with that. Worth watching for whether Anthropic publishes a public companion post over the next 24-72h — if not, treat the letter as an inside-the-Beltway artifact rather than a fully-aired technical disclosure.
- **Picked up by**: Bloomberg (primary), Reuters, WSJ ("Brazen Campaign"), CNBC, Stocktwits, Yahoo Finance, US News, Investing.com, Global Banking & Finance Review, MarketScreener, Euronews-adjacent outlets.

### No other rumors/leaks in window worth flagging

The renewed identity-verification HN discussion has cooled. No new front-page items on the Mythos/classified-systems AP story from yesterday. No new HN front-page Claude items beyond the Alibaba thread.

---

## Other / community

- HN: "[Ask HN: Where is our profession (programmer) going?](https://news.ycombinator.com/item?id=48666559)" (2026-06-25 02:42 UTC, 63 pts / 69 comments at briefing time). Indirect Claude relevance — substantial discussion about agentic coding tools and where Claude Code / Cursor / similar leave human developers. Worth a skim if you're tracking how the engineering community is framing the agentic-coding shift.
- HN: A small Reuters re-post of the Alibaba story at 23:26 UTC (8 pts) was dwarfed by the main thread.

---

## Status (incidents)

Two new Opus 4.8 incidents in window:

- **2026-06-24 13:16-14:56 UTC** — Opus 4.8 elevated error rate (~100 min). [Incident](https://status.claude.com/incidents/8b0rggdfh1hv).
- **2026-06-24 18:22-18:33 UTC** — Opus 4.8 Fast elevated errors (~11 min). [Incident](https://status.claude.com/incidents/wkzf38t1yh8z).

Both fit the Opus 4.8 reliability pattern from Jun 15-22 — the "reliability streak" noted in yesterday's briefing did not hold. The Mythos 5 / Fable 5 export-control suspension remains active.

---

## Excluded but worth knowing

- **The Bloomberg origin article on the Alibaba accusation could not be fetched directly** (paywall + cowork blocklist on `reuters.com`). Above details are reconciled across Stocktwits, Global Banking & Finance Review, and the published HN search-results summary, all of which carry the same quoted passages. The actual letter text has not been published.
- **CNBC's article ([cnbc.com/2026/06/24/anthropic-alibaba-distillation-campaign.html](https://www.cnbc.com/2026/06/24/anthropic-alibaba-distillation-campaign.html))** returned an empty body via fetch — confirmed it exists from search index but couldn't pull content. If you have CNBC access, worth reading for any quote variation.
- A Hugging Face / huggingface.co writeup or model release tied to the Alibaba story would be worth flagging — none surfaced in window. Same for any GLM-5.2 or DeepSeek follow-on commentary linking back to Anthropic.
