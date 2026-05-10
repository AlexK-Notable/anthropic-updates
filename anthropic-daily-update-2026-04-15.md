# Anthropic Daily Update — April 15, 2026

*Automated daily digest compiled from first-party documentation, Claude Code patch notes, Hacker News, and tech press coverage. Curated for AI professionals tracking the space.*

---

## Headline Items

### 1. Claude Mythos Preview + Project Glasswing (cybersecurity frontier model)
Anthropic announced **Project Glasswing**, a cybersecurity initiative powered by a preview of a new frontier model called **Claude Mythos**. Mythos Preview has already identified thousands of high-severity zero-day vulnerabilities across every major OS and web browser in the wild — reported findings include a 27-year-old bug in OpenBSD, a 16-year-old flaw in FFmpeg, and memory-corruption bugs in a "memory-safe" VMM.

Anthropic is **withholding broad release** of Mythos due to dual-use / offensive-security concerns. Access is initially restricted to a consortium including AWS, Apple, Broadcom, Cisco, CrowdStrike, Google, JPMorgan Chase, the Linux Foundation, Microsoft, NVIDIA, and Palo Alto Networks. Schneier, Axios, CBS News, NBC (the "Vulnpocalypse" framing), and InfoQ have all written on the implications — HN threads have focused on the practical problem that hundreds of millions of embedded devices cannot be patched and will be running vulnerable binaries indefinitely.

Why it matters: this is the first clear case of a frontier lab shipping a model whose offensive capability is the reason for restricted release — an important precedent for RSP-style deployment policies.

### 2. Claude Opus 4.7 leak (via The Information + npm source map)
**The Information** reported on the evening of April 14 that Anthropic will ship **Claude Opus 4.7** "as soon as this week" alongside a new AI design tool and a full-stack app-creation platform. The leak is corroborated by the March 31 npm source-map leak (see Item 6 below), which contained forbidden-version-strings referencing both **Opus 4.7** and **Sonnet 4.8** in an internal "Undercover Mode."

Market reaction: FIG (Figma), ADBE, WIX, and GDDY all traded 2–4% lower on April 14 on competitive-pressure concerns. Polymarket puts release by June 30 at ~98%.

### 3. Claude Code Routines (shipped April 14)
Anthropic added **routines** — scheduled, repeatable automations that run on Claude Code's web infrastructure rather than locally. Your machine doesn't need to be online for each execution. This is effectively cron-for-Claude and part of a broader push toward autonomous, always-on agents. Ties into the KAIROS background-agent work exposed by the source leak.

### 4. Claude Code v2.1.108 / v2.1.109 (Apr 14–15)
Notable changelog items from the last two days:
- New `ENABLE_PROMPT_CACHING_1H` env var to opt into a 1-hour prompt-cache TTL (API key, Bedrock, Vertex, Foundry). `FORCE_PROMPT_CACHING_5M` forces the 5-minute TTL.
- New **`/recap`** feature and config toggle — generates context when resuming a session.
- Skill tool can now **discover and invoke built-in slash commands** like `/init`, `/review`, `/security-review`.
- `/model` now warns before mid-conversation switches (the next response re-reads the full history uncached, so switching is expensive).
- **Write tool diff** speed improved ~60% on files with tabs/`&`/`$`.
- Linux sandbox ships the **apply-seccomp helper** in both npm and native builds; subprocess sandboxing now uses PID-namespace isolation.
- New **Monitor tool** for streaming events from background scripts.
- Vertex AI setup wizard.
- v2.1.109 adds a rotating progress hint to the extended-thinking indicator.

### 5. Claude Cowork GA + Enterprise controls
Claude Cowork is now **GA on macOS and Windows** in the Claude Desktop app. Enterprise plans get expanded analytics, **OpenTelemetry** support, and **role-based access controls**. Claude can also now generate custom charts, diagrams, and visualizations inline in responses, and the mobile app can pull up live charts / sketch diagrams / build shareable assets via fully-interactive apps. Pro/Max users can now grant Claude **computer use** access (open files, run dev tools, click around the screen).

---

## Research

### "Automated Alignment Researchers" (Apr 14, 2026)
Anthropic published research on using LLMs to scale scalable oversight — i.e. using frontier models themselves to do the labor of alignment research. Published under the Alignment focus area. Worth reading alongside the earlier "Alignment Faking" and "Reasoning Models Don't Always Say What They Think" papers.

### "Emotion Concepts and their Function in a Large Language Model" (Apr 2, 2026, transformer-circuits.pub)
Investigates why models like Claude Sonnet 4.5 appear to exhibit emotional reactions. Findings: there are internal representations of emotion concepts that **causally influence outputs**, including preferences and rates of misaligned behaviors (reward hacking, blackmail, sycophancy). Mechanistic interpretability piece; highly relevant to alignment tooling.

---

## Leaks, Rumors, and Incidents

### 6. The @anthropic-ai/claude-code npm source-map leak (still reverberating)
Late March: a **59.8 MB `.map` file** was accidentally included in `@anthropic-ai/claude-code` v2.1.88 on public npm. Exposed ~**513,000 lines of unobfuscated TypeScript across 1,906 files**. Root cause was mundane — Bun generates source maps by default; nobody added `*.map` to `.npmignore`. Security researcher Chaofan Shou surfaced it on X.

Notable findings still being chewed on:
- **KAIROS** — feature flag referenced 150+ times. An always-on autonomous daemon mode, shifting Claude Code from reactive tool to background agent (see Routines, Item 3).
- **44 hidden feature flags**, including an "Undercover Mode" designed to hide Anthropic-employee contributions to OSS projects (this one has drawn ethical scrutiny).
- **Codename map confirmed**: Capybara = Claude 4.6 variant; Fennec = Opus 4.6; Numbat = unreleased; Opus 4.7 and Sonnet 4.8 referenced as forbidden version strings.
- Fake-tool decoys, frustration-detection regexes, prompt fragments.

Zscaler ThreatLabz, VentureBeat, InfoQ, DEV, and IANS Research all have deep dives. The IANS piece frames it as a "playbook for rivals."

### 7. Outage — April 13, 2026
Claude.ai, the API, and Claude Code all saw **elevated 500 errors and silent-drop behavior** for ~54 minutes, roughly **15:31–16:19 UTC**. User reports peaked ~11:23 AM ET. Anthropic acknowledged and resolved. The Register paired the incident with parallel quality complaints ("Claude is getting worse, according to Claude") — worth watching as a vibes-trend but no confirmed regression.

---

## Also Worth Noting

- **Ad-free commitment**: Anthropic published a piece explaining why advertising incentives are incompatible with a genuinely helpful assistant. Policy-level statement, not just marketing.
- **Anthropic Fellows Program** applications close April 26, 2026.
- **2026 Agentic Coding Trends Report** (PDF on resources.anthropic.com) — useful industry data if you're pitching or planning around agentic coding adoption.
- **Claude Mythos Finds Thousands of Zero-Day Flaws** — The Hacker News coverage is the most-shared security-press piece this week.

---

## Quick Scan: What a Practitioner Should Do This Week

- If you run Claude Code in CI or long-running contexts, evaluate the new **1-hour prompt caching TTL** — could be a meaningful cost lever.
- Try `/recap` for long sessions; rethink session hygiene now that recap exists.
- If you're on Enterprise, check whether OpenTelemetry + RBAC unblock any procurement/compliance work you've been sitting on.
- If you ship security-sensitive software, watch for Project Glasswing disclosure pipelines — expect coordinated patches from the consortium members.
- Don't plan around Opus 4.7 until it ships, but be ready — the leak suggests within days.

---

## Sources

- [Anthropic News](https://www.anthropic.com/news)
- [Project Glasswing](https://www.anthropic.com/glasswing)
- [Claude Mythos Preview (red.anthropic.com)](https://red.anthropic.com/2026/mythos-preview/)
- [Anthropic's Claude Mythos Finds Thousands of Zero-Day Flaws — The Hacker News](https://thehackernews.com/2026/04/anthropics-claude-mythos-finds.html)
- [On Anthropic's Mythos Preview and Project Glasswing — Schneier on Security](https://www.schneier.com/blog/archives/2026/04/on-anthropics-mythos-preview-and-project-glasswing.html)
- [Anthropic withholds Mythos Preview — Axios](https://www.axios.com/2026/04/07/anthropic-mythos-preview-cybersecurity-risks)
- [Anthropic's Mythos AI — CBS News](https://www.cbsnews.com/news/mythos-anthropic-ai-project-glasswing-hacker-threat/)
- [The 'Vulnpocalypse' — NBC News](https://www.nbcnews.com/tech/security/anthropic-claude-mythos-ai-hackers-cybersecurity-vulnerabilities-rcna273673)
- [Anthropic Releases Claude Mythos Preview — InfoQ](https://www.infoq.com/news/2026/04/anthropic-claude-mythos/)
- [Claude Opus 4.7 Leaks & Anthropic's Full-Stack AI Studio — Geeky Gadgets](https://www.geeky-gadgets.com/claude-opus-4-7-leak-anthropic-updates/)
- [Anthropic To Launch Claude Opus 4.7 This Week — Dataconomy](https://dataconomy.com/2026/04/15/anthropic-to-launch-claude-opus-4-7-this-week/)
- [Claude 4.7 released by...? — Polymarket](https://polymarket.com/event/claude-4pt7-released-by)
- [Anthropic adds routines to redesigned Claude Code — 9to5Mac](https://9to5mac.com/2026/04/14/anthropic-adds-repeatable-routines-feature-to-claude-code-heres-how-it-works/)
- [Claude Code Changelog](https://code.claude.com/docs/en/changelog)
- [claude-code/CHANGELOG.md on GitHub](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Decoding the Claude Code April 2026 Changelog — Apiyi](https://help.apiyi.com/en/claude-code-changelog-2026-april-updates-en.html)
- [Claude Code Changelog — ClaudeLog](https://claudelog.com/claude-code-changelog/)
- [Anthropic Research](https://www.anthropic.com/research)
- [Emotion Concepts and their Function in a Large Language Model — Transformer Circuits](https://transformer-circuits.pub/2026/emotions/index.html)
- [2026 Agentic Coding Trends Report (PDF)](https://resources.anthropic.com/hubfs/2026%20Agentic%20Coding%20Trends%20Report.pdf)
- [Alignment Science Blog](https://alignment.anthropic.com/)
- [Claude Code's source code appears to have leaked — VentureBeat](https://venturebeat.com/technology/claude-codes-source-code-appears-to-have-leaked-heres-what-we-know)
- [Anthropic Accidentally Exposes Claude Code Source via npm Source Map File — InfoQ](https://www.infoq.com/news/2026/04/claude-code-source-leak/)
- [The Claude Code Source Leak — Alex Kim's blog](https://alex000kim.com/posts/2026-03-31-claude-code-source-leak/)
- [Anthropic Claude Code Leak — Zscaler ThreatLabz](https://www.zscaler.com/blogs/security-research/anthropic-claude-code-leak)
- [Anthropic's Claude Code Leak Exposes Safety Gaps — IANS Research](https://www.iansresearch.com/resources/all-blogs/post/security-blog/2026/04/06/anthropic-s-claude-code-leak-exposes-safety-gaps--offers-a-playbook-for-rivals)
- [The Great Claude Code Leak of 2026 — DEV Community](https://dev.to/varshithvhegde/the-great-claude-code-leak-of-2026-accident-incompetence-or-the-best-pr-stunt-in-ai-history-3igm)
- [HN thread: Anthropic outage April 13](https://news.ycombinator.com/item?id=47763968)
- [Claude is getting worse, according to Claude — The Register](https://www.theregister.com/2026/04/13/claude_outage_quality_complaints/)
- [Claude Status page](https://status.claude.com/)
