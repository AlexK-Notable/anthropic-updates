# Anthropic Daily Update — April 7, 2026

---

## Top Story: Project Glasswing & Claude Mythos Preview

Anthropic today debuted **Project Glasswing**, a cybersecurity initiative built around its unreleased frontier model, **Claude Mythos**. Mythos is Anthropic's most powerful model to date and is being made available in limited preview exclusively for defensive security work.

Twelve founding partner organizations — including Amazon, Apple, Microsoft, Google, Broadcom, Cisco, CrowdStrike, JPMorganChase, and Nvidia — will use Mythos Preview to hunt for vulnerabilities in their products and critical infrastructure. Roughly 40 additional organizations responsible for critical software and open-source projects are also receiving access.

**Key claim:** Anthropic says that in recent weeks, Mythos identified "thousands of zero-day vulnerabilities, many of them critical" across every major operating system and web browser. Tom's Hardware reports many of these bugs are 10–20 years old, with the oldest being a now-patched 27-year-old OpenBSD flaw. Mythos can convert 72.4% of identified vulnerabilities into working exploits (in Firefox's JS shell) and achieves register control in an additional 11.6% of attempts. In one case it chained four vulnerabilities into a full browser exploit and autonomously obtained local privilege escalation on Linux.

These offensive capabilities are precisely why the rollout is restricted — CNBC reports Anthropic fears the model could enable large-scale AI-driven cyberattacks. Project Glasswing is backed by a **$100 million** cybersecurity initiative. There are no plans to make Mythos Preview generally available; Anthropic says it will deploy Mythos-class models at scale only when new safeguards are in place.

Simon Willison [commented](https://simonwillison.net/2026/Apr/7/project-glasswing/) that restricting Mythos to security researchers "sounds necessary."

**Sources:** [Fortune](https://fortune.com/2026/04/07/anthropic-claude-mythos-model-project-glasswing-cybersecurity/) · [TechCrunch](https://techcrunch.com/2026/04/07/anthropic-mythos-ai-model-preview-security/) · [CNBC](https://www.cnbc.com/2026/04/07/anthropic-claude-mythos-ai-hackers-cyberattacks.html) · [CrowdStrike Blog](https://www.crowdstrike.com/en-us/blog/crowdstrike-founding-member-anthropic-mythos-frontier-model-to-secure-ai/) · [Anthropic](https://www.anthropic.com/glasswing)

---

## Business & Financial

### Revenue Hits $30B ARR, Surpasses OpenAI

Anthropic's annualized revenue run rate has topped **$30 billion**, up from $9B at end of 2025 and $1B in December 2024. More than 1,000 business customers now spend over $1M annually, more than doubling since February. Eight of the Fortune 10 use Claude, and 80% of revenue comes from ~300,000 enterprise customers.

**Sources:** [The AI Corner](https://www.the-ai-corner.com/p/anthropic-30b-arr-passed-openai-revenue-2026) · [Bloomberg](https://www.bloomberg.com/news/articles/2026-04-06/broadcom-confirms-deal-to-ship-google-tpu-chips-to-anthropic)

### Broadcom / Google Compute Deal

Anthropic sealed a new agreement with Google and Broadcom for **3.5 gigawatts of compute capacity** (coming online in 2027) to power Claude models. This was confirmed in a Broadcom SEC filing.

**Sources:** [TechCrunch](https://techcrunch.com/2026/04/07/anthropic-compute-deal-google-broadcom-tpus/) · [CNBC](https://www.cnbc.com/2026/04/06/broadcom-agrees-to-expanded-chip-deals-with-google-anthropic.html)

### IPO Trajectory

Anthropic is expected to list **as early as October 2026**, targeting a $60B+ raise at a $400–500B valuation. Goldman Sachs and JPMorgan Chase are in active discussions. The company closed a $30B funding round at $380B valuation in February. Key risk: Anthropic plans to spend ~$19B on training and inference in 2026, roughly matching revenue, and gross margins have fallen to 40%.

**Sources:** [Fortune](https://fortune.com/2026/04/07/spacex-openai-anthropic-reopen-ipo-market-crunchbase/) · [WinBuzzer](https://winbuzzer.com/2026/03/30/anthropic-ipo-q4-2026-60-billion-target-xcxwbn/)

---

## Claude Code Updates (First Week of April)

Four releases shipped this week (v2.1.89 through v2.1.92). Highlights:

- **`/powerup` command** — Interactive lessons with animated demos for learning Claude Code features.
- **MCP 500K** — Tool result character limit increased to 500,000 characters.
- **`defer` permission for hooks** — PreToolUse hooks can now pause headless sessions and resume externally.
- **`/cost breakdown`** — Per-model and cache-hit cost analysis now available.
- **Performance** — Write tool diff computation is 60% faster; SSE transport now O(n) instead of O(n²).
- **Windows PowerShell preview** added, along with richer admin and MCP controls.
- **Removed** — `/tag` and `/vim` commands dropped in v2.1.92 due to low usage.
- **`/release-notes`** now presents an interactive version picker instead of just showing the latest.

**Sources:** [Claude Code Changelog](https://code.claude.com/docs/en/changelog) · [GitHub Releases](https://github.com/anthropics/claude-code/releases) · [Daily 1 Bite](https://daily1bite.com/en/blog/ai-tutorial/claude-code-april-2026-update)

---

## Policy: OpenClaw / Third-Party Harness Subscription Cut-Off

As of **April 4 at noon PT**, Claude Code subscribers can no longer use subscription limits for third-party harnesses like OpenClaw. Users must now opt into pay-as-you-go billing or use the API directly. Anthropic cited compute strain — third-party tools often bypass prompt-cache optimizations that first-party tools rely on.

A one-time credit equal to the monthly subscription price is available until April 17.

**Context:** OpenClaw creator Peter Steinberger left for OpenAI in February; the restrictions were announced shortly after. This generated significant backlash on Hacker News.

**Sources:** [TechCrunch](https://techcrunch.com/2026/04/04/anthropic-says-claude-code-subscribers-will-need-to-pay-extra-for-openclaw-support/) · [HN Discussion](https://news.ycombinator.com/item?id=47633396) · [VentureBeat](https://venturebeat.com/technology/anthropic-cuts-off-the-ability-to-use-claude-subscriptions-with-openclaw-and)

---

## Industry: Anti-Distillation Alliance

OpenAI, Anthropic, and Google are now **sharing intelligence through the Frontier Model Forum** to detect adversarial distillation attempts by Chinese competitors. The effort follows Anthropic's February accusation that DeepSeek, Moonshot, and MiniMax illegally copied Claude's capabilities. U.S. officials estimate unauthorized distillation costs Silicon Valley labs billions annually.

**Sources:** [Bloomberg](https://www.bloomberg.com/news/articles/2026-04-06/openai-anthropic-google-unite-to-combat-model-copying-in-china) · [Japan Times](https://www.japantimes.co.jp/business/2026/04/07/tech/openai-anthropic-google-china-copy/)

---

## Research & Safety

- **Anthropic Fellows Program** — Applications open for May and July 2026 cohorts, expanding to cover scalable oversight, adversarial testing, and more safety research areas.
- **Natural Emergent Misalignment** — Anthropic published a paper on emergent misalignment from reward hacking in production RL systems.
- **New team** focused on mitigating post-deployment risks of AI systems is being formed.

**Sources:** [Alignment Blog](https://alignment.anthropic.com/2025/anthropic-fellows-program-2026/) · [Research Paper](https://assets.anthropic.com/m/74342f2c96095771/original/Natural-emergent-misalignment-from-reward-hacking-paper.pdf)

---

*Report generated automatically on April 7, 2026.*
