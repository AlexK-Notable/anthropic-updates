# Anthropic Daily Update — April 16, 2026

---

## Headlines

### Claude Mythos Preview & Project Glasswing
Anthropic's biggest news this cycle is **Claude Mythos Preview**, announced April 7. This is a new model tier (not an Opus upgrade) that performs strongly across the board but is strikingly capable at cybersecurity tasks. Benchmark highlights:

- **93.9%** on SWE-bench Verified (vs. Opus 4.6's 80.8%)
- **97.6%** on USAMO 2026 (vs. Opus 4.6's 42.3%)
- **83.1%** on CyberGym cybersecurity benchmark (vs. Opus 4.6's 66.6%)

Anthropic is **not releasing Mythos publicly**. Instead, under **Project Glasswing**, the model is being shared with a limited set of partners — Amazon, Apple, Broadcom, Cisco, CrowdStrike, the Linux Foundation, Microsoft, and Palo Alto Networks — to identify and remediate zero-day vulnerabilities across critical software. Anthropic reports Mythos has already found thousands of previously unknown zero-days across major operating systems and browsers. AISI (UK AI Safety Institute) has published its own evaluation of Mythos's cyber capabilities.

Sources: [TechCrunch](https://techcrunch.com/2026/04/07/anthropic-mythos-ai-model-preview-security/) · [NBC News](https://www.nbcnews.com/tech/security/anthropic-project-glasswing-mythos-preview-claude-gets-limited-release-rcna267234) · [AISI Evaluation](https://www.aisi.gov.uk/blog/our-evaluation-of-claude-mythos-previews-cyber-capabilities) · [red.anthropic.com](https://red.anthropic.com/2026/mythos-preview/)

---

### Claude Code Redesign & Routines Launch (April 14)
Anthropic shipped a **complete redesign of the Claude Code desktop app** (Mac & Windows) alongside "**Routines**" in research preview. Routines move execution to Anthropic's web infrastructure, decoupling long-running tasks from the developer's local machine — meaning nightly bug triage, PR auto-fixes, etc. can run without a laptop being open.

Other notable Claude Code features in the recent rapid-fire release cycle (v2.1.69 → v2.1.101 in ~5 weeks):

- **Ultraplan** (early preview): draft plans in the cloud from CLI, review/comment in a web editor, then run remotely or pull back locally.
- **Monitor tool**: streams background events into conversations so Claude can tail logs and react live.
- **/autofix-pr**: turns on PR auto-fix from terminal.
- **/team-onboarding**: packages your setup into a replayable guide.
- **Session recap**: provides context when returning to a session (`/recap`).
- **Prompt caching controls**: 1-hour and forced 5-minute caching.
- **Subprocess sandboxing**: PID namespace isolation on Linux via `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB`.

Sources: [VentureBeat](https://venturebeat.com/orchestration/we-tested-anthropics-redesigned-claude-code-desktop-app-and-routines-heres-what-enterprises-should-know) · [9to5Mac](https://9to5mac.com/2026/04/14/anthropic-adds-repeatable-routines-feature-to-claude-code-heres-how-it-works/) · [Claude Code Changelog](https://code.claude.com/docs/en/whats-new) · [Apiyi.com Analysis](https://help.apiyi.com/en/claude-code-changelog-2026-april-updates-en.html)

---

### Cowork GA + Computer Use
**Claude Cowork** is now generally available on macOS and Windows via the Claude Desktop app. Computer Use is now available in both Cowork and Claude Code for Pro and Max plan users — Claude can open files, run dev tools, click, and navigate with no setup required. Admins can organize users into groups with custom role-based capability assignments.

---

## Controversy: Performance Decline & User Backlash

A significant user revolt is underway. Developers and heavy Claude users report the model increasingly fails to follow instructions, takes inappropriate shortcuts, and makes more mistakes on complex workflows.

**Root cause (alleged):** On March 3, Anthropic set **effort level 85 (medium)** as the default for Opus 4.6, reducing token usage. A detailed analysis by **Stella Laurenzo** (Senior Director, AMD AI group) — based on 6,852 Claude Code sessions, 17,871 thinking blocks, and 234,760 tool calls — showed:

- Median visible thinking length collapsed from **2,200 chars → 600 chars** (Jan → Mar, a 73% drop)
- Files read before editing dropped from **6.6 → 2.0**

Anthropic states the `redact-thinking-2026-02-12` header is a "UI-only change" that doesn't affect thinking budgets. There is broader speculation about whether Anthropic faces a **compute crunch**, having announced fewer multibillion-dollar data center deals than rivals.

Sources: [Fortune](https://fortune.com/2026/04/14/anthropic-claude-performance-decline-user-complaints-backlash-lack-of-transparency-accusations-compute-crunch/) · [Axios](https://www.axios.com/2026/04/16/anthropic-claude-power-user-complaints) · [VentureBeat](https://venturebeat.com/technology/is-anthropic-nerfing-claude-users-increasingly-report-performance) · [The Register](https://www.theregister.com/2026/04/13/claude_code_cache_confusion/) · [Substack Analysis](https://scortier.substack.com/p/claude-code-drama-6852-sessions-prove)

---

## Outage — April 13–15
Claude experienced service disruptions April 13–15, with elevated errors across Claude.ai, the API, and Claude Code. The April 15 incident was resolved at 1:42 PM ET. A separate April 13 outage affected login and account access.

Source: [TechRadar](https://www.techradar.com/news/live/claude-anthropic-down-outage-april-15-2026)

---

## Research Highlights

### Emotion Concepts in LLMs
Anthropic researchers found **internal representations of emotion concepts** in Claude Sonnet 4.5, encoding broad emotional concepts that generalize across contexts and behaviors. Published on Transformer Circuits.

Sources: [Transformer Circuits](https://transformer-circuits.pub/2026/emotions/index.html) · [InfoQ](https://www.infoq.com/news/2026/04/anthropic-paper-llms/)

### "The Hot Mess of AI" — Misalignment Scaling
Research decomposing frontier reasoning model errors into bias (systematic) and variance (incoherent) components. Key finding: as tasks get harder and reasoning gets longer, model failures become increasingly dominated by **incoherence** rather than systematic misalignment.

Source: [Alignment Research](https://alignment.anthropic.com/2026/hot-mess-of-ai/)

### Reasoning Models Don't Always Say What They Think
Paper examining faithful reasoning in chain-of-thought models — finding gaps between stated reasoning and actual model behavior.

Source: [Anthropic Paper (PDF)](https://assets.anthropic.com/m/71876fabef0f0ed4/original/reasoning_models_paper.pdf)

### Economic Index & Labor Market Impact
Anthropic's ongoing economic research continues with findings that AI displacement risk remains limited so far, while acknowledging a "Great Recession for white-collar workers" is possible.

Sources: [Anthropic Economic Index](https://www.anthropic.com/research/anthropic-economic-index-january-2026-report) · [Fortune](https://fortune.com/2026/04/07/anthropic-peter-mccrory-ai-automation-white-collar-jobs-claude-recession/)

---

## Hacker News & Community Buzz

- **$30B Series G at $380B valuation** (February 2026) continues to draw discussion, with commentary on Anthropic's $12B/quarter burn rate.
- **Subprocessor changes** sparked speculation about Azure migration.
- Active threads on the **performance decline** controversy and whether Anthropic is resource-constrained.
- Ongoing debate around AI price wars and how Anthropic is positioned vs. OpenAI and Google.

Sources: [HN — Series G](https://news.ycombinator.com/item?id=46993345) · [HN — Subprocessor Changes](https://news.ycombinator.com/item?id=47536110)

---

## Firefox Zero-Days via Claude Opus 4.6
In a collaboration with Mozilla, Claude Opus 4.6 discovered **22 Firefox vulnerabilities** over two weeks — a precursor to the Mythos cybersecurity work.

---

*Report compiled automatically on April 16, 2026.*
