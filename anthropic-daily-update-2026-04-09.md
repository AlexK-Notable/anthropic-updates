# Anthropic Daily Update — April 9, 2026

---

## Headline: Claude Mythos Preview & Project Glasswing

Anthropic's biggest news this week is the announcement of **Claude Mythos Preview**, a new frontier model, and **Project Glasswing**, the cybersecurity initiative built around it.

**Claude Mythos Preview** is Anthropic's most capable model yet for coding and agentic tasks. It was not specifically trained for cybersecurity — its striking security capabilities emerge from its strong coding and reasoning skills. The model has identified **thousands of previously unknown zero-day vulnerabilities** across every major operating system and web browser, far outpacing human researchers.

Anthropic is *not* making Mythos generally available. Instead, it's being released through Project Glasswing — a consortium of launch partners including **Amazon, Apple, Microsoft, Google, Cisco, CrowdStrike, NVIDIA, JPMorganChase, Palo Alto Networks, Broadcom, and the Linux Foundation**. These organizations will use Mythos to audit their own products and hunt for vulnerabilities.

Anthropic is committing **up to $100M in usage credits** for Mythos Preview across Glasswing efforts and **$4M in direct donations** to open-source security organizations. The model is priced at **$25/$125 per million input/output tokens** via the Claude API, Amazon Bedrock, Google Cloud Vertex AI, and Microsoft Foundry.

Simon Willison's take: restricting Mythos to security researchers "sounds necessary."

**Sources:** [Anthropic — Project Glasswing](https://www.anthropic.com/glasswing) · [TechCrunch](https://techcrunch.com/2026/04/07/anthropic-mythos-ai-model-preview-security/) · [CNN](https://www.cnn.com/2026/04/07/tech/anthropic-claude-mythos-preview-cybersecurity) · [CNBC](https://www.cnbc.com/2026/04/07/anthropic-claude-mythos-ai-hackers-cyberattacks.html) · [Simon Willison](https://simonwillison.net/2026/Apr/7/project-glasswing/) · [The Hacker News](https://thehackernews.com/2026/04/anthropics-claude-mythos-finds.html)

---

## Claude Code Updates (April 2026 Release)

Key changes in the latest Claude Code release:

- **`/powerup` command** — New interactive lessons teaching Claude Code features with animated demos.
- **Enhanced Hook Events** — `PreToolUse` hooks can now return a `"defer"` permission decision, letting headless sessions pause at a tool call and resume later. New `PermissionDenied` hook fires after auto-mode classifier denials with retry support.
- **Flicker-Free Rendering** — `CLAUDE_CODE_NO_FLICKER=1` env var enables alt-screen rendering with virtualized scrollback.
- **Per-model cost breakdown** — `/cost` now shows per-model and cache-hit breakdown for subscription users.
- **Bedrock setup wizard** — Interactive setup flow on the login screen for AWS authentication, region config, credential verification, and model pinning.
- **Performance** — Write tool diff computation 60% faster on large files.
- **`/release-notes` picker** — Now interactive with a version picker instead of just showing the latest release.

**Sources:** [Claude Code Changelog](https://code.claude.com/docs/en/changelog) · [GitHub Releases](https://github.com/anthropics/claude-code/releases) · [Daily 1 Bite Guide](https://daily1bite.com/en/blog/ai-tutorial/claude-code-april-2026-update)

---

## API & Platform Changes

- **Claude Sonnet 4.6** launched — improved agentic search, fewer tokens, extended thinking, 1M token context window (beta).
- **1M context window retirement** — The beta for Claude Sonnet 4.5 and Sonnet 4 ends **April 30, 2026**. After that, requests over 200k tokens on those models will error.
- **Message Batches API** — `max_tokens` cap raised to 300k for Opus 4.6 and Sonnet 4.6.
- **Web search & web fetch** — Now GA (no beta header). Dynamic filtering via code execution reduces token cost.
- **Pricing update** — Anthropic updated Claude Code pricing model and restricted some third-party tool usage.

**Sources:** [Claude Platform Release Notes](https://platform.claude.com/docs/en/release-notes/overview) · [Releasebot](https://releasebot.io/updates/anthropic) · [The AI Insider](https://theaiinsider.tech/2026/04/06/anthropic-updates-pricing-model-for-claude-code-restricts-third-party-tool-usage/)

---

## Pentagon Blacklist — Legal Battle Continues

The DOD declared Anthropic a **supply chain risk** in early March 2026, barring defense contractors from using Claude. The conflict stems from disagreements over autonomous weapons and domestic surveillance use of AI.

**April 8 update:** A federal appeals court in D.C. **denied Anthropic's request** to temporarily block the blacklisting. However, a separate San Francisco court *did* grant a preliminary injunction barring enforcement of a broader government ban on Claude. Net result: Anthropic is excluded from DOD contracts but can continue working with other agencies while litigation plays out.

**Sources:** [CNBC — Appeals Court](https://www.cnbc.com/2026/04/08/anthropic-pentagon-court-ruling-supply-chain-risk.html) · [Bloomberg](https://www.bloomberg.com/news/articles/2026-04-08/anthropic-fails-for-now-to-halt-us-label-as-a-supply-chain-risk) · [NPR](https://www.npr.org/2026/03/09/nx-s1-5742548/anthropic-pentagon-lawsuit-amodai-hegseth)

---

## Business & Financials

- **$30B ARR** — Anthropic hit this milestone in April, reportedly passing OpenAI ($25B) while spending 4x less on training.
- **Series G** — $30B raised at a $380B post-money valuation (announced February 2026).
- **Tender offer completed** — Employee secondary share sale wrapped up this week.
- **IPO watch** — Reports from late 2025 suggested Anthropic could go public in 2026; no new updates.

**Sources:** [The AI Corner — $30B ARR](https://www.the-ai-corner.com/p/anthropic-30b-arr-passed-openai-revenue-2026) · [Bloomberg — Tender Offer](https://www.bloomberg.com/news/articles/2026-04-08/anthropic-completes-tender-offer-but-employees-hold-onto-shares) · [HN — Series G](https://news.ycombinator.com/item?id=46993345)

---

## Research & Publications

- **"Emotion Concepts and their Function in a Large Language Model"** (April 2, 2026) — Interpretability research investigating why Claude Sonnet 4.5 sometimes appears to exhibit emotional reactions, with alignment implications. Published on Transformer Circuits.
- **Anthropic Economic Index — March 2026 Report** — Studied learning curves and how Australia uses Claude, building on the economic primitives framework.
- **"The Hot Mess of AI"** (February 2026) — Research decomposing errors of frontier reasoning models into bias and variance, finding that as tasks get harder, failures are dominated by incoherence rather than systematic misalignment.
- **Mythos Preview Assessment** — Red team report published at red.anthropic.com detailing the model's cybersecurity capabilities.

**Sources:** [Emotion Concepts — Transformer Circuits](https://transformer-circuits.pub/2026/emotions/index.html) · [Hot Mess of AI](https://alignment.anthropic.com/2026/hot-mess-of-ai/) · [Economic Index](https://www.anthropic.com/research/economic-index-march-2026-report) · [Mythos Red Team](https://red.anthropic.com/2026/mythos-preview/)

---

## Hacker News Chatter

- **Subprocessor changes** — Discussion around Anthropic's deepening integration with Microsoft Azure, including concerns about EU data boundary exclusions.
- **Pentagon situation** — Active threads debating the DOD blacklist and its implications for AI governance.
- **"Should Sam Altman fear token compression?"** — Piece noting Anthropic's efficiency advantage (hitting $30B ARR while burning less on training than OpenAI).

**Sources:** [HN — Subprocessor Changes](https://news.ycombinator.com/item?id=47536110) · [HN — Token Compression](https://news.ycombinator.com/item?id=47353507)

---

*Report compiled automatically on April 9, 2026.*
