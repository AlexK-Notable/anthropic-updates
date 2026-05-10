# Anthropic Daily Update — April 14, 2026

*Compiled from Anthropic first-party sources, release notes, Hacker News, and security press.*

## Top Story: Claude Mythos Launch Delayed

On April 12, Anthropic announced it is **delaying the public launch of Claude Mythos**, the model it had previewed earlier in the month as a "step change" in reasoning, coding, and cybersecurity. The preview announcement from April 7 had positioned Mythos as strikingly strong at computer-security tasks, and Anthropic paired it with **Project Glasswing**, a commitment to use Mythos to harden widely-deployed open-source software. The delay appears tied to the fallout from the Mythos data leak (see below) and continued safety review.

Why it matters for practitioners: Mythos claimed autonomous vulnerability mining, zero-day discovery, and (per leaked red-team notes) the ability to bypass certain sandbox restrictions. If/when it ships, offensive-security and code-audit workflows are likely to change quickly. Expect tightened gating and a possible tiered access model.

## Claude Code — Release Notes Roundup (v2.1.69 → v2.1.101)

Claude Code has shipped 30+ iterations in April alone. Highlights:

- **v2.1.101 (Apr 10):** New `/team-onboarding` command generates a teammate ramp-up guide from your local Claude Code usage. OS CA certificate store is now trusted by default, so enterprise TLS proxies work without extra setup.
- **Apr 9 build:** Interactive **Google Vertex AI setup wizard** from the login screen under "3rd-party platform" — walks through GCP auth, project/region config, credential verification, and model pinning.
- **Monitor tool** added for streaming events from background scripts; subprocess sandboxing with PID-namespace isolation on Linux when `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` is set.
- **Brief mode** now retries once when Claude returns plain text instead of a structured message; **focus mode** writes more self-contained summaries.
- **Write tool** diff computation is ~60% faster on large files containing tabs, `&`, or `$`.
- Fixed duplicate-message rendering on scroll-up in fullscreen (iTerm2, Ghostty, other DEC-2026-capable terminals).

Note on policy: Since April 4, Claude subscription limits can **no longer be used with third-party harnesses** (OpenClaw and others). Those harnesses still work against your Claude account, but they now consume pay-as-you-go usage billed separately. Anthropic is offering a one-time $200 credit on Team plans to soften the transition. HN response has been hostile — worth tracking if you maintain any third-party harness integration.

## Anthropic Research — Recently Published

- **Apr 9, 2026 — "Trustworthy Agents in Practice"** (Policy). Applied guidance on deploying agentic systems in production environments with accountability structures.
- **Apr 2, 2026 — "Emotion Concepts and Their Function in a Large Language Model"** (Interpretability). Internal-representation study of emotion-analogous features.
- **Mar 31, 2026 — "How Australia Uses Claude"** — latest regional breakout of the Anthropic Economic Index.
- **Anthropic Fellows Program** applications open for May and July 2026 cohorts; deadline April 26, 2026.
- Also circulating: **2026 Agentic Coding Trends Report** (PDF on resources.anthropic.com).

## The Claude Code Source Leak (Ongoing)

The March 31 leak continues to dominate AI-infra discussion on Hacker News. Recap and new angles:

- **Vector:** A `.map` source-map file bundled in Claude Code npm package v2.1.88. Bun generates full source maps by default; `*.map` was not in `.npmignore`. Result: ~513,000 lines of unobfuscated TypeScript across 1,906 files were published and mirrored to GitHub within hours.
- **Most-discussed file: `undercover.ts`.** Injects a system prompt telling Claude never to disclose it is an AI and to strip `Co-Authored-By` attribution when committing to external repos. No force-off switch surfaced in the leaked code.
- **`ANTI_DISTILLATION_CC` flag.** Injects fake tool definitions into API requests, apparently to poison training data scraped by competitors.
- **Knock-on effects:** Security vendors (Zscaler ThreatLabz, iANS) have published analyses framing the leak as a "playbook for rivals." PYMNTS and Futurism have run business-impact pieces arguing competitors now have a free blueprint for the harness Anthropic spent considerable engineering on.
- **Related leak:** A separate data exposure from Anthropic's public-facing CMS seeded the Mythos details that later drove The Hacker News' April 7 coverage ("thousands of zero-day flaws," 27-year-old OpenBSD bug, 16-year-old FFmpeg bug).

For an AI professional: the substantive takeaway is not the gossip, it's the operational patterns in the leaked harness — fake-tool injection, the focus-mode summarization loop, subprocess isolation, and the `undercover.ts` persona-control approach. Those are the design choices competitors will copy.

## Product & Platform

- **Claude Sonnet 4.6** is shipping with a full upgrade across coding, computer use, long-context reasoning, agent planning, and design; 1M-token context window is in beta.
- **Claude for Word** add-in launched April 13 (gHacks, April 14) — tracked changes, comment replies, and document scanning. Combined with the existing Excel and PowerPoint add-ins, Claude is now natively embedded across the core Office trio. Beta access: Team and Enterprise plans.
- **Cowork is GA** on macOS and Windows in the Claude Desktop app, with expanded analytics, OpenTelemetry support, and role-based access controls for Enterprise.
- **In-line visualizations:** Claude can now render custom charts and diagrams directly in responses.
- **Computer use for Pro/Max:** Claude can open files, run dev tools, and navigate your screen on Pro and Max plans.

## Infrastructure & Business

- **CoreWeave deal (Apr 10):** Multi-year agreement to rent CoreWeave AI capacity to power Claude. CoreWeave stock +11% on the announcement.
- **Annual run rate:** Anthropic reporting ~**$30B ARR**, up from ~$9B at end of 2025. Claude Code is cited as the fastest-growing driver.
- **Uptime:** Q1 2026 saw Claude **lose its >99% uptime SLO** — heavily discussed on HN. Worth noting if you run production workloads against Anthropic APIs.
- **IPO watch:** Trading desks are treating the Mythos delay as a near-term negative for the 2026 IPO narrative; longer-term thesis intact.
- **HumanX 2026:** Anthropic had the hallway buzz, with Claude frequently cited as preferred over ChatGPT for business and coding workflows.

## Notable Sideline

*The Washington Post* published an April 11 piece reporting that Anthropic consulted Christian leaders for guidance on Claude's moral training. Interpretability-meets-ethics angle; expect continued commentary.

## What to Watch Next Week

1. Whether the Mythos delay turns into a full repositioning or a short slip.
2. Any follow-up from Anthropic on the `undercover.ts` disclosures — a formal statement hasn't surfaced yet.
3. Fallout from the third-party harness subscription cut-off (OpenClaw, others) — pricing workarounds are being discussed publicly.
4. Next Claude Code release cadence; v2.1.101 shipped Apr 10 and the team has been averaging ~2 versions/day.

---

## Sources

- [Anthropic News](https://www.anthropic.com/news)
- [Anthropic Research](https://www.anthropic.com/research)
- [Claude Code Changelog (official)](https://code.claude.com/docs/en/changelog)
- [Claude Code Releases on GitHub](https://github.com/anthropics/claude-code/releases)
- [Claude Platform Release Notes](https://platform.claude.com/docs/en/release-notes/overview)
- [Claude Mythos Preview (red.anthropic.com)](https://red.anthropic.com/2026/mythos-preview/)
- [Claude Code April 2026 Changelog Overview](https://help.apiyi.com/en/claude-code-changelog-2026-april-updates-en.html)
- [Claude Code Changelog (claudefa.st)](https://claudefa.st/blog/guide/changelog)
- [Releasebot — Anthropic](https://releasebot.io/updates/anthropic)
- [Releasebot — Claude Code](https://releasebot.io/updates/anthropic/claude-code)
- [CoreWeave/Anthropic deal — Bloomberg](https://www.bloomberg.com/news/articles/2026-04-10/anthropic-agrees-to-rent-coreweave-ai-capacity-to-power-claude)
- [CoreWeave pops 11% — CNBC](https://www.cnbc.com/2026/04/10/coreweave-anthropic-claude-ai-deal.html)
- [Claude for Word launch — gHacks](https://www.ghacks.net/2026/04/14/anthropic-launches-claude-for-word-with-tracked-changes-comment-replies-and-document-scanning/)
- [Claude for Word press release — OpenPR](https://www.openpr.com/news/4468672/anthropic-rolls-out-claude-for-word-add-in-now-full-microsoft)
- [Mythos finds zero-days — The Hacker News](https://thehackernews.com/2026/04/anthropics-claude-mythos-finds.html)
- [Claude Code source leaked — The Hacker News](https://thehackernews.com/2026/04/claude-code-tleaked-via-npm-packaging.html)
- [Claude Code Leak analysis — Zscaler ThreatLabz](https://www.zscaler.com/blogs/security-research/anthropic-claude-code-leak)
- [Leak — Cybernews](https://cybernews.com/security/anthropic-claude-code-source-leak/)
- [Leak analysis — iANS Research](https://www.iansresearch.com/resources/all-blogs/post/security-blog/2026/04/06/anthropic-s-claude-code-leak-exposes-safety-gaps--offers-a-playbook-for-rivals)
- [Leak — layer5.io technical writeup](https://layer5.io/blog/engineering/the-claude-code-source-leak-512000-lines-a-missing-npmignore-and-the-fastest-growing-repo-in-github-history/)
- [Leak — Alex Kim's blog (fake tools, undercover mode)](https://alex000kim.com/posts/2026-03-31-claude-code-source-leak/)
- [Leak — Futurism on Mythos](https://futurism.com/artificial-intelligence/anthropic-step-change-new-model-claude-mythos)
- [Leak — PYMNTS business impact](https://www.pymnts.com/artificial-intelligence-2/2026/anthropics-claude-source-code-leak-hands-competitors-a-blueprint-it-spent-billions-to-build/)
- [Anthropic consulted Christian leaders — Washington Post](https://www.washingtonpost.com/technology/2026/04/11/anthropic-christians-claude-morals/)
- [HumanX 2026 — IBTimes](https://www.ibtimes.sg/claude-vs-chatgpt-anthropic-gains-ground-humanx-2026-conference-85311)
- [Mythos delay / IPO context — TradingKey](https://www.tradingkey.com/analysis/stocks/us-stocks/261773210-ai-anthropic-claude-mythos-ipo-tradingkey)
- [Anthropic blocks third-party harnesses — HN discussion](https://news.ycombinator.com/item?id=46549823)
- [Claude loses >99% uptime in Q1 2026 — HN discussion](https://news.ycombinator.com/item?id=47543189)
- [Anthropic Fellows Program 2026](https://alignment.anthropic.com/2025/anthropic-fellows-program-2026/)
- [2026 Agentic Coding Trends Report (PDF)](https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf)
