# Anthropic Daily Update — April 17, 2026

---

## Headline: Claude Opus 4.7 Released; Mythos Dominates National Security Discussion

---

## 1. Claude Opus 4.7 Launch (April 16)

Anthropic released **Claude Opus 4.7** as a general-availability upgrade across all Claude products, APIs, and cloud partners (AWS Bedrock, Google Cloud, Azure). Key improvements:

- **13% lift on coding benchmarks**, 3x more production tasks resolved vs. Opus 4.6
- **High-resolution vision** — first Claude model to support up to 2,576px / 3.75MP images (up from 1,568px / 1.15MP)
- **New tokenizer** for improved efficiency
- **128K max output tokens**, 1M context window at standard pricing (no long-context surcharge)
- **Same pricing as Opus 4.6** ($5/$25 per MTok)
- **Cybersecurity safeguards** — automatic detection/blocking of prohibited or high-risk cyber uses, plus a new Cyber Verification Program for legitimate security researchers
- Benchmarks show Opus 4.7 beats ChatGPT 5.4 and Gemini 3.1 Pro on key tasks, though it still trails unreleased Mythos internally

Sources: [CNBC](https://www.cnbc.com/2026/04/16/anthropic-claude-opus-4-7-model-mythos.html) | [Axios](https://www.axios.com/2026/04/16/anthropic-claude-opus-model-mythos) | [VentureBeat](https://venturebeat.com/technology/anthropic-releases-claude-opus-4-7-narrowly-retaking-lead-for-most-powerful-generally-available-llm) | [AWS Blog](https://aws.amazon.com/blogs/aws/introducing-anthropics-claude-opus-4-7-model-in-amazon-bedrock/)

---

## 2. Mythos: The Model Too Dangerous to Release

The biggest ongoing story in AI remains **Claude Mythos Preview**, Anthropic's unreleased frontier model with unprecedented cybersecurity capabilities:

- Mythos autonomously discovered a **17-year-old remote code execution vulnerability in FreeBSD** and exploited it for root access
- Over the past weeks, Anthropic used Mythos to identify **thousands of zero-day vulnerabilities** across every major OS and browser — over 99% remain unpatched
- Mythos successfully reproduced and created proof-of-concept exploits on first attempt in **83.1% of cases**
- Rather than public release, Anthropic launched **Project Glasswing** — a defensive security initiative with 12 partners including Amazon, Apple, Microsoft, Cisco, CrowdStrike, Broadcom, Palo Alto Networks, and the Linux Foundation
- Bloomberg reports the **White House is moving to give US agencies Mythos access**, and the **US Treasury is seeking access** to probe for financial system vulnerabilities
- **Today (April 17)**: Anthropic CEO Dario Amodei is meeting White House chief of staff Susie Wiles to resolve the company's dispute with the Pentagon, which blacklisted Anthropic after Amodei refused to allow unrestricted military use

Sources: [TechCrunch](https://techcrunch.com/2026/04/07/anthropic-mythos-ai-model-preview-security/) | [Fortune](https://fortune.com/2026/04/14/anthropic-mythos-reveals-security-gap-ai-finds-flaws-far-faster-than-companies-can-patch-them/) | [Axios](https://www.axios.com/2026/04/17/anthropic-trump-administration-mythos) | [Anthropic Glasswing](https://www.anthropic.com/glasswing) | [The Hill](https://thehill.com/policy/technology/5829315-anthropic-mythos-ai-cybersecurity-risks/) | [CFR Analysis](https://www.cfr.org/articles/six-reasons-claude-mythos-is-an-inflection-point-for-ai-and-global-security)

---

## 3. Claude Code Updates (April 14)

Major Claude Code release this week:

- **Desktop App Redesign** — new multi-session sidebar, integrated terminal, file editing, HTML/PDF preview, faster diff viewer (Mac + Windows)
- **Routines** (research preview) — cloud-based automation: save a prompt + repos + connectors, schedule it to run server-side without your machine being online
- **Ultraplan** (early preview) — draft plans in the cloud from CLI, review/comment in a web editor, execute remotely or pull back local
- **New tools**: Monitor (streaming background events), `/loop` self-pacing, `/team-onboarding`, `/autofix-pr`
- **Opus 4.7 xhigh** effort level available in Claude Code
- **Auto mode** for Max subscribers

Sources: [VentureBeat](https://venturebeat.com/orchestration/we-tested-anthropics-redesigned-claude-code-desktop-app-and-routines-heres-what-enterprises-should-know) | [SiliconANGLE](https://siliconangle.com/2026/04/14/anthropics-claude-code-gets-automated-routines-desktop-makeover/) | [9to5Mac](https://9to5mac.com/2026/04/14/anthropic-adds-repeatable-routines-feature-to-claude-code-heres-how-it-works/) | [Apiyi Changelog](https://help.apiyi.com/en/claude-code-changelog-2026-april-updates-en.html)

---

## 4. Business & Financials

- **$800B+ valuation**: Anthropic has received multiple investor offers for a new funding round at approximately $800 billion or higher (Bloomberg, April 14)
- **~$19B annualized revenue**: Anthropic is approaching $19 billion in annualized revenue
- **Google Cloud expansion** (April 6): Anthropic announced expanded use of TPU chips and cloud services for scaling foundation models, agents, and enterprise apps

Sources: [Bloomberg](https://www.bloomberg.com/news/articles/2026-04-14/anthropic-attracts-investor-offers-at-a-800-billion-valuation) | [Google Cloud](https://www.googlecloudpresscorner.com/2026-04-06-Anthropic-Expands-Use-of-Google-Cloud-and-TPUs)

---

## 5. Competitive Landscape

- **OpenAI**: Surpassed $25B annualized revenue; exploring late-2026 IPO; launched a drug-discovery-focused AI model (April 16)
- **Meta**: Debuted "Muse Spark" (codename Avocado), first model from Meta Superintelligence Labs under Alexandr Wang
- **Google**: Launched Gemma 4, an Apache 2.0 open-weight model family — major open-source push
- **Joint anti-copying effort**: OpenAI, Anthropic, and Google are collaborating to prevent Chinese competitors from cloning US frontier models via distillation
- **Combined Anthropic + OpenAI valuation now exceeds $1 trillion**

Sources: [Bloomberg](https://www.bloomberg.com/news/articles/2026-04-06/openai-anthropic-google-unite-to-combat-model-copying-in-china) | [CNBC - Meta](https://www.cnbc.com/2026/04/08/meta-debuts-first-major-ai-model-since-14-billion-deal-to-bring-in-alexandr-wang.html)

---

## 6. Hacker News Chatter

- Discussion around Anthropic's **April 13 outage** affecting login and account access
- Ongoing thread about Anthropic **subprocessor changes** (potential Azure infrastructure move)
- Active debate threads on the Anthropic–Pentagon dispute and Mythos safety implications
- Community tool tracking real-time AI model pricing across 44 models including Claude

Sources: [HN - Outage](https://news.ycombinator.com/item?id=47763968) | [HN - Subprocessor](https://news.ycombinator.com/item?id=47536110)

---

*Report compiled automatically on April 17, 2026.*
