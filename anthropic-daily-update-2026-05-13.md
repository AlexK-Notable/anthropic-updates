# Anthropic Daily Briefing — 2026-05-13

Window: items published, released, or substantively updated between roughly 2026-05-12 09:00 UTC and 2026-05-13 09:00 UTC.

Headline: **Claude For Legal** is the day's big launch (broad press, open-source plugin repo published). Smaller items: Claude Code v2.1.140 polish release, Fast mode now supports Opus 4.7 on the API, an NYT update that pushes the rumored fundraise to $950B, and a disclosed-and-patched Claude Code RCE worth flagging if you run anything older than 2.1.118.

---

## Claude Code

- **Claude Code v2.1.140** — *2026-05-12 21:09 UTC* — [GitHub release](https://github.com/anthropics/claude-code/releases/tag/v2.1.140)
  Polish/fixes release on top of yesterday's 2.1.139. Worth knowing:
  - Agent tool's `subagent_type` is now case- and separator-insensitive — `"Code Reviewer"` resolves to `code-reviewer`. Less surprising for users typing display-style names.
  - `/goal` no longer hangs silently when `disableAllHooks` or `allowManagedHooksOnly` is set; shows a clear message. (Bug introduced when `/goal` shipped in 2.1.139.)
  - Fix for spurious `ConfigChange` hooks when `settings.json` is a symlink.
  - `claude --bg` no longer errors with "connection dropped mid-request" against an idle-exiting background service; startup timeout extended for enterprise endpoint-security setups.
  - Remote managed settings retry once on 401 with a force-refreshed token; `extraKnownMarketplaces` auto-update now actually persists.
  - Windows: event-loop stall fixed when a missing exe (e.g. `gh`) caused synchronous `where.exe` re-spawns on every check.
  - `Read` tool now tolerates whitespace-padded or `+`-prefixed string `offset` values.
  - Plugins emit a warning in `/doctor`, `claude plugin list`, and `/plugin` when a default folder (e.g. `commands/`) is being silently ignored because `plugin.json` already sets the matching key. Helpful for new plugin authors who set keys and then wonder why their colocated files don't load.

  *Why it matters:* No new headline features — this is a stability follow-up to yesterday's Agent View / `/goal` release. The hot-reload-symlink and `--bg` fixes are the ones most likely to affect day-to-day use. If you author plugins, also run `/doctor` after upgrading and look for the new shadowed-folder warning.

- **Update on prior item — Claude For Legal as a Claude Code plugin suite (open-sourced)** — [github.com/anthropics/claude-for-legal](https://github.com/anthropics/claude-for-legal) — *most recent push 2026-05-13 05:12 UTC; repo created 2026-04-21, made active today*
  Apache-2.0, 1,134 stars at time of writing. Practice-area plugins (`commercial-legal`, `corporate-legal`, `employment-legal`, `privacy-legal`, `product-legal`, `regulatory-legal`, `ai-governance-legal`, `ip-legal`, `litigation-legal`), named agents (Vendor Agreement Reviewer, NDA Triager, DSAR Responder, DPA Reviewer, Termination Reviewer, Trademark Clearance Screener, etc. — dozens), managed-agent cookbooks for scheduled "eyes-on-the-feed" workflows, and MCP connectors for both general productivity (Slack/Drive/Box) and legal-specific systems (Ironclad, DocuSign, iManage, Everlaw, CourtListener…). Same artifacts ship as Cowork plugins, Claude Code plugins, and through the Managed Agents API — "same system prompt, same skills — you choose where it runs."
  *Why it matters:* As a practitioner, this is the most concrete reference design Anthropic has published for skill + plugin + managed-agent + MCP-connector composition. Even if you don't do legal, the patterns in `CLAUDE.md` practice-profile + cold-start interview + named agents + scheduled "watcher" agents are reusable; the connector layout shows the in-Anthropic conventions for plugin-shipped MCP servers.

---

## Research

Nothing new. Last research-page entries remain dated 2026-05-07 / 2026-05-08 (NLA, Teaching Claude Why, alignment-tool donation, policy post) — all covered in prior briefings. Engineering blog also unchanged in window.

---

## Rumors & Leaks

- **Update on prior item — Anthropic fundraise rumor moves from $900B (Bloomberg/TC, late April) to up to $950B (NYT, 2026-05-12)** — [NYT: Anthropic in Talks to Raise Funding at a $950 Billion Valuation](https://www.nytimes.com/2026/05/12/technology/anthropic-funding-950-billion-valuation.html) (paywall, summary via [HN thread](https://news.ycombinator.com/item?id=48116014)) — *2026-05-12*
  Material change vs. prior reporting: target valuation rose from $900B (Bloomberg, 2026-04-29; TechCrunch follow-up 2026-04-30) to "up to $950B"; round size still reported in the $30B–$50B range; Bloomberg's same-day piece [Anthropic In Talks to Raise $30 Billion at $900 Billion Valuation](https://www.bloomberg.com/news/articles/2026-05-12/anthropic-in-talks-to-raise-30-billion-at-900-billion-valuation) sits at the lower end of that band, suggesting the deal is still being negotiated. NYT cites a public listing as early as October 2026.
  *Why it matters:* If accurate, this is the round funding the compute deals (Amazon up to $25B in April, SpaceX in May, AWS GA on 2026-05-11). Capacity outlook for API customers continues to be tied to this trajectory.

- **Anthropic publishes updated warning about unauthorized secondary-market stock sales** — [support.claude.com: Unauthorized Anthropic stock sales and investment scams](https://support.claude.com/en/articles/13704655-unauthorized-anthropic-stock-sales-and-investment-scams) — *original 2026-02-11; updated 2026-05-12* — surfaced on [HN](https://news.ycombinator.com/item?id=48112190) (18 pts, 7 comments)
  The update names eight specific entities (Open Door Partners, Unicorns Exchange, Forge, etc.) as unauthorized, declares all such sales void absent board approval, and warns about SPV/indirect-access pitches. Related HN coverage: [Anthropic warns against secondary platforms](https://news.ycombinator.com/item?id=48113182).
  *Why it matters:* Not directly engineering-relevant, but the update's timing (same day as the NYT $950B leak and the Claude For Legal launch) reads as defensive against scam activity that tracks valuation news. Worth knowing if anyone on your team has been pitched "Anthropic shares" by a secondary marketplace — it's almost certainly fraud.

---

## API & Models

- **Fast mode now supports Claude Opus 4.7 (research preview)** — [Platform release notes, 2026-05-12](https://platform.claude.com/docs/en/release-notes/overview)
  Set `speed: "fast"` with `model: "claude-opus-4-7"` and beta header `fast-mode-2026-02-01`. Pricing, rate limits, and access are stated to match Opus 4.6 fast mode (which was up to ~2.5× faster output at premium pricing — see [Fast mode docs](https://platform.claude.com/docs/en/build-with-claude/fast-mode)). Still gated — [waitlist](https://claude.com/fast-mode).
  *Why it matters:* The first speed-tier expansion for Opus 4.7 since its 2026-04-16 launch. If you're already on the 4.6 fast-mode waitlist for latency-sensitive agentic loops, this is the path to 4.7-quality outputs at fast-mode throughput; benchmark before committing — Opus 4.7 has API breaking changes vs. 4.6 (notably no manual `thinking.budget_tokens`, adaptive thinking recommended).

---

## Other

- **Claude For Legal — formal product launch** — *2026-05-12* — broad press coverage; no first-party `anthropic.com/news` post located at briefing time. Best summaries: [Bloomberg](https://www.bloomberg.com/news/articles/2026-05-12/anthropic-expands-push-into-legal-industry-with-new-ai-tools), [TechCrunch](https://techcrunch.com/2026/05/12/the-ai-legal-services-industry-is-heating-up-anthropic-is-getting-in-on-the-action/), [LawNext](https://www.lawnext.com/2026/05/anthropic-goes-all-in-on-legal-releasing-more-than-20-connectors-and-12-practice-area-plugins-for-claude.html), [Artificial Lawyer](https://www.artificiallawyer.com/2026/05/12/claude-for-legal-launches-may-reshape-the-legal-tech-world/), [Fortune](https://fortune.com/2026/05/12/anthropic-legal-plug-in-release-claude-cowork-big-law/). Also see [Thomson Reuters' joint release](https://www.prnewswire.com/news-releases/thomson-reuters-and-anthropic-expand-partnership-to-connect-claude-with-cocounsel-legal-302769890.html) extending the CoCounsel partnership.
  What shipped: 12 practice-area plugins (M&A diligence, employment, privacy, product, AI governance, IP, litigation, etc.); 20+ MCP connectors (DocuSign, Box, Ironclad, iManage, NetDocuments, LexisNexis, Thomson Reuters/Westlaw, Everlaw, LSuite, plus the open-source CourtListener and a Harvey integration); Microsoft 365 add-ins (Word, Outlook, Excel, PowerPoint) carrying context across apps; named-launch firms Freshfields, Quinn Emanuel Urquhart & Sullivan, Holland & Knight, Crosby Legal; access-to-justice partnerships with Free Law Project, Justice Technology Association, Courtroom5 (pro-se litigants), BoardWise.
  *Why it matters:* Two practitioner takeaways beyond the legal vertical itself: (1) the plugin/skill/managed-agent/MCP-connector composition pattern is now a publicly auditable reference design (see the open-source repo above); (2) the "same artifacts run in Cowork, in Claude Code, or via Managed Agents API" claim is a useful pattern for shipping internal tooling that needs both an IDE-style and a managed-headless deployment from one source.

- **Disclosed Claude Code RCE via deeplink + settings injection (patched in 2.1.118)** — [0day.click writeup by @joernchen, 2026-05-12](https://0day.click/recipe/2026-05-12-cc-rce/) — surfaced on [HN](https://news.ycombinator.com/item?id=48107213)
  Pre-2.1.118 versions parsed CLI flags eagerly with `startsWith()`, letting a crafted `claude-cli://open` deeplink inject `--settings` into the `--prefill` value, which in turn injected an arbitrary settings file containing command hooks executed on launch. Particularly nasty: if the deeplink's `repo` parameter pointed at a repo the user had already cloned and trusted (e.g. `anthropics/claude-code`), execution happened without prompts. Anthropic was notified, patched the issue, and shipped the fix before disclosure; the public writeup is post-fix.
  *Why it matters:* If your team has any pinned/managed installs older than 2.1.118, ensure they're upgraded; the disclosure includes a working trigger. The class of bug (CLI argument parsing across the deeplink/IPC trust boundary) is also worth keeping in mind for anyone building MCP clients or other agent-launching desktop apps.

---

## Excluded due to recency filter — but worth a look

- HN community tooling that didn't break double digits but is on-topic — [Dragoman (multi-model routing for Claude Code via sub-agents)](https://news.ycombinator.com/item?id=48110268), [agent-dash (TUI for Claude Code in tmux)](https://news.ycombinator.com/item?id=48118041), [Graphmind (persistent memory MCP for Claude Code)](https://news.ycombinator.com/item?id=48118017), [CC-Ledger (per-session/PR cost tracker)](https://news.ycombinator.com/item?id=48112700), [Atlas (local-first AI code reviewer)](https://news.ycombinator.com/item?id=48110504), [Clodcapture (save/resume Claude.ai chats)](https://news.ycombinator.com/item?id=48115739).
- [Statewright (visual state machines for AI agents)](https://news.ycombinator.com/item?id=48108778) — 94 pts, not Anthropic-specific but front-page-adjacent; broadly applicable to Claude-agent reliability work, so worth a look if you're hardening long-horizon workflows.
- The [news.sap.com SAP × Anthropic announcement](https://news.sap.com/2026/05/sap-anthropic-to-bring-claude-sap-business-ai-platform/) surfaced in search — confirm timestamp before treating as in-window; SAP Sapphire week.
