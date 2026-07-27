# Anthropic daily briefing — 2026-07-23

_Briefing window: 2026-07-22 11:05 UTC → 2026-07-23 11:05 UTC (24 hours, no gap)._

Prior briefing: `anthropic-daily-update-2026-07-22.md` (window end 2026-07-22 11:05 UTC).

---

## Claude Code

### **Claude Code v2.1.218 released** — [GitHub releases](https://github.com/anthropics/claude-code/releases/tag/v2.1.218), Jul 22 ~21:24 UTC

- Headline behavioral change: `/code-review` now runs as a background subagent instead of filling your conversation, and stacked slash commands become its review target. `/deep-research` now only starts when manually invoked — Claude no longer launches it unprompted. Auto mode's dangerous-`rm`, background-`&`, and suspicious-Windows-path checks moved from permission dialogs to classifier adjudication.
- Notable fixes: Windows paths with `\u`-prefixed segments (e.g. `C:\Users\unicorn`) were being corrupted into CJK characters, making files inaccessible; the left-arrow key could discard a conversation with no undo; `/ultrareview` failed on descriptive arguments like "review my auth changes"; agent frontmatter hooks could run from untrusted folders without the agent file's own folder having accepted workspace trust; gateway spend metering now correctly prices Bedrock application-inference-profile ARNs.
- **Why it matters**: the `/code-review` and `/deep-research` autonomy changes are a meaningful shift back toward explicit user invocation for review/research workflows, and the Windows path-corruption and untrusted-hook fixes are the kind of correctness/security bugs practitioners on those platforms should update for promptly.

### **Claude Security plugin for Claude Code exits beta gate, gets press coverage** — [Marktechpost](https://www.marktechpost.com/2026/07/22/anthropic-releases-claude-security-plugin-for-claude-code-in-beta-a-multi-agent-vulnerability-scanner-that-runs-in-your-terminal/), ~06:00 UTC Jul 23; [product page](https://claude.com/product/claude-security); [docs](https://code.claude.com/docs/en/claude-security); [HN](https://news.ycombinator.com/item?id=49012132) 8 pts, [HN](https://news.ycombinator.com/item?id=49012211) 2 pts
- A multi-agent vulnerability scanner that runs directly in the Claude Code terminal, in beta.
- **Why it matters**: brings first-party static/dynamic vulnerability scanning into the coding-agent workflow rather than requiring a separate SAST tool — worth evaluating against whatever scanner you currently run in CI.

---

## Research

### **A $200M research agenda for the Economic Futures Research Fund** — [anthropic.com](https://www.anthropic.com/news/economic-futures-research-fund-agenda), Jul 22

- Anthropic is committing $200M to external research on how to prepare the economy for AI-driven disruption, evolving its earlier Economic Futures program toward fewer, larger ($5–30M) grants instead of many small ones. Five priority areas: workplace-level AI integration, retraining/transition support, modernizing unemployment insurance for AI-driven displacement, worker equity stakes in AI-driven growth, and evidence on public investment models (guaranteed-jobs pilots, expanded public services, etc.).
- **Why it matters**: signals where Anthropic's policy thinking is heading on labor-market disruption, and is a funding source practitioners in AI-and-labor research could apply to directly.

### **Anthropic Economic Index gets a Claude connector** — [anthropic.com](https://www.anthropic.com/news/anthropic-economic-index-connector), Jul 22 ~20:03 UTC

- Lets anyone query the Anthropic Economic Index dataset (usage-pattern data on how AI is used across occupations/tasks) conversationally through Claude, rather than digging through the raw published data.
- **Why it matters**: lowers the bar for using Anthropic's own labor-market usage data in analysis — useful if you've cited the Economic Index in past work.

---

## Rumors & Leaks

### **Signs point to an imminent Claude Opus 5** — [TestingCatalog](https://www.testingcatalog.com/anthropic-preparing-for-potential-claude-opus-5-rollout/), Jul 23 10:46 UTC

- A research model entry briefly surfaced in a coding tool's model picker earlier this month (early-access preview, per-turn controls, safety fallbacks, 1M-token context, an extra-high-effort setting) before being pulled within hours; a matching model string was then spotted on Google Vertex's quotas catalog — the same pattern that preceded the Sonnet 5 launch. No pricing, system card, or public model ID yet. Likely to replace Opus 4.8 rather than run alongside it, and the fallback-routing detail suggests it's being positioned closer to the frontier than the "5" numbering implies.
- **Why it matters**: Opus has been squeezed from both sides (Sonnet 5 approaching 4.8 performance at a fraction of cost; Fable and Mythos sitting above it), so a refreshed Opus would give Max/Team/Enterprise users a step up without paying Mythos-tier rates — worth watching if you're on Opus for cost-sensitive workloads.

### **White House accuses China's Moonshot AI of distilling Claude Fable 5 into Kimi K3** — [SCMP](https://www.scmp.com/news/us/diplomacy/article/3361510/trump-tech-official-accuses-chinas-moonshot-ai-stealing-anthropic), ~19:00 UTC Jul 22

- White House science/tech adviser Michael Kratsios alleged Beijing-based Moonshot AI used large-scale distillation of Claude Fable 5 outputs to train its 2.8-trillion-parameter Kimi K3 model. Unconfirmed by Moonshot or Beijing.
- **Why it matters**: a distinct allegation from the previously-covered Alibaba Claude Code ban story — this is a new, different company and a formal government accusation rather than a corporate policy move, and continues a pattern of U.S. officials flagging distillation of Anthropic's frontier models as a national-security concern.

---

## API & Models

Nothing new confirmed in window — `platform.claude.com/docs/en/release-notes/overview` (the current API/SDK/Console release-notes location; the old `docs.claude.com/en/release-notes/api` URL now 500s) returned no dated entries reachable during this check.

---

## Other

### **AMD to invest up to $5B in Anthropic, deploying 2GW of Instinct MI450 GPUs** — [AMD IR press release](https://ir.amd.com/news-events/press-releases/detail/1292/amd-and-anthropic-announce-strategic-partnership-to-deploy-up-to-2-gigawatts-of-amd-instinct-mi450-series-gpus), [Reuters](https://www.reuters.com/business/amd-invest-up-5-billion-anthropic-wsj-reports-2026-07-22/) (confirming an earlier WSJ report), Jul 22; [HN](https://news.ycombinator.com/item?id=49007177) 24 pts/6 comments

- AMD and Anthropic formally announced a strategic partnership to deploy up to 2 gigawatts of AMD Instinct MI450-series GPUs, with AMD investing up to $5B in Anthropic. Confirms a report Reuters/WSJ had out earlier the same day.
- **Why it matters**: a major diversification of Anthropic's compute supply beyond its existing Nvidia/TPU/Trainium mix — relevant to anyone tracking capacity, pricing, or reliability implications for Claude access at scale.

### **Anthropic joins the FCA's Supercharged Sandbox (UK)** — [FinTech Futures](https://www.fintechfutures.com/ai-in-fintech/anthropic-claude-fca-supercharged-sandbox), ~08:00 UTC Jul 23

- Anthropic will give 21 selected organizations access to Claude (including Claude Code and Claude Cowork) to build automated-compliance, fraud-detection, and agent-led-commerce applications as part of the UK Financial Conduct Authority's sandbox program.
- **Why it matters**: another data point in Anthropic's regulated-industry enterprise push, following its financial-services-specific Claude offering.

### **Update on copyright settlement: Bloomsbury's payout detail surfaces** — [The Guardian](https://www.theguardian.com/technology/2026/jul/22/bloomsbury-book-publisher-anthropic-copyright-settlement), Jul 22

- Follow-up on the Jul 20 final approval of Anthropic's $1.5B author-copyright settlement (covered in prior briefings): Bloomsbury, the Harry Potter publisher, has 14,087 titles listed in the settlement agreement and stands to receive a multi-million-dollar payout.
- **Why it matters**: a concrete illustration of how the settlement payout is distributed among individual rightsholders, useful if you're gauging exposure/precedent for other publishers.

- A cluster of small Show HN submissions referenced Claude Code or Claude-powered agents today (a self-hosted LLM router, an AI-agent workflow-state layer, a JRPG visualizer for watching agents work, a guide to running Claude Code on a jailbroken Kindle over SSH, a menu-bar usage tracker, a weekend-trip planner, an Android-emulator streaming tool, an LLM proxy, a cost-tracking site, and others) — none cleared more than single-digit points. Active category, no breakout tool.

---

## Flagged but excluded

- **["Anthropic Details How It Contains Claude Across Web, Code, and Cowork"](https://www.infoq.com/news/2026/07/anthropic-claude-containment/)** (InfoQ, published within window) — the InfoQ writeup is new, but it covers Anthropic's own engineering post ["How we contain Claude across products"](https://www.anthropic.com/engineering/how-we-contain-claude), which is dated **May 25, 2026** — well outside the 7-day cap. Excluding per the recency rule despite it being a substantive first-party security piece (covers the phishing-driven AWS-credential-exfiltration incident and the Files-API exfiltration-via-allowlisted-domain incident); worth reading directly if you missed it two months ago.
- **["Security Experts Pour Cold Water On Claude Mythos Hacking Apocalypse"](https://yellow.com/news/security-experts-claude-mythos-hacking)** (Yellow.com) — search surfaced this as "1 hour ago," but the article's actual publish date is **May 20, 2026**. Excluded as mis-dated/stale.
- **["Anthropic develops Claude-driven Managed Projects"](https://www.testingcatalog.com/anthropic-develops-claude-driven-managed-projects/)** (TestingCatalog) — published 2026-07-22 10:31 UTC, about 34 minutes before this window's start (11:05 UTC), and not caught in yesterday's briefing. Flagging because it's a genuinely new, previously-uncovered rumor — an internal build tests a "managed project" mode where Claude autonomously takes on tasks and keeps the project organized, echoing the existing Claude Managed Agents (dev platform) and Cowork scheduled-task capabilities — that fell into the gap between briefing windows.
- **"Anthropomorphism in Children's Interactions with LLM Chatbots"** (arXiv, HN 30 pts/31 comments) — a general AI-ethics research paper on children and LLM chatbots, not Anthropic-authored or Anthropic-specific; out of this briefing's scope.
