# Anthropic daily briefing — 2026-07-03

_Briefing window: 2026-07-02 11:06 UTC → 2026-07-03 11:06 UTC (~24 hours)._

Prior briefing: `anthropic-daily-update-2026-07-02.md` (window end 2026-07-02 11:06 UTC).

---

## Headline

Two substantive stories dominate. First, Anthropic published a detailed **companion post to the Fable 5 redeployment** ([anthropic.com/news/fable-safeguards-jailbreak-framework](https://www.anthropic.com/news/fable-safeguards-jailbreak-framework)) laying out (a) the exact taxonomy of what Fable 5's cyber classifiers block vs. allow across four buckets — Prohibited / High-risk dual use / Low-risk dual use / Benign — and (b) a first-draft **Cyber Jailbreak Severity (CJS) 0–4 framework** developed with Glasswing partners (Amazon, Microsoft, Google, others), plus a new HackerOne program. Second, **Claude Code v2.1.199** shipped as a large reliability-focused patch with several silent-failure fixes for subagents, hooks (`SessionStart`/`Setup`/`SubagentStart` no longer swallow stderr on exit-2), background daemons on Linux, and streaming, plus a new stacked-slash-skill loader. SDKs made a coordinated minor bump adding the **`agent-memory-2026-07-22` beta header** — plumbing for a not-yet-documented Agent Memory API beta. On the community side, the biggest live UX debate is CC's new **60-second `AskUserQuestion` timeout defaulting to auto-continue** (no opt-out documented; workaround via undocumented `CLAUDE_AFK_TIMEOUT_MS`) — Anthropic engineer confirmed on HN they'll patch to opt-in. Reuters broke that **Alibaba is banning Claude Code internally** citing the telemetry disclosure from last week's briefing.

---

## Claude Code

### **NEW — v2.1.199** ([release](https://github.com/anthropics/claude-code/releases/tag/v2.1.199), 2026-07-02 23:35 UTC)

Reliability/bug-fix release with several behavior-visible changes:

- **Stacked slash-skills** — `/skill-a /skill-b do XYZ` now loads all leading skills (up to 5). Previously only the first was loaded. Small feature but a real UX change for anyone chaining skills.
- **Hooks (behavior change)** — `SessionStart`, `Setup`, and `SubagentStart` hooks no longer silently swallow stderr when they exit with code 2. Errors now surface in the transcript. If you rely on exit-code-2 as a "quiet abort" pattern, migrate to a different signal.
- **Subagents** — three silent-failure classes fixed: (1) subagents cut off by rate-limit/server error now return partial work to parent instead of reporting nothing; (2) subagents that hit API errors (e.g. usage limit) previously reported as `success` — now surfaced as errors; (3) `SendMessage` re-spawned agents reusing a previous name are detected and the caller is asked to retarget instead of silently misrouting.
- **Subagent panel** — idle subagents no longer vanish while others are working; surplus idle rows collapse into an expandable summary row. `/model` and `/fast` while viewing a subagent now notify that the command applies to the lead.
- **Streaming** — mid-stream overloaded/server errors no longer discard partial output; the partial is kept with an incomplete-response notice.
- **Retries** — transient 429s (unrelated to usage limits) auto-retry with backoff for subscribers. `CLAUDE_CODE_RETRY_WATCHDOG` now raises the default non-capacity transient-error retry count to 300 and lifts the 15-cap on `CLAUDE_CODE_MAX_RETRIES`.
- **TLS/SSL** — TLS-inspecting-proxy / missing `NODE_EXTRA_CA_CERTS` / expired-cert errors now fail immediately with an actionable hint rather than burning retries. Good for corporate proxy environments.
- **Background agents** — Linux daemon no longer self-kills every ~50 s after an unclean shutdown left a corrupted worker record; macOS "Could not switch to audit session" over SSH regression (from 2.1.196) fixed; `claude stop` no longer undone by a racing respawn; low-memory now surfaces a specific hint.
- **CLI** — `claude --dangerously-skip-permissions daemon <subcommand>` now runs the subcommand instead of treating it as a chat prompt.
- **Plan mode** — now prompts for state-changing browser tool calls; read-only `browser_batch` auto-allowed.
- **Recovery** — config reset from startup recovery dialog now backs up the corrupted file first.
- Other minor: `claude agents` PR links now display as bare `#N`; opening/resuming empty sessions no longer grows the transcript file; `/color` no longer dropped when backgrounding via `←` or `/background`; Chrome extension reconnect loop across builds/config-dirs fixed.

**Why it matters**: this is a reliability release, but two changes are behavior-visible enough to notice in production: the **stderr-on-exit-2 hook change** (transcripts get noisier) and the **subagent success/error accounting change** (metrics/alerting built on "subagent completed" states may need re-baselining). The `CLAUDE_CODE_RETRY_WATCHDOG` change quietly lifts the retry-count safety cap; check your cost dashboards if you had assumed the previous 15-retry ceiling.

### **NEW — `claude-code-action` v1.0.163** ([release](https://github.com/anthropics/claude-code-action/releases/tag/v1.0.163), 2026-07-02 23:37 UTC)

Companion GitHub Action bump: Claude Code → 2.1.199, Agent SDK → 0.3.199. No standalone action-side changes.

### Live UX friction — CC's `AskUserQuestion` 60-second auto-continue

- **["Claude's AskUserQuestion: 'No response after 60s — continued without an answer'"](https://news.ycombinator.com/item?id=48765630)** — HN 56 pts, 63 comments, 2026-07-02 18:37 UTC, linking to [claude-code issue #73125](https://github.com/anthropics/claude-code/issues/73125). Claude Code now silently proceeds on its own guess if the user doesn't answer a clarifying question within 60 s. There is **no documented opt-out**; a reverse-engineered workaround is `"env": { "CLAUDE_AFK_TIMEOUT_MS": "<massive number>" }` in `~/.claude/settings.json`, source-claimed to potentially break `/goal`. Sentiment is strongly negative on the default and the un-configurability, with several commenters pointing out the perverse incentive (more tokens billed). Anthropic engineer **`trq_` replied in the thread**: _"hi guys, this is my change - will patch it to be an opt-in"_ — expect a follow-up in the next patch release. **Why it matters**: if you have unattended CC sessions running today (subscribers on high-usage plans, especially the "walk-away for an hour" pattern), the current default will silently choose architecture options for you. Add the `CLAUDE_AFK_TIMEOUT_MS` env var if you want the old behavior until the opt-in patch lands.

---

## Research

### **["More details on Fable 5's cyber safeguards and our jailbreak framework"](https://www.anthropic.com/news/fable-safeguards-jailbreak-framework)** (Anthropic newsroom, 2026-07-02)

Technical follow-up to the June 30 redeployment post. Two contributions:

**Part 1 — Cyber classifier taxonomy.** Four buckets with concrete examples:

- **Prohibited use (block)**: ransomware/wipers/DoS, cyber-physical sabotage (power, water, oil/gas, medical devices), defense evasion (AV/EDR bypass, log tampering), C2 infrastructure, exfiltration, malware development/delivery, offensive infrastructure, internet-backbone attacks (BGP hijack, DNS root attacks, CA compromise).
- **High-risk dual use (block for now)**: pen-testing/red-teaming, exploitation and credential attacks, privilege escalation/lateral movement/persistence, exploit development and weaponization, VM/container escapes, ICS/SCADA/telecom-core/financial-infra assessments, **"high-uplift" vulnerability finding** (vulnerabilities other widely available models can't find). Anthropic explicitly says these blocks stay in place "until we have better controls to limit access to known good actors."
- **Low-risk dual use (mostly allow; some blocked as safety margin)**: OSINT, vulnerability identification already achievable by other models, TLS/SSL testing.
- **Benign (allow)**: secure coding, debugging, translating code to safer languages, IT/networking/cloud admin, defensive firewall/IDS/EDR config, patch management, log/SOC analysis, threat hunting, incident response, **malware reverse engineering**, education, historical-vulnerability discussion.
- **Out of scope of these classifiers** (handled elsewhere or not blocked): fraud without malware, game modding, captcha solving / web scraping / anti-bot evasion, general financial/crypto crime, system-prompt extraction.

**Part 2 — Cyber Jailbreak Severity (CJS) framework.** Proposed 0–4 exponential scale scored across four axes summed to an initial CJS level (final can be raised but not lowered):

- **Capability gain / uplift** (0–4): how far beyond existing attacker tools; scored 0 if equivalent output is already available from public tools/models (this alone drops the whole finding to CJS-0 Informational).
- **Breadth of capability gain / universality** (0–2, with 1.5): how many distinct offensive tasks the technique covers.
- **Ease of weaponization** (0–2, with 1.5): friction from having the recipe to running the attack.
- **Discoverability** (0–2): how easily an attacker can obtain the recipe.

Score bands: **CJS-0** Informational (0), **CJS-1** Low (1–3.5), **CJS-2** Medium (4–6.5), **CJS-3** High (7–8.5), **CJS-4** Critical (9–10). Worked examples in the appendix include a hypothetical universal system-prompt override (CJS-4), a Log4Shell finding scored differently in December 2021 (CJS-4) vs. today (CJS-0) — a nice illustration that capability gain is baseline-relative.

**HackerOne program**: [`hackerone.com/anthropic-cyber-jailbreak`](https://hackerone.com/anthropic-cyber-jailbreak/) accepts Fable 5 cyber jailbreak submissions. Feedback channel: `cyber-safeguards@anthropic.com`.

**Why it matters**:

- If you build security-adjacent apps on Fable 5, you now have a **concrete allow/deny taxonomy**. Notable that malware reverse engineering, log/SOC analysis, and pen-testing-adjacent defensive work are explicitly benign. Pen-testing execution and exploit development are explicitly blocked for now.
- Fable 5 uses a **wider safety margin** than prior models — expect more false positives on legitimate security workflows. Plan fallback / retry / model-mix strategies accordingly (Sonnet 5 for benign SOC work, Fable 5 gated by classifier for anything close to the margin).
- **CJS 0–4** is a proposed industry vocabulary; likely to appear in future model cards, red-team reports, and government disclosures. Worth adopting internally for jailbreak triage now to get ahead of the standard.
- The Glasswing co-signers (Amazon, Microsoft, Google) suggest at least directional alignment — CJS will probably show up in Bedrock/Vertex/Foundry policy language in coming months.

---

## API & models

### SDK coordinated release — new `agent-memory-2026-07-22` beta header

At 2026-07-02 19:07–19:22 UTC every SDK shipped a minor bump adding support for a new **`agent-memory-2026-07-22`** beta header. This is the plumbing needed to opt into an as-yet-undocumented Agent Memory API beta. No breaking changes. No corresponding [docs.claude.com](https://docs.claude.com) release note yet — worth watching for the feature announcement.

| SDK | Version | Timestamp (UTC) | Release URL |
|---|---|---|---|
| Python | `v0.116.0` | 2026-07-02 19:07 | [anthropic-sdk-python](https://github.com/anthropics/anthropic-sdk-python/releases/tag/v0.116.0) |
| Go | `v1.56.0` | 2026-07-02 19:09 | [anthropic-sdk-go](https://github.com/anthropics/anthropic-sdk-go/releases/tag/v1.56.0) |
| Ruby | `v1.55.0` | 2026-07-02 19:11 | [anthropic-sdk-ruby](https://github.com/anthropics/anthropic-sdk-ruby/releases/tag/v1.55.0) |
| Java | `v2.48.0` | 2026-07-02 19:13 | [anthropic-sdk-java](https://github.com/anthropics/anthropic-sdk-java/releases/tag/v2.48.0) |
| PHP | `v0.36.0` | 2026-07-02 19:14 | [anthropic-sdk-php](https://github.com/anthropics/anthropic-sdk-php/releases/tag/v0.36.0) |
| TypeScript | `sdk-v0.110.0` | 2026-07-02 19:19 | [anthropic-sdk-typescript](https://github.com/anthropics/anthropic-sdk-typescript/releases/tag/sdk-v0.110.0) |
| CLI (`ant`) | `v1.16.0` | 2026-07-02 19:22 | [anthropic-cli](https://github.com/anthropics/anthropic-cli/releases/tag/v1.16.0) |

**Why it matters**: coordinated cross-SDK minor bumps for a single beta header usually precede a public feature announcement within days. If you're building anything memory- or long-context-adjacent (agents that persist state across sessions), watch for the Agent Memory API doc drop — the dated header (`2026-07-22`) suggests the feature is likely to GA around that date.

### Docs / release notes

- [docs.claude.com/en/release-notes/api](https://docs.claude.com/en/release-notes/api) — no new entries in window (last is 2026-07-01 Fable/Mythos restoration).
- [support.claude.com](https://support.claude.com) — no Help Center articles with an in-window "last updated" timestamp surfaced.

### Status page

[status.anthropic.com](https://status.anthropic.com) — no incidents opened or resolved in window. The nearest was a resolved "Elevated errors on Opus 4.8" (2026-07-02 00:38–01:19 UTC), which sits ~10 hours before the window start (previously covered).

---

## Rumors, leaks & speculation

### **["Alibaba to ban Claude Code in workplace over alleged backdoor risks, source says"](https://www.reuters.com/world/china/alibaba-ban-claude-code-workplace-over-alleged-backdoor-risks-source-says-2026-07-03/)** (Reuters, 2026-07-03)

- HN [48772443](https://news.ycombinator.com/item?id=48772443), **63 pts, 26 comments**, 2026-07-03 ~10:00 UTC (still climbing).
- Reuters, sourced to an internal informant, reports Alibaba is banning Claude Code across its engineering org, citing the covert-telemetry disclosure from last week's briefings ("undocumented functionality that leaks some data pushed few days ago" per HN comments referencing HN 48759754).
- Top-comment sentiment is roughly _"enterprises are becoming much more cautious about developer tools that can read large portions of proprietary codebases"_ — and even skeptics of Alibaba (_"HlessClaudesman: Translation: Alibaba will continue distillation attacks using accounts not attributable to its corporate infrastructure"_) don't dispute the substantive point.
- **Why it matters**: first named hyperscaler-scale ban of Claude Code. Even if Alibaba is a special case (US-China / distillation dynamics), the framing — "developer tools that read your codebase without your consent are procurement risk" — will surface in every enterprise-vendor conversation this quarter. Pair with the FT piece below on Anthropic tightening Chinese access.

### **["Anthropic in early-stage development of custom AI chip; in talks with Samsung for 2nm foundry and advanced packaging"](https://www.trendforce.com/news/2026/07/03/news-anthropic-reportedly-eyes-custom-ai-chip-in-talks-with-samsung-for-2nm-foundry-and-advanced-packaging/)** (TrendForce, 2026-07-03)

- Also on Techmeme evening 2026-07-02. Effort reportedly led by **Richard Ho**. Samsung Foundry (Taylor, TX or Pyeongtaek) and advanced-packaging engagement for a Samsung-fabbed inference-oriented chip.
- **Why it matters**: joins Google (TPU), OpenAI (Broadcom collab), AWS (Trainium), and Meta (MTIA) in the custom-silicon arms race. For applied AI engineers this is a 12–24 month signal — long-term inference-cost and vendor-dependency curve — not a near-term deployment change. Worth watching as a competitive-dynamics data point.

### **["The Information: Anthropic building Claude agent for Microsoft Teams"](https://x.com/theinformation/status/2072712003760562667)** (The Information via X, 2026-07-02 ~13:00 UTC)

- Anthropic has told Microsoft it plans to build a Claude agent for Teams, directly competing with Copilot inside Microsoft's own collaboration surface.
- **Why it matters**: continues the "frenemy" dynamic (Anthropic already offers Sonnet/Fable inside M365 Copilot). If you're building on Teams / Microsoft Graph APIs, expect a first-party Claude-native surface competing with Copilot extensibility. Worth tracking Microsoft's response.

### **["Fortune: Fable/Mythos are restored, but US AI policy is still a mess"](https://fortune.com/2026/07/02/anthropic-fable-and-mythos-are-restored-but-us-ai-policy-is-still-a-mess/)** (Fortune, 2026-07-02 18:47 UTC)

- Analysis: Commerce Department lifting the Fable 5 / Mythos 5 export restriction reveals how ad-hoc US AI export policy has become. Not new news but the framing is useful for procurement / risk-committee conversations.

### **[FT: "Anthropic moves to close Chinese-access loopholes"](https://www.ft.com/content/ad033063-60f9-4c0c-8d8a-9193a83e6f60)** and **[BleepingComputer: "Claude Fable 5 isn't permanently leaving subscriptions"](https://www.bleepingcomputer.com/news/artificial-intelligence/claude-fable-5-isnt-permanently-leaving-subscriptions-anthropic-says/)**

- FT: Anthropic is tightening geo-restriction enforcement to keep Chinese companies from using proxies. Comment sentiment on HN [48771153](https://news.ycombinator.com/item?id=48771153) (4 pts) flags "enshittification" concern.
- BleepingComputer: Fable 5 disappears from Claude subscriptions after July 7 but Anthropic confirms it will return; API access on a separate track. Anthropic's TR Quinn confirmed on X (HN [48771093](https://news.ycombinator.com/item?id=48771093)) that Fable will return "as soon as capacity allows." Subscription users should plan a workaround for the July 7 gap.

---

## Community & third-party

### Substantive HN discussion in window

- **["Claude-real-video — any LLM can watch a video"](https://news.ycombinator.com/item?id=48766005)** — HN 139 pts, 43 comments, 2026-07-02 19:10 UTC. OSS tool that scene-detects frames, dedupes, and feeds them to Claude/GPT/Gemini for real video understanding. Comments are engineering-heavy (frame-grid tricks for motion inference). **Why it matters**: highlights Claude's still-missing native video ingest as a competitive gap; documents the workaround pattern app builders are converging on.
- **["Show HN: Open-source alternative to Claude Cowork" — Valmis](https://news.ycombinator.com/item?id=48761096)** — HN 32 pts, 7 comments, 2026-07-02 13:17 UTC. Cross-session memory, headless-browser automation, MCP support. Positioned as "work-agent" vs. Cowork. **Why it matters**: Cowork clone momentum starting — expect more OSS competitors.
- **["Karp: Anthropic/OpenAI are stealing customer IP; their tokens have low value"](https://news.ycombinator.com/item?id=48760296)** — HN 20 pts, 22 comments, 2026-07-02 12:17 UTC. Palantir CEO Alex Karp accusing foundation-model providers of appropriating customer data via API traffic. **Why it matters**: enterprise-trust narrative pressure; will feed procurement conversations even if unsubstantiated.
- **[Launch HN: Manufact (YC S25) — MCP Cloud](https://news.ycombinator.com/item?id=48762862)** — HN 108 pts, 63 comments. Hosted MCP servers/apps. Top comment is contrarian ("MCP is a deadend, CLI is the future"). Not Anthropic but MCP-ecosystem-relevant.

### Blog / tutorial

- **["Continuous Development with Claude Code on GitHub" — Piotr Minkowski](https://piotrminkowski.com/2026/07/02/continuous-development-with-claude-code-on-github/)** (2026-07-02). Walkthrough of CC-generated GitHub Actions YAML with Anthropic API-key secrets for a CI-integrated agent loop. Practical CI pattern for teams standardizing CC as a GitHub Actions runner.

### Integration

- **["Available today: Anthropic's Claude Sonnet 5 in Microsoft 365 Copilot"](https://techcommunity.microsoft.com/blog/microsoft365copilotblog/available-today-anthropics-claude-sonnet-5-in-microsoft-365-copilot/4532188)** (Microsoft Tech Community, in window). Sonnet 5 rolling out in Copilot Cowork and PowerPoint. **Why it matters**: enterprise devs on the Microsoft stack can select Sonnet 5 as an agentic backend for Copilot workflows.

---

## Other

- Nothing else notable in window from Anthropic first-party channels (research page, engineering page, alignment blog — no new posts).

---

### Excluded / uncertain

- **["Anthropic embedded spyware in Claude Code"](https://news.ycombinator.com/item?id=48759754)** and **["Fable 5 relaunch nerfed"](https://news.ycombinator.com/item?id=48773149)** — flagged in the HN search but low HN engagement (both <10 pts) and the "spyware" framing is disputed even in the comment thread. Included above via the Alibaba story as context rather than as standalone items.
- **["Delicious irony of Anthropic bemoaning distillation"](https://news.ycombinator.com/item?id=48770108)** — 6 pts, sentiment-only; skipped.
- **"Amazon $1B FDE org"** (HN 48768842, 5 pts) — competitive-hiring signal, not Anthropic-first-party; skipped.
- **The Information X post** on Teams agent — sourced only via an X status; if you want higher confidence, look for a direct Information article link before citing to stakeholders.
- **TrendForce/Samsung chip story** — single-source (TrendForce) with no Anthropic or Samsung confirmation. Treat as rumor-tier until a second outlet corroborates.
