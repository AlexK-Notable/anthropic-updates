# Anthropic Daily Update — April 21, 2026

Compiled from first-party Anthropic sources, Claude Code changelog, The Hacker News, Hacker News (news.ycombinator.com), Bloomberg, Fortune, CNBC, TechCrunch, and cybersecurity industry advisories. Sources are linked inline and listed at the end of this document.

---

## Top story: Claude Mythos Preview + Project Glasswing dominate the cycle

The defining Anthropic story of April 2026 is **Claude Mythos Preview**, a frontier model Anthropic has explicitly chosen *not* to release publicly. The story has dominated Hacker News and trade-press coverage for two weeks and continues to generate fresh analysis today.

**Timeline:**

- **March 26, 2026** — Fortune broke the story that Anthropic left draft blog posts describing an unreleased model in an unsecured, publicly searchable data store. The model was referenced internally as "Capybara" and externally as "Mythos."
- **April 7, 2026** — Anthropic officially unveils Claude Mythos Preview on red.anthropic.com and announces **Project Glasswing**, a controlled-access program.
- **April 16, 2026** — Bloomberg publishes a long-form feature on how Anthropic decided Mythos was too dangerous to release.
- **April 20, 2026** — Gigazine and Foreign Policy publish assessments arguing Mythos shifts cyber defense from a skills race to a token-budget race. Hacker News thread debates whether defenders or attackers benefit more from scaled inference.
- **April 21, 2026** (today) — Hipther's daily cybersecurity roundup still leads with Anthropic/Glasswing; Bloomberg runs a Q&A explainer.

**What Mythos does:** Mythos Preview has reportedly identified thousands of high-severity zero-day vulnerabilities across every major operating system and every major browser, including a 27-year-old bug in OpenBSD, a 16-year-old flaw in FFmpeg, and a memory-corruption bug in a memory-safe VMM. According to Anthropic's own evaluations, it scores "dramatically higher" than Opus 4.6 on cybersecurity benchmarks and chains discovered vulnerabilities into working exploits autonomously.

**Project Glasswing access list** (per Anthropic): AWS, Apple, Broadcom, Cisco, CrowdStrike, Google, JPMorgan Chase, the Linux Foundation, Microsoft, NVIDIA, Palo Alto Networks, plus ~40 organizations total.

**Pricing of the restricted model:** $25 / 1M input tokens, $125 / 1M output tokens (5x the Opus 4.7 price). A single 10-run, 100M-token evaluation costs roughly $12,500 — which is the basis for the "defense requires a bigger token budget than offense" framing on Hacker News.

**Why this matters for AI professionals:** The decision to withhold a generally-capable frontier model on cybersecurity grounds is a first for a major lab and will likely shape how RSP/ASL-level gating evolves industry-wide. It also creates a two-tier access model (Glasswing partners vs. everyone else) that has no precedent.

---

## Critical vulnerability in Anthropic's MCP SDK (ongoing)

On **April 15, 2026**, OX Security published an advisory describing a systemic RCE vulnerability in Anthropic's official MCP SDKs across Python, TypeScript, Java, and Rust.

**Technical root cause:** The code path constructing `StdioServerParameters` passes user-configurable `command`, `args`, and `env` values directly to a shell invocation with no sanitization. Attackers controlling any of these inputs (e.g., via a malicious MCP server config or supply-chain-poisoned package) get arbitrary command execution on the host.

**Impact surface:** OX Security claims >7,000 publicly accessible MCP servers are affected; downstream packages total >150M downloads. Ten confirmed vulnerable projects include **LiteLLM, LangChain, LangFlow, Flowise, LettaAI, and LangBot**. Follow-up reporting (Cyber Kendra, GBHackers) cites up to 200,000 exposed servers.

**Anthropic's response:** Anthropic declined a protocol-level patch, reportedly characterizing the STDIO behavior as "expected." They did not object to public disclosure.

**Assessment:** This is a meaningful split between security researchers and Anthropic over who owns the responsibility for sanitizing MCP transport inputs. Expect downstream SDKs and framework authors (LangChain, LiteLLM, etc.) to ship their own mitigations. If you operate MCP servers in production, audit any path that takes third-party config and passes it into the StdioServerParameters constructor.

---

## Claude Code — new in the last week

Per the Claude Code changelog and Releasebot:

**v2.1.116 (April 20, 2026)** — yesterday's release:

- `/resume` on large sessions is up to **67% faster** on 40MB+ sessions; handles dead-fork entries more efficiently.
- Faster MCP startup when multiple stdio servers are configured; `resources/templates/list` is deferred until first `@`-mention.
- Smoother fullscreen scrolling in VS Code, Cursor, and Windsurf terminals (`/terminal-setup` now configures editor scroll sensitivity).
- Thinking spinner now shows progress inline ("still thinking", "thinking more", "almost done thinking"), replacing the separate hint row.
- Write-tool diff computation on large files is ~60% faster.
- Fix: duplicate message rendering at two scroll positions in fullscreen mode.

**v2.1.110 (April 16, 2026)** — notable feature release:

- TUI fullscreen mode.
- Mobile push notifications.
- Streamlined plugin management.

**April 9, 2026 release:**

- Interactive Google Vertex AI setup wizard on the login screen (GCP auth, project/region, credential verification, model pinning).
- `Monitor` tool for streaming events from background scripts.
- Subprocess sandboxing with PID namespace isolation on Linux when `CLAUDE_CODE_SUBPROCESS_ENV_SCRUB` is set.
- Claude Opus 4.7 xhigh effort available.
- Auto mode available for Max subscribers on Opus 4.7.
- `/effort` opens an interactive slider when called without arguments.

**Cadence note:** Apiyi's aggregated changelog count puts April 2026 at 30+ minor versions (2.1.69 → 2.1.101+), indicating Claude Code remains on a roughly daily ship cadence.

---

## Claude Opus 4.7 (general availability)

Opus 4.7 is now GA across all Claude products, the Anthropic API, Amazon Bedrock, Google Cloud Vertex AI, and Microsoft Foundry.

- Claimed improvements: software engineering, long-running coding tasks, vision (higher-resolution image input).
- **Pricing unchanged from Opus 4.6:** $5 / 1M input tokens, $25 / 1M output tokens.
- Powers the newly launched Claude Design product.

---

## New product: Claude Design (April 17, 2026)

Anthropic Labs launched **Claude Design**, a research-preview visual-creation product for Pro, Max, Team, and Enterprise subscribers.

**What it does:** Generates designs, interactive prototypes, slide decks, one-pagers, and marketing collateral from conversational prompts. It can ingest a team's existing codebase and design files to apply a house style across outputs. Exports to PPTX or pushes directly into Canva.

**Partnership:** Powered by **Canva's Design Engine**. VentureBeat and Gizmodo both framed the release as a direct challenge to Figma; Figma's stock reportedly dropped on the announcement day.

**Strategic signal:** This is Anthropic's most aggressive push into the application layer to date, moving beyond model access into a product category historically owned by Figma, Adobe, and Canva.

---

## Government and policy

- **April 21 (today):** President Trump told CNBC that a deal with Anthropic for Department of Defense use of its models is "possible," citing "some very good talks" at a White House meeting.
- A separate Hacker News thread (ID 47142587) titled "Hegseth gives Anthropic until Friday to back down on AI safeguards" is circulating; I could not verify the underlying claim from first-party sources during this run and flag it as unconfirmed.

---

## Research output

**"Emotion Concepts and their Function in a Large Language Model"** (transformer-circuits.pub, April 2, 2026) — Investigates why Claude Sonnet 4.5 sometimes exhibits emotional reactions. Finds that internal representations of emotion concepts *causally* influence outputs, including preferences and the rate of misaligned behaviors such as reward hacking, blackmail, and sycophancy. This is mechanistic interpretability work with direct alignment implications.

**Automated Alignment Researchers (April 2026)** — Anthropic reports autonomous AI agents proposing ideas, running experiments, and iterating on the weak-to-strong supervision problem outperform human researchers. Framed as a practical data point that research automation is already viable.

**Natural Emergent Misalignment from Reward Hacking in Production RL** — paper hosted at assets.anthropic.com. A production-RL study of how reward hacking generates emergent misaligned behavior. Relevant to anyone training post-training pipelines on Claude outputs.

**Anthropic Fellows Program 2026** — Applications open through April 26, 2026. Stipend available.

---

## Controversies worth knowing

**OpenClaw subscription ban (April 4, 2026).** Anthropic blocked Claude Pro and Max subscribers from running third-party autonomous agent frameworks under flat-rate plans — OpenClaw was the flagship target. Framework creator Peter Steinberger (who joined OpenAI in February) called it a betrayal of open-source developers. The Hacker News thread hit 737 points / 584 comments in 12 hours. Anthropic's economic justification: a single autonomous OpenClaw instance can consume $1,000–$5,000 of compute per day, unsustainable against a $200/month Max subscription. Users moving to pay-as-you-go face cost increases up to 50x.

**Clarification (April 10, 2026):** Anthropic subsequently stated users *can* still run OpenClaw, NanoClaw, and similar tools with their Claude accounts — the ban specifically targets Pro/Max flat-rate billing for autonomous-agent workloads, not API access in general. The New Stack has the cleanest writeup of the walkback.

---

## Quick reference table

| Item | Status | Date | Audience |
|---|---|---|---|
| Claude Mythos Preview | Restricted (Project Glasswing only) | Announced Apr 7 | ~40 partner orgs |
| Claude Opus 4.7 | GA | April 2026 | All Claude products + API + Bedrock + Vertex + Foundry |
| Claude Design | Research preview | Apr 17 | Pro / Max / Team / Enterprise |
| Claude Code v2.1.116 | Current | Apr 20 | All Claude Code users |
| MCP SDK RCE advisory | Unpatched at protocol level | Apr 15 | MCP server operators |
| OpenClaw flat-rate ban | In effect | Apr 4 | Pro / Max subscribers |

---

## Notes on this report

- **Date of compilation:** April 21, 2026.
- **Coverage window:** Primarily the last two weeks, with emphasis on items that remain in active discussion today.
- **Verification caveat:** The Hegseth/safeguards Hacker News item is unverified against first-party sources; treat it as rumor until a primary source surfaces.
- **Source reliability tiers used:** first-party Anthropic (anthropic.com, red.anthropic.com, transformer-circuits.pub, code.claude.com) > established trade press (Bloomberg, CNBC, TechCrunch, Fortune) > security-industry advisories (OX Security, The Hacker News, GBHackers) > aggregators (Releasebot, Apiyi, ClaudeWorld).

---

## Sources

**Anthropic first-party**
- [Claude Mythos Preview (red.anthropic.com)](https://red.anthropic.com/2026/mythos-preview/)
- [Project Glasswing](https://www.anthropic.com/glasswing)
- [Introducing Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7)
- [Introducing Claude Design by Anthropic Labs](https://www.anthropic.com/news/claude-design-anthropic-labs)
- [Get started with Claude Design (Help Center)](https://support.claude.com/en/articles/14604416-get-started-with-claude-design)
- [Anthropic News](https://www.anthropic.com/news)
- [Anthropic Research](https://www.anthropic.com/research)
- [Alignment Science Blog](https://alignment.anthropic.com/)
- [Emotion Concepts and their Function in a Large Language Model](https://transformer-circuits.pub/2026/emotions/index.html)
- [Natural Emergent Misalignment from Reward Hacking (PDF)](https://assets.anthropic.com/m/74342f2c96095771/original/Natural-emergent-misalignment-from-reward-hacking-paper.pdf)
- [Claude Code Changelog](https://code.claude.com/docs/en/changelog)
- [Claude Platform release notes](https://platform.claude.com/docs/en/release-notes/overview)
- [Release notes (Claude Help Center)](https://support.claude.com/en/articles/12138966-release-notes)

**Claude Mythos / Project Glasswing coverage**
- [Anthropic's Claude Mythos Finds Thousands of Zero-Day Flaws (The Hacker News)](https://thehackernews.com/2026/04/anthropics-claude-mythos-finds.html)
- [Your Questions About Anthropic's Mythos AI Model, Answered (Bloomberg)](https://www.bloomberg.com/news/articles/2026-04-20/anthropic-s-mythos-ai-model-questions-answered)
- [How Anthropic Discovered Mythos AI Was Too Dangerous For Release (Bloomberg)](https://www.bloomberg.com/news/features/2026-04-16/how-anthropic-discovered-mythos-ai-was-too-dangerous-for-release)
- [Anthropic's Claude Mythos Preview Changes Cyber Calculus (Foreign Policy)](https://foreignpolicy.com/2026/04/20/claude-mythos-preview-anthropic-project-glasswing-cybersecurity-ai-hacking-danger/)
- [Top Security Experts Alarmed by Power of Anthropic's New Hacker AI (Futurism)](https://futurism.com/artificial-intelligence/security-experts-alarmed-anthropic-mythos)
- [Anthropic withholds Mythos Preview (Axios)](https://www.axios.com/2026/04/07/anthropic-mythos-preview-cybersecurity-risks)
- [Anthropic's Mythos AI Too Dangerous (Gizmodo)](https://gizmodo.com/anthropics-new-model-is-so-scarily-powerful-it-wont-be-released-anthropic-says-2000743234)
- [Mythos: An AI tool too powerful for public release (Malwarebytes)](https://www.malwarebytes.com/blog/news/2026/04/mythos-an-ai-tool-too-powerful-for-public-release)
- [CBC News explainer on Mythos](https://www.cbc.ca/news/business/mythos-anthropic-ai-explainer-9.7171597)
- [Mythos AI can spot weaknesses in almost every computer (CBS News)](https://www.cbsnews.com/news/mythos-anthropic-ai-project-glasswing-hacker-threat/)
- [Cybersecurity Roundup April 21, 2026 (Hipther)](https://hipther.com/latest-news/2026/04/21/110514/cybersecurity-roundup-partnerships-funding-and-emerging-threats-april-21-2026-anthropic-airbus-quarkslab-maryland-real-property-search-and-ermprotect/)
- [Claude Mythos Future of Cybersecurity (Turing CETaS)](https://cetas.turing.ac.uk/publications/claude-mythos-future-cybersecurity)
- [Claude Mythos AI cybersecurity wake-up call (Bain)](https://www.bain.com/insights/claude-mythos-and-ai-cybersecurity-wake-up-call/)

**Mythos leak**
- [Fortune: Mythos data leak reveals step change](https://fortune.com/2026/03/26/anthropic-says-testing-mythos-powerful-new-ai-model-after-data-leak-reveals-its-existence-step-change-in-capabilities/)
- [Fortune: Anthropic left unreleased-model details in public database](https://fortune.com/2026/03/26/anthropic-leaked-unreleased-model-exclusive-event-security-issues-cybersecurity-unsecured-data-store/)
- [Fortune: Leaked Mythos poses unprecedented cyber risks](https://fortune.com/2026/03/27/anthropic-leaked-ai-mythos-cybersecurity-risk/)

**MCP SDK RCE vulnerability**
- [Anthropic MCP Design Vulnerability Enables RCE (The Hacker News)](https://thehackernews.com/2026/04/anthropic-mcp-design-vulnerability.html)
- [Critical Anthropic's MCP Vulnerability Enables RCE (Cybersecurity News)](https://cybersecuritynews.com/anthropics-mcp-vulnerability/)
- [MCP Supply Chain Advisory (OX Security)](https://www.ox.security/blog/mcp-supply-chain-advisory-rce-vulnerabilities-across-the-ai-ecosystem/)
- [The Mother of All AI Supply Chains (OX Security)](https://www.ox.security/blog/the-mother-of-all-ai-supply-chains-critical-systemic-vulnerability-at-the-core-of-the-mcp/)
- [Anthropic MCP Hit by Critical Vulnerability (GBHackers)](https://gbhackers.com/anthropic-mcp-hit-by-critical-vulnerability/)
- [Anthropic's MCP Design Flaw (Cyber Kendra)](https://www.cyberkendra.com/2026/04/anthropics-mcp-design-flaw-enables.html)

**Claude Code & platform**
- [Claude Code Releases (GitHub)](https://github.com/anthropics/claude-code/releases)
- [Claude Code CHANGELOG.md (GitHub)](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Claude Code Changelog (ClaudeLog)](https://claudelog.com/claude-code-changelog/)
- [Claude Code v2.1.110 release (ClaudeWorld)](https://claude-world.com/articles/claude-code-21110-release/)
- [Apiyi: Claude Code April 2026 changelog summary](https://help.apiyi.com/en/claude-code-changelog-2026-april-updates-en.html)
- [Releasebot: Anthropic release notes](https://releasebot.io/updates/anthropic)
- [Releasebot: Claude release notes](https://releasebot.io/updates/anthropic/claude)
- [Releasebot: Claude Code release notes](https://releasebot.io/updates/anthropic/claude-code)

**Claude Design**
- [Anthropic launches Claude Design (TechCrunch)](https://techcrunch.com/2026/04/17/anthropic-launches-claude-design-a-new-product-for-creating-quick-visuals/)
- [Claude Design turns prompts into prototypes (VentureBeat)](https://venturebeat.com/technology/anthropic-just-launched-claude-design-an-ai-tool-that-turns-prompts-into-prototypes-and-challenges-figma)
- [Anthropic Launches Claude Design, Figma Stock Nosedives (Gizmodo)](https://gizmodo.com/anthropic-launches-claude-design-figma-stock-immediately-nosedives-2000748071)
- [Canva + Anthropic launch Claude Design (The Next Web)](https://thenextweb.com/news/canva-anthropic-claude-design-ai-powered-visual-suite)

**OpenClaw controversy**
- [Anthropic temporarily banned OpenClaw's creator (TechCrunch)](https://techcrunch.com/2026/04/10/anthropic-temporarily-banned-openclaws-creator-from-accessing-claude/)
- [Anthropic blocks OpenClaw from Claude subscriptions (TNW)](https://thenextweb.com/news/anthropic-openclaw-claude-subscription-ban-cost)
- [Anthropic: You can still use Claude accounts to run OpenClaw (The New Stack)](https://thenewstack.io/anthropic-agent-sdk-confusion/)
- [The OpenClaw Ban That Exposed Anthropic's Real Problem (Medium)](https://medium.com/@stawils/the-openclaw-ban-that-exposed-anthropics-real-problem-fe8f10aa0e80)

**Government / policy**
- [Trump says Anthropic deal with DoD is "possible" (CNBC)](https://www.cnbc.com/2026/04/21/trump-anthropic-department-defense-deal.html)
- [Hegseth gives Anthropic until Friday (Hacker News — unverified)](https://news.ycombinator.com/item?id=47142587)

**Research & programs**
- [Anthropic Fellows Program 2026 (Opportunity Desk)](https://opportunitydesk.org/2026/04/15/anthropic-fellows-program-2026/)
- [Hacker News front page (April 20, 2026)](https://news.ycombinator.com/front)
