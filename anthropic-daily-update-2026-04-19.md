# Anthropic Daily Update — April 19, 2026

---

## Claude Mythos Preview & Project Glasswing (Biggest Story)

Anthropic announced **Claude Mythos Preview** on April 7 — a new general-purpose model that is exceptionally capable at computer security tasks. Key benchmarks: 93.9% on SWE-bench Verified, 94.6% on GPQA Diamond. Anthropic says Mythos is not planned for general availability due to safety concerns.

In tandem, Anthropic launched **Project Glasswing**, an initiative to use Mythos Preview defensively to secure critical software infrastructure. During internal testing, Mythos autonomously identified **thousands of zero-day vulnerabilities** across every major OS and browser — including a 17-year-old remote code execution flaw in FreeBSD (CVE-2026-4747) that grants root from an unauthenticated internet connection.

Access is being granted to ~40 organizations responsible for critical infrastructure, plus major partners including AWS, Apple, Broadcom, Cisco, CrowdStrike, Google, JPMorganChase, Microsoft, and Nvidia. Anthropic is committing $100M in usage credits and $4M in direct donations to open-source security orgs.

The White House OMB is separately setting up protections to allow US federal agencies to begin using Mythos for defensive purposes ([Bloomberg, Apr 16](https://www.bloomberg.com/news/articles/2026-04-16/white-house-moves-to-give-us-agencies-anthropic-mythos-access)).

Notable commentary: [Bruce Schneier](https://www.schneier.com/blog/archives/2026/04/on-anthropics-mythos-preview-and-project-glasswing.html) and [Simon Willison](https://simonwillison.net/2026/Apr/7/project-glasswing/) both wrote analyses. Willison called the restricted-access approach "necessary."

---

## Claude Code Updates (April 2026)

**Ultraplan (Early Preview):** Draft plans in the cloud from the CLI, review/comment in a web editor, then run remotely or pull back local. First run auto-creates a cloud environment.

**Routines (Research Preview, Apr 14):** Complete redesign of the Claude Code desktop app (Mac + Windows). Routines come in three flavors: Scheduled (repeatable maintenance on a cadence), API (dedicated HTTP endpoints for triggering Claude from alerting tools), and Webhook (GitHub events).

**Model updates:** Claude Opus 4.7 xhigh effort level now available (sits between high and max). Auto mode available for Max subscribers with Opus 4.7.

**New commands & tools:**
- `/ultrareview` — comprehensive cloud-based code review using parallel multi-agent analysis
- `/loop` — self-paces when interval is omitted
- `/team-onboarding` — packages setup into a replayable guide
- `/autofix-pr` — enables PR auto-fix from terminal
- **Monitor tool** — streams background events into conversation for live log tailing

**Quality of life:** 1-hour and forced 5-minute prompt caching controls, session recap feature, Skill tool access to built-in slash commands, reduced permission prompts, improved Windows support, improved resume/model switching/memory usage.

---

## Valuation & IPO

Anthropic has received investor offers at an **$800 billion valuation**, up from $380B in February 2026. The company crossed **$30B in annualized revenue** (roughly 1,400% YoY growth). An IPO is expected as early as **October 2026**, potentially raising $60B+ — which would make it the second-largest IPO ever after SpaceX. Goldman Sachs, JPMorgan, and Morgan Stanley are in early talks. ([Bloomberg](https://www.bloomberg.com/news/articles/2026-04-14/anthropic-attracts-investor-offers-at-a-800-billion-valuation), [Euronews](https://www.euronews.com/business/2026/04/18/the-rapid-ascent-of-anthropic-inside-the-strategy-behind-an-800-billion-valuation))

---

## Research & Safety

**"The Hot Mess of AI" paper:** Anthropic published research decomposing frontier model errors into bias (systematic) vs. variance (incoherent) components. Key finding: as tasks get harder and reasoning chains lengthen, failures become increasingly dominated by *incoherence* rather than systematic misalignment. ([alignment.anthropic.com](https://alignment.anthropic.com/2026/hot-mess-of-ai/))

**"Automated Alignment Researchers":** New research on using LLMs to scale scalable oversight — essentially having AI systems help with their own alignment research.

**Jailbreak resistance:** Anthropic developed classifiers that filter the majority of jailbreaks while maintaining practical deployment. A prototype withstood 3,000+ hours of red teaming with no universal jailbreak discovered.

**Economic Index (March 2026):** Studied Claude usage post-Opus 4.5/4.6 releases. Found augmentation (collaborative human-AI interaction) slightly increased across both Claude.ai and API traffic, with usage diversifying across task types. ([anthropic.com](https://www.anthropic.com/research/economic-index-march-2026-report))

**Fellows Program:** Applications open for May & July 2026 cohorts, covering scalable oversight, adversarial robustness, model organisms, mechanistic interpretability, AI security, and model welfare. Deadline: April 26, 2026.

---

## Corporate & Infrastructure

- **Google Cloud expansion (Apr 6):** Anthropic expanded its partnership with Google and Broadcom for multiple gigawatts of next-generation compute. ([Google Cloud](https://www.googlecloudpresscorner.com/2026-04-06-Anthropic-Expands-Use-of-Google-Cloud-and-TPUs))
- **Board appointment (Apr 14):** Anthropic's Long-Term Benefit Trust appointed Vas Narasimhan to the Board of Directors.

---

## Hacker News Chatter

- **Third-party harness crackdown:** Anthropic is blocking third-party harnesses (including OpenClaw) from using Claude Code subscriptions as of April 4. Team plan users received a one-time $200 credit. Community reception is mixed. ([HN thread](https://news.ycombinator.com/item?id=47633396))
- **Uptime issues:** Claude lost its >99% uptime streak in Q1 2026. ([HN thread](https://news.ycombinator.com/item?id=47543189))
- **Subprocessor changes:** Discussion about Anthropic updating its subprocessor list. ([HN thread](https://news.ycombinator.com/item?id=47536110))

---

*Report compiled automatically on April 19, 2026.*
