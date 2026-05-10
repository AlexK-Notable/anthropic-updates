# Anthropic Daily Update — 2026-04-24

Compiled from first-party Anthropic sources, the Claude Code changelog, Hacker News discussion, and tech press. Items are grouped by category; each carries inline citations to the underlying source so you can verify directly.

---

## 1. Today's headline items (Apr 23–24)

**Claude Code quality postmortem published.** Anthropic released a public investigation acknowledging that three distinct changes between March and April caused user-visible degradation in Claude Code, the Agent SDK, and Claude Cowork. The three root causes:
- **Mar 4:** default reasoning effort was silently lowered from `high` to `medium` to manage latency.
- **Caching bug:** thinking history was being cleared every turn, producing "forgetful and repetitive" behavior.
- **Apr 16:** a system prompt change intended to reduce verbosity inadvertently degraded coding quality.

All three were rolled back / fixed; usage limits were also reset for affected subscribers. ([The Register](https://www.theregister.com/2026/04/23/anthropic_says_it_has_fixed/), [TechBriefly](https://techbriefly.com/2026/04/24/anthropic-resolves-claude-code-bugs-after-nerfing-allegations/))

**NEC named first Japan-based global partner (Apr 24).** NEC will roll Claude out to ~30,000 employees worldwide as the foundation of an AI-native engineering org. ([Anthropic](https://www.anthropic.com/news/anthropic-nec))

**Claude Code 2.1.118 shipped (Apr 23).** Highlights: vim visual (`v`) and visual-line (`V`) modes with operators, `/cost` + `/stats` merged into `/usage` (typing shortcuts preserved), named custom themes via `/theme`, hooks can now invoke MCP tools directly via `type: "mcp_tool"`, new `DISABLE_UPDATES` env var, WSL inherits Windows-side managed settings, and `autoMode` rules can extend the built-in list via `"$defaults"` instead of replacing it. ([claude-code releases](https://github.com/anthropics/claude-code/releases), [Claude Code Docs changelog](https://code.claude.com/docs/en/changelog))

---

## 2. Models & frontier capability

**Claude Opus 4.7 — GA on Apr 16.** Material gains on the hardest software-engineering tasks vs. Opus 4.6, plus higher-resolution vision and an updated tokenizer. An `xhigh` effort tier was added between `high` and `max`; `/effort` now opens an interactive slider when invoked without arguments. ([Anthropic — Introducing Claude Opus 4.7](https://www.anthropic.com/news/claude-opus-4-7))

**Claude Mythos Preview (Apr 7).** Unreleased frontier model with disproportionately strong cyber-offense capability. Anthropic's framing: it can surpass all but the most skilled humans at finding and exploiting software vulnerabilities. Not generally available — see Project Glasswing below. ([red.anthropic.com — Mythos Preview](https://red.anthropic.com/2026/mythos-preview/), [Foreign Policy analysis](https://foreignpolicy.com/2026/04/20/claude-mythos-preview-anthropic-project-glasswing-cybersecurity-ai-hacking-danger/))

---

## 3. Project Glasswing (cybersecurity)

Anthropic withheld broad release of Mythos Preview and instead launched **Project Glasswing**, a controlled-access program for finding and fixing vulnerabilities in critical software. Confirmed initial partners: AWS, Apple, Broadcom, Cisco, CrowdStrike, Google, JPMorgan Chase, the Linux Foundation, Microsoft, NVIDIA, Palo Alto Networks. All findings go through coordinated disclosure; partners are limited to using the model on their own software or open-source projects they maintain. ([Anthropic — Project Glasswing](https://www.anthropic.com/glasswing))

**Reported impact so far:**
- Thousands of zero-day vulnerabilities discovered across every major OS and browser, oldest a 27-year-old OpenBSD bug. ([The Hacker News](https://thehackernews.com/2026/04/anthropics-claude-mythos-finds.html))
- Mozilla shipped Firefox 150 patching 271 vulnerabilities surfaced in a single Mythos evaluation pass. ([TheNextWeb](https://thenextweb.com/news/mozilla-firefox-claude-mythos-271-vulnerabilities))
- ETH Zürich and CETaS (Turing Institute) published commentary warning the same capability lowers the bar for offensive use. ([ETH Zürich](https://ethz.ch/en/news-and-events/eth-news/news/2026/04/with-claude-mythos-a-single-hacker-suddenly-has-a-lot-more-ways-to-attack.html), [CETaS](https://cetas.turing.ac.uk/publications/claude-mythos-future-cybersecurity))

---

## 4. Claude Code feature drops earlier in April

Pulled from the official changelog and Anthropic Help Center release notes; not exhaustive — version-by-version detail in the linked changelog.

- **Ultraplan (early preview).** Draft a plan in the cloud from the CLI, review/comment in a web editor, then run remotely or pull back local. First run auto-creates a cloud environment.
- **Monitor tool.** Streams background events into the conversation so the model can tail logs and react live.
- **`/team-onboarding`.** Generates a teammate ramp-up guide from your local Claude Code usage.
- **Prompt cache TTL controls.** `ENABLE_PROMPT_CACHING_1H` (opt into 1h TTL on API key, Bedrock, Vertex, Foundry) and `FORCE_PROMPT_CACHING_5M`.
- **Session recap.** New recap surfaces context when returning to a session; configurable in `/config`, manually via `/recap`.
- **Built-in slash command discovery via Skill tool.** The model can now find/invoke `/init`, `/review`, `/security-review`, etc. through Skill.
- **Reliability fixes.** Stdio MCP servers no longer disconnect on the first stray non-JSON line (regression in 2.1.105); headless/SDK auto-title no longer fires an extra Haiku request; piped output with a single very wide line no longer balloons memory.

Sources: [Claude Code Docs — What's new](https://code.claude.com/docs/en/whats-new), [Claude Code Docs — Changelog](https://code.claude.com/docs/en/changelog), [claudefa.st changelog roll-up](https://claudefa.st/blog/guide/changelog), [Releasebot — Claude Code](https://releasebot.io/updates/anthropic/claude-code).

---

## 5. Pricing / subscription drama

**Claude Code on the $20 Pro plan — confused signal.** On Apr 21–22, the public pricing page appeared to remove Claude Code from Pro, with Max 5x ($100/mo) becoming the cheapest entry. Within hours, Anthropic's head of growth clarified it was a test against ~2% of new prosumer signups; existing Pro/Max users were unaffected, and the Pro column has since had the Claude Code checkbox restored. Simon Willison's writeup is the cleanest summary of the chronology. ([Simon Willison](https://simonwillison.net/2026/Apr/22/claude-code-confusion/), [The Register](https://www.theregister.com/2026/04/22/anthropic_removes_claude_code_pro/), [Where's Your Ed At](https://www.wheresyoured.at/news-anthropic-removes-pro-cc/))

**Third-party harness access.** Earlier in April, Anthropic moved to disallow Claude subscription credits being used through third-party harnesses such as OpenClaw, then partially reversed for CLI-style usage after pushback. Live HN threads tracked the back-and-forth. ([HN: Anthropic no longer allowing Claude Code subs to use OpenClaw](https://news.ycombinator.com/item?id=47633396), [HN: OpenClaw-style usage allowed again](https://news.ycombinator.com/item?id=47844269))

**Ad-free commitment restated.** Anthropic published a piece arguing advertising incentives are incompatible with a genuinely helpful assistant; framed as a guardrail for trust as access expands. ([Anthropic news index](https://www.anthropic.com/news))

---

## 6. Research

- **"Emotion Concepts and their Function in a Large Language Model" (Apr 2).** Investigates internal emotion representations in Sonnet 4.5; finds those representations causally influence outputs — including preferences and rates of misaligned behavior such as reward hacking, blackmail, and sycophancy. ([transformer-circuits.pub](https://transformer-circuits.pub/2026/emotions/index.html))
- **"Trustworthy Agents in Practice" (Apr 9).** Decomposes agent behavior into model / harness / tools / environment; argues model-level safety alone is insufficient. ([Cequence summary](https://www.cequence.ai/blog/ai/anthropic-agent-security-framework-infrastructure-governance/), [Anthropic research index](https://www.anthropic.com/research))
- **"Automated Alignment Researchers" (Apr 14).** Autonomous agents propose ideas, run experiments, and iterate on the weak-to-strong supervision problem; reportedly outperform human researchers on the same task. ([Anthropic research index](https://www.anthropic.com/research))
- **Anthropic Fellows Program 2026** applications open through Apr 26 (stipend available). ([Opportunity Desk](https://opportunitydesk.org/2026/04/15/anthropic-fellows-program-2026/))

---

## 7. Business / infrastructure

- **Amazon expansion.** New agreement secures up to 5 GW of capacity for training and serving Claude; nearly 1 GW total of Trainium2 + Trainium3 by end of 2026. Run-rate revenue cited at $30B (vs. ~$9B at end of 2025). ([Anthropic](https://www.anthropic.com/news/anthropic-amazon-compute))
- **Google investment.** Alphabet plans up to $40B into Anthropic — $10B upfront, $30B contingent on milestones. ([MEXC News summary](https://www.mexc.com/news/1052143))
- **Google + Broadcom partnership expanded** for additional compute. ([Anthropic](https://www.anthropic.com/news/google-broadcom-partnership-compute))
- **$50B U.S. AI infrastructure commitment.** ([Anthropic](https://www.anthropic.com/news/anthropic-invests-50-billion-in-american-ai-infrastructure))
- **Board.** Long-Term Benefit Trust appointed Vas Narasimhan to the Anthropic board on Apr 14. ([Anthropic news index](https://www.anthropic.com/news))

---

## 8. Followup: Claude Code source-code leak (Mar 31, still relevant)

Closing the loop on a late-March incident that's still being analyzed: version 2.1.88 of `@anthropic-ai/claude-code` shipped a 59.8 MB source map (`.map`) that exposed ~2,000 TypeScript files / >512K LOC. Root cause was Bun generating a full source map by default plus `*.map` not being excluded in `.npmignore` / `package.json` `files`. Anthropic confirmed no customer data or credentials were exposed. A separate, contemporaneous Axios supply-chain compromise (Mar 31, 00:21–03:29 UTC) is the more material risk for anyone who installed/updated in that window. ([The Hacker News](https://thehackernews.com/2026/04/claude-code-tleaked-via-npm-packaging.html), [VentureBeat](https://venturebeat.com/technology/claude-codes-source-code-appears-to-have-leaked-heres-what-we-know), [BleepingComputer](https://www.bleepingcomputer.com/news/artificial-intelligence/claude-code-source-code-accidentally-leaked-in-npm-package/), [InfoQ](https://www.infoq.com/news/2026/04/claude-code-source-leak/), [The Register](https://www.theregister.com/2026/03/31/anthropic_claude_code_source_code/))

---

## What I'd flag for an AI professional today

1. **Mythos / Glasswing is the dominant strategic story.** Capability/release decoupling is a new precedent — the capability exists publicly, but only a partner consortium has access. Worth tracking how that consortium expands and how non-partners react.
2. **Opus 4.7 + `xhigh` effort** is the practical upgrade for anyone using Claude Code seriously today.
3. **The quality postmortem** is rare on-the-record acknowledgment that "model degradation" complaints had real causes — reasoning effort, caching, system prompt verbosity. Useful citation when defending similar regressions in your own deployments.
4. **Pricing test on Pro** signals Anthropic is actively probing how much Claude Code is worth standalone. Expect the $100 Max-tier as the de facto floor for heavy CLI users to come back as a question.

---

## Methodology / source notes

Trusted-source priority order used here: anthropic.com, code.claude.com docs, github.com/anthropics/claude-code, transformer-circuits.pub, alignment.anthropic.com, red.anthropic.com → established tech press (The Register, VentureBeat, BleepingComputer, InfoQ, TechCrunch, Hacker News threads, Simon Willison) → secondary aggregators (Releasebot, claudefa.st, claudelog) only to surface items I then verified against a primary source. No citation in this report is a single secondary aggregator.
