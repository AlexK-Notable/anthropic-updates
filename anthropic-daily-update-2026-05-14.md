# Anthropic Daily Briefing — 2026-05-14

**Window:** items published, released, or substantively updated between roughly 2026-05-13 09:00 UTC and 2026-05-14 09:00 UTC.

**Headline:** Two material first-party items in 24h — **Claude for Small Business** launches with 15 ready-to-run workflows and a wide connector set (282 pts on HN), and **Claude Code v2.1.141** ships a substantial fix-heavy release with several new agentic primitives (`agents --cwd`, "Summarize up to here" in Rewind, hook `terminalSequence`, etc.). Plus an announced **50% bump in Claude Code weekly limits through July 13** (anti-Codex retention play) and a TechCrunch interview with Cat Wu (head of product, Claude Code + Cowork) flagging "proactivity" as the next product frontier.

---

## Claude Code

- **Claude Code v2.1.141** — *2026-05-13 23:19 UTC* — [GitHub release](https://github.com/anthropics/claude-code/releases/tag/v2.1.141) (commit `c5712671`)
  Substantial fix-heavy release with several new agentic-loop primitives on top of yesterday's v2.1.140 polish pass. Notable additions:
  - **Hook `terminalSequence` field** — hooks can now emit desktop notifications, window titles, and bells without a controlling terminal. Useful for headless/SDK invocations and CI watchers.
  - **`CLAUDE_CODE_PLUGIN_PREFER_HTTPS`** — clone GitHub plugin sources over HTTPS instead of SSH for environments without a GitHub SSH key (corporate laptops, ephemeral CI).
  - **`ANTHROPIC_WORKSPACE_ID`** env var — scopes the workload-identity-federation–minted token to a specific workspace when the federation rule covers more than one. Matters for enterprise multi-workspace OIDC setups.
  - **`claude agents --cwd <path>`** — scope the Agent View session list to a directory. Pairs with the per-project triage workflow introduced in v2.1.139.
  - **`/feedback` can attach recent sessions** (last 24h or 7d) for issues spanning more than the current session. Materially better bug reports for cross-session regressions.
  - **Rewind: "Summarize up to here"** — compress earlier context while keeping recent turns intact. New axis on the rewind/compaction surface.
  - **Background agents preserve permission mode** — `/bg` or `←←` launches no longer revert to default permission mode.
  - Spinner warms to amber after 10 seconds during long thinking — small but useful signal that the model is still working.
  - Auto mode permission dialog now explains when a `permissions.ask` rule caused the prompt.
  - File-edit permission prompts: restored "view diff in your IDE" option when an IDE is connected (regression from earlier 2.1.x).

  **Bug fixes worth knowing about:**
  - Bedrock/Vertex/Foundry/gateway: background side-queries no longer send an unavailable Haiku model ID when `ANTHROPIC_SMALL_FAST_MODEL` isn't set; falls back to main-loop model. Affects anyone running Claude Code on a third-party provider.
  - Bedrock: `awsCredentialExport` now always runs when configured (was being skipped when ambient AWS credentials resolved) — fixes cross-account auth.
  - `/model` in one session no longer silently changes the autocompact threshold in concurrent sessions.
  - MCP HTTP/SSE servers returning 403 on connect now show "needs auth" instead of "failed". MCP `${var%pattern}` parameter expansions no longer incorrectly flagged as missing env vars.
  - Remote Control: all-MCP-connectors-401 when worker session token rotated mid-session is fixed; trusted device re-enrollment no longer loops through `/login` on stale token rejection.
  - Windows: Alt+V image paste now finds clipboard screenshots.
  - VSCode: in-chat mic shows "No audio detected" instead of nothing when capture produces only silence; voice mode WSL error now suggests installing `sox libsox-fmt-pulse` for WSLg users.
  - `/tui` no longer silently drops running background shells and subagents — refuses and asks to wait.
  - SDK on Linux: "Claude Code native binary not found" fixed when both glibc and musl platform packages are installed.

  *Why it matters:* The headline-feature density is lower than v2.1.139 but the fix list is large and lands several things practitioners hit daily — third-party-provider Haiku fallback, MCP auth state, `/model` cross-session bleed, and Bedrock cross-account auth. The new hook `terminalSequence` and `--cwd`-scoped Agent View are the small additions most likely to change workflow. Upgrade.

---

## Research

**Nothing new.** No new posts on [anthropic.com/research](https://www.anthropic.com/research) (latest: 2026-05-08 "Teaching Claude Why"), [alignment.anthropic.com](https://alignment.anthropic.com) (latest two are "Teaching Claude Why" and "Model Spec Midtraining," both May 2026 but already in prior briefings), or [anthropic.com/engineering](https://www.anthropic.com/engineering) (latest: 2026-04-23 "An update on recent Claude Code quality reports") in the 24-hour window.

---

## Rumors & Leaks

- **Update on prior item — fundraise reporting recap** — multiple May 13 outlets (Bloomberg via [Bankless Times](https://www.banklesstimes.com/articles/2026/05/13/record-breaking-unicorn-anthropic-may-outpace-openai-to-1t/), [tradingkey.com](https://www.tradingkey.com/analysis/stocks/us-stocks/261889029-anthropic-funding-30b-valuation-trillion-claude-code-revenue-growth-ipo-spacex-colossus-tradingkey)) recapped the NYT $950B / Bloomberg $900B reporting from 2026-05-12 (covered in yesterday's briefing). **No material new development**; valuation band still $900B–$950B, round size still $30B–$50B, end-of-month close still reported as the target. One stray new datapoint: Bankless Times cites Anthropic annualized revenue at $30B as of April — at odds with the $14B figure reported elsewhere (TFN), so treat as unverified.
  *Why it matters:* Not a fresh story; flagged only so a same-day search doesn't read it as new movement.

- **Update on prior item — secondary-market stock-sale warning re-surfaces** — [Axios: Anthropic raises alarm over secondary share sales](https://www.axios.com/2026/05/13/anthropic-alarm-secondary-shares) — *2026-05-13*
  Axios coverage of the same support.claude.com warning Anthropic updated on 2026-05-12 (covered yesterday). No new content; just press pickup.

---

## API & Models

- **Claude Code weekly limits +50% through 2026-07-13** — *announced 2026-05-13* — [@ClaudeDevs on X](https://twitter.com/ClaudeDevs/status/2054639777685934564); HN thread [item 48126429](https://news.ycombinator.com/item?id=48126429) (9 pts, low traction so far); commentary at [pasqualepillitteri.it](https://pasqualepillitteri.it/en/news/2494/claude-code-weekly-limits-50-percent-anti-codex-anthropic-2026)
  Live for Pro, Max, Team, and seat-based Enterprise; free plan excluded. No opt-in required. Stacks with the 2026-05-06 doubling of 5-hour limits and the 2026-05-07 SpaceX/Colossus 1 compute deal. Promo runs through 2026-07-13 (exactly two months).
  *Why it matters:* Third coordinated rate-limit move in a month (2026-04-16 "Claude 2x" promo → 2026-05-06 hourly-limit double + peak-hours removal → today's weekly +50%). The reported motivation is retention against OpenAI Codex, which converts at lower token-per-task rates; if you've been near the weekly cap and paying for the Codex equivalent, this widens the practical gap. Time-boxed so plan around the July 13 expiry.

- **Platform release notes:** [no new entries](https://platform.claude.com/docs/en/release-notes/overview) since the 2026-05-12 Fast-mode-supports-Opus-4.7 entry (covered yesterday).

---

## Other

- **Claude for Small Business** — *2026-05-13* — [anthropic.com/news/claude-for-small-business](https://www.anthropic.com/news/claude-for-small-business) — HN: [item 48130950](https://news.ycombinator.com/item?id=48130950) (282 pts, 207 comments)
  Cowork toggle install that bundles 15 ready-to-run agentic workflows and 15 skills across finance, ops, sales, marketing, HR, and customer service. Headline workflows: payroll planning, monthly financial reconciliation, business-performance dashboards, campaign management. Connector lineup is the QuickBooks/PayPal/HubSpot/Canva/DocuSign/Google-Workspace/Microsoft-365 stack — small-business default tools, not the enterprise/legal connector universe shipped with [Claude For Legal](https://github.com/anthropics/claude-for-legal) yesterday. A "Claude SMB Tour" with in-person workshops starts 2026-05-14 across multiple cities; attendees get a one-month Claude Max trial. No specific pricing for the SMB bundle stated in the announcement.
  *Why it matters:* Second vertical product packaging in 48 hours (Legal yesterday, SMB today), both shipped as Cowork plugin bundles + skills. The pattern is identical to Legal — practice-area-style plugins, a curated MCP-connector set, and managed-agent workflows — confirming that "skill + plugin + connector + managed-agent" is now Anthropic's standard composition unit for verticalized offerings. Top HN comment ([CSMastermind](https://news.ycombinator.com/item?id=48130950)) frames the play as "Excel for coding agents — the killer app is a UI that lets non-engineers actually use them." Useful pattern reference even if SMB isn't your market.

- **TechCrunch interview with Cat Wu (Anthropic head of product, Claude Code + Cowork)** — *2026-05-13 12:28 PDT* — [TechCrunch](https://techcrunch.com/2026/05/13/anthropics-cat-wu-says-that-in-the-future-ai-will-anticipate-your-needs-before-you-know-what-they-are/) (also picked up by [Digit.in](https://www.digit.in/news/general/anthropic-executive-says-future-ai-may-anticipate-your-needs-before-you-even-ask.html))
  Wu lays out a three-phase product roadmap framing — synchronous → routines → proactivity — and identifies "proactivity" (Claude understands what you work on and sets up automations without being asked) as the next six-month focus. Notes Anthropic shipped ~six models in the last year and expects the pace to accelerate; describes selective deployment as a feature, citing the gated Mythos / Project Glasswing release model.
  *Why it matters:* Most concrete public-facing roadmap signal in weeks. If you're building on top of Claude Code or the Managed Agents API, this is the framing to plan against: expect more first-class "schedule/observe/act" primitives, more model-release frequency, and more verticalized gated previews on the Mythos pattern. Pair with [Routines on Claude Code on the web](https://code.claude.com/docs/en/whats-new) (Week 16, mid-April) and the [Multiagent sessions / Outcomes betas](https://platform.claude.com/docs/en/release-notes/overview) (2026-05-06) — those are the existing surface area the "proactivity" framing builds on.

---

## Excluded due to recency filter — but worth a look

- **[Futurism: "Anthropic Says Claude Turned Evil for a Bizarre Reason"](https://futurism.com/artificial-intelligence/anthropic-claude-evil-internet-blame)** (2026-05-13) — derivative commentary on the [Teaching Claude Why](https://www.anthropic.com/research/teaching-claude-why) research (2026-05-08, already covered). Not new research; reads as a critical take on Anthropic's "internet text portraying AI as evil influenced model conduct" framing. Skip unless you want the press-narrative angle.
- **[9to5Mac: Anthropic's latest Claude release turns your Mac into a small business powerhouse](https://9to5mac.com/2026/05/13/anthropics-latest-claude-release-turns-your-mac-into-a-small-business-powerhouse/)** and [SiliconANGLE](https://siliconangle.com/2026/05/13/anthropic-launches-claude-small-business-new-automation-workflows/) — derivative coverage of the SMB launch.
- **[News.Bitcoin.com: Bitcoiner recovers 5 BTC using Claude](https://news.bitcoin.com/bitcoiner-dumps-old-computer-files-into-claude-ai-recovers-5-btc-lost-since-2015/)** — single-user anecdote about Claude debugging `btcrecover` to crack a forgotten passphrase on a P2PKH wallet. Color piece, not engineering-relevant, but plays into the "agentic debugging on unfamiliar tooling" narrative if you're collecting examples.
- **[IBTimes / DesignTAXI: Claude AI intermittent glitches on 2026-05-13](https://www.ibtimes.com.au/anthropic-claude-ai-stability-surging-demand-1868742)** — community-sourced outage reports, no first-party confirmation on the [Anthropic status page](https://status.anthropic.com) at briefing time. Excluded for lack of first-party confirmation.

---

## Sources

- [Claude Code v2.1.141 release (GitHub)](https://github.com/anthropics/claude-code/releases/tag/v2.1.141)
- [Claude Code CHANGELOG.md](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Claude Code What's New](https://code.claude.com/docs/en/whats-new)
- [Anthropic News](https://www.anthropic.com/news)
- [Claude for Small Business — anthropic.com/news](https://www.anthropic.com/news/claude-for-small-business)
- [Anthropic Research](https://www.anthropic.com/research)
- [Alignment Science Blog](https://alignment.anthropic.com)
- [Anthropic Engineering](https://www.anthropic.com/engineering)
- [Claude Platform Release Notes](https://platform.claude.com/docs/en/release-notes/overview)
- [Claude Help Center — Release Notes](https://support.claude.com/en/articles/12138966-release-notes)
- [HN: Claude for Small Business](https://news.ycombinator.com/item?id=48130950)
- [HN: Claude Code weekly limits +50% till July 13](https://news.ycombinator.com/item?id=48126429)
- [HN: front-page submissions from anthropic.com](https://news.ycombinator.com/from?site=anthropic.com)
- [TechCrunch — Cat Wu interview](https://techcrunch.com/2026/05/13/anthropics-cat-wu-says-that-in-the-future-ai-will-anticipate-your-needs-before-you-know-what-they-are/)
- [Axios — Anthropic secondary share sales alarm](https://www.axios.com/2026/05/13/anthropic-alarm-secondary-shares)
- [@ClaudeDevs on X — weekly-limits announcement](https://twitter.com/ClaudeDevs/status/2054639777685934564)
