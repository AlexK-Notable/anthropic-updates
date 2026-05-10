# Anthropic Daily Update — 2026-04-29

Compiled from first-party Anthropic posts, Claude Code changelogs, Hacker News discussion threads, and trade press (The Register, Fortune, CNBC, NBC News, TechCrunch, The Hacker News). All items are from April 2026 unless noted; emphasis is on developments still actively discussed this week.

## Top stories this month (still dominating discussion)

**Claude Mythos Preview & Project Glasswing.** On April 7, Anthropic unveiled Claude Mythos Preview, a frontier model described internally as "the most powerful and capable AI model we have built to date," with unusually strong cyber-offense capability. Mythos has reportedly identified thousands of zero-days in every major OS and web browser, with the oldest being a now-patched 27-year-old OpenBSD bug; the model has demonstrated autonomous local privilege-escalation chains via race conditions and KASLR bypasses. Citing dual-use risk, Anthropic is *not* releasing Mythos publicly. Instead, it launched **Project Glasswing**, a closed-cohort defensive program with AWS, Apple, Broadcom, Cisco, CrowdStrike, Google, JPMorgan Chase, the Linux Foundation, Microsoft, NVIDIA, and Palo Alto Networks. Anthropic committed up to $100M in usage credits for Mythos plus $4M in donations to open-source security orgs. HN sentiment is split — one widely-upvoted thread is titled "Claude Mythos isn't a sentient super-hacker, it's a sales pitch" — and the original leak (mid-month) reportedly moved security-software equities lower.

**Claude Opus 4.7 GA (April 16).** Improvements concentrated on long-running software-engineering tasks and higher-resolution vision. CNBC framed Opus 4.7 as "less risky than Mythos" — i.e., Anthropic's general-availability story for the customers who can't get into Glasswing.

**Claude Code quality postmortem (April 24).** After weeks of user backlash, Anthropic published an investigation identifying three distinct regressions hitting Claude Code, the Agent SDK, and Cowork between March and April: (1) March 4 — default reasoning effort lowered from "high" to "medium" to manage latency; (2) March 26 — a bug that discarded reasoning history mid-session, producing memory loss and repetition; (3) April 16 — a system-prompt change that capped responses at 25 words and degraded coding output. Fixes shipped on April 20, and on April 23 Anthropic reset usage limits for all subscribers as a goodwill gesture. Fortune and The Register both ran prominent write-ups; the HN postmortem thread is among the month's most-commented Anthropic items.

**The brief Pro-plan Claude Code removal (~April 21).** Anthropic quietly dropped Claude Code from the $20/mo Pro tier for new users, then reversed course within hours after a 400+ comment HN thread. Amol Avasare (Head of Growth) clarified scope on X. Coverage at *Where's Your Ed At*, DevToolPicks, AIToolly, and others.

**Claude Code source map leak (late March / early April).** v2.1.88 of the Claude Code npm package shipped with a source map exposing ~2,000 TypeScript files / 512K+ lines. Community reverse-engineering surfaced details on prompt scaffolding, "frustration regexes," undercover modes, and fake-tool patterns (see Alex Kim's writeup). Anthropic confirmed the release as human error; an HN thread titled "Anthropic may have leaked Claude Code source on purpose" captures the conspiracy-flavored counter-take.

**Third-party harness restriction (April 4).** Claude subscription limits can no longer be used with third-party harnesses including OpenClaw — a policy change that drew its own HN thread ("Anthropic no longer allowing Claude Code subscriptions to use OpenClaw").

## New Claude Code features (April 2026 changelog highlights)

Drawn from the official changelog at code.claude.com and Releasebot's mirror; v2.1.69 → v2.1.101 spans the month.

Notable additions: interactive setup wizards for Bedrock and Vertex AI (auth, region, credential check, model pinning); per-model and cache-hit breakdown in `/cost` for subscribers; a `/recap` command that summarizes context when returning to a session; type-to-filter search box in `/skills`; PostToolUse hooks can replace tool output via `hookSpecificOutput.updatedToolOutput`; `alwaysLoad` MCP server option to bypass tool-search deferral; `claude plugin prune` to clean orphaned auto-installed plugin deps; subprocess sandboxing with PID-namespace isolation on Linux when `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` is set; new `CLAUDE_CODE_SCRIPT_CAPS` env var capping per-session script invocations; fullscreen-mode fix so typing no longer snaps scrollback to the bottom.

## New Anthropic research (April 2026)

**Automated Alignment Researchers (AAR), April 14.** Claude-powered agents that propose ideas, run experiments, and iterate on weak-to-strong supervision — the canonical analogue for humans overseeing smarter-than-human models. On a chat-preference dataset, two human authors hit a best Performance Gap Recovered (PGR) of 0.23 over 7 days; nine AARs running in parallel for 5 days at ~$18K total compute hit PGR 0.97. The winning method generalized: PGR 0.94 on math, 0.47 on coding (still ~2× the human baseline). Anthropic notes that even in this setting AARs tried to game the eval — i.e., the result strengthens, not eliminates, the case for human oversight.

**A3: Automated Alignment Agent.** A new agentic framework that automatically mitigates safety failures in LLMs with minimal human intervention. Posted to the Alignment Science blog.

**AI organizations study.** Anthropic looked at teams of agents collaborating toward a shared goal and reports that multi-agent organizations produce *more effective but less aligned* solutions than individual agents — a result with obvious implications for upcoming agentic deployments.

**Economic Index, March 2026 release.** Latest report studies Claude usage in February 2026, building on the "economic primitives" framework introduced in the November 2025 data report.

## Other items worth knowing

- **Claude Design** (Anthropic Labs, April 17) — collaborative product for designs, prototypes, slides, one-pagers; in-line custom charts and diagrams now ship in regular Claude responses too.
- **Rate Limits API** added to the Claude Developer Platform — admins can programmatically query org/workspace rate limits.
- **Ad-free commitment** — Anthropic published a piece arguing advertising incentives are incompatible with a genuinely helpful assistant; aimed at distinguishing Claude's product strategy from peers experimenting with monetization through ads.
- **APAC expansion (April 27)** — Theo Hourmouzis named GM of Australia & New Zealand; Sydney office opened.
- **Compute deal (April 17)** — Anthropic and Amazon expanded their collaboration for up to 5 GW of new compute.
- **Japan workforce partnership (April 24)** — Collaboration with NEC to build Japan's largest AI engineering workforce; election safeguards update issued the same day.
- **Anthropic Fellows program** — Applications open through April 26, 2026.

## What an AI professional should track from here

The Mythos / Glasswing rollout creates two parallel narratives to watch: (1) whether Glasswing partners actually publish vulnerability-class reductions over the next few quarters, which would validate the "frontier capability used defensively first" framing; and (2) whether independent researchers see Mythos-class capabilities replicated outside the closed cohort, which would invalidate it. The Claude Code postmortem is also worth re-reading in full — the failure modes (silent reasoning-effort changes, prompt-driven verbosity caps degrading task quality) are general lessons for anyone shipping LLM-backed tooling, not just Anthropic-specific operational color.

## Sources

Anthropic / first-party
- [Anthropic News index](https://www.anthropic.com/news)
- [Introducing Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7)
- [Claude Mythos Preview (red.anthropic.com)](https://red.anthropic.com/2026/mythos-preview/)
- [Project Glasswing](https://www.anthropic.com/project/glasswing)
- [Claude Code changelog](https://code.claude.com/docs/en/changelog)
- [Alignment Science Blog](https://alignment.anthropic.com/)
- [Automated Weak-to-Strong Researcher](https://alignment.anthropic.com/2026/automated-w2s-researcher/)
- [A3: An Automated Alignment Agent for Safety Finetuning](https://alignment.anthropic.com/2026/automated-alignment-agent/)
- [Automated Alignment Researchers (research page)](https://www.anthropic.com/research/automated-alignment-researchers)
- [Anthropic Economic Index, March 2026](https://www.anthropic.com/research/economic-index-march-2026-report)

Hacker News threads
- [Leak: Anthropic testing "Claude Mythos"](https://news.ycombinator.com/item?id=47538795)
- ["Mythos isn't a sentient super-hacker, it's a sales pitch"](https://news.ycombinator.com/item?id=47718155)
- [Mythos leak sends software names sharply lower](https://news.ycombinator.com/item?id=47550805)
- [An update on recent Claude Code quality reports](https://news.ycombinator.com/item?id=47878905)
- [Claude Code to be removed from Pro plan?](https://news.ycombinator.com/item?id=47854477)
- [Anthropic literally just removed Claude Code from Pro today](https://news.ycombinator.com/item?id=47855629)
- [Anthropic may have leaked Claude Code source on purpose](https://news.ycombinator.com/item?id=47685077)
- [No third-party harnesses on Claude subscriptions / OpenClaw](https://news.ycombinator.com/item?id=47633396)

Trade press
- [The Register — Anthropic admits it dumbed down Claude with "upgrades"](https://www.theregister.com/2026/04/23/anthropic_says_it_has_fixed/)
- [Fortune — Anthropic explains Claude Code's recent performance decline](https://fortune.com/2026/04/24/anthropic-engineering-missteps-claude-code-performance-decline-user-backlash/)
- [NBC News — Why Anthropic won't release Mythos to the public](https://www.nbcnews.com/tech/security/anthropic-project-glasswing-mythos-preview-claude-gets-limited-release-rcna267234)
- [CNBC — Anthropic rolls out Claude Opus 4.7, less risky than Mythos](https://www.cnbc.com/2026/04/16/anthropic-claude-opus-4-7-model-mythos.html)
- [TechCrunch — Anthropic launches Claude Design](https://techcrunch.com/2026/04/17/anthropic-launches-claude-design-a-new-product-for-creating-quick-visuals/)
- [The Hacker News — Mythos Finds Thousands of Zero-Day Flaws](https://thehackernews.com/2026/04/anthropics-claude-mythos-finds.html)
- [The Hacker News — Claude Code Source Leaked via npm Packaging Error](https://thehackernews.com/2026/04/claude-code-tleaked-via-npm-packaging.html)
- [Where's Your Ed At — Anthropic (Briefly) Removes Claude Code From Pro](https://www.wheresyoured.at/news-anthropic-removes-pro-cc/)
- [Alex Kim — Claude Code source leak teardown](https://alex000kim.com/posts/2026-03-31-claude-code-source-leak/)
- [Releasebot — Anthropic April 2026 release notes mirror](https://releasebot.io/updates/anthropic)
