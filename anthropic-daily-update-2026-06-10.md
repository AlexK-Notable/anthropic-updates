# Anthropic daily briefing — 2026-06-10

_Briefing window: 2026-06-09 11:05 UTC → 2026-06-10 11:06 UTC (~24 hours). Audience: applied AI engineer working in the Anthropic ecosystem._

Prior briefing: `anthropic-daily-update-2026-06-09.md` (window end 2026-06-09 11:05 UTC). No skipped runs.

## Headline

**Claude Fable 5 / Mythos 5 launched** — 2026-06-09 ≈16:58 UTC ([anthropic.com/news](https://www.anthropic.com/news/claude-fable-5-mythos-5), [system card PDF](https://www-cdn.anthropic.com/d00db56fa754a1b115b6dd7cb2e3c342ee809620.pdf), [model page](https://www.anthropic.com/claude/fable)). Fable 5 is the new top-tier broadly-available model (Mythos-class, with safety classifiers for cyber / bio-chem / distillation that route flagged prompts to an Opus 4.8 fallback in <5% of sessions). Mythos 5 is the same underlying model with cyber safeguards lifted, gated to Project Glasswing partners (bio/chem safeguards to be lifted for select trusted-biology researchers). **Pricing: $10/MTok input, $50/MTok output** for both — "less than half the price of Claude Mythos Preview." Fable 5 is on Pro/Max/Team/seat-based Enterprise free through Jun 22 (moves to usage credits Jun 23), and on Claude API + consumption-based Enterprise immediately as `claude-fable-5`. Capability claims (unverified by us, sourced from launch post): state-of-the-art on "nearly all tested benchmarks," top score on Cognition's FrontierCode at medium effort, top score on Hebbia Finance Benchmark, beat Pokémon FireRed with a vision-only harness, Stripe completed a 50M-line Ruby migration in a day that would have taken "over two months by hand," and "misalignment level similar to that of Opus 4.8."

Lockstep tooling: **Claude Code v2.1.170** ([release](https://github.com/anthropics/claude-code/releases/tag/v2.1.170), 17:23 UTC) enables Fable 5 in the model picker. **`anthropic-sdk-python` v0.108.0/0.109.0/0.109.1** and the **TypeScript SDK monorepo** (sdk + bedrock + vertex + foundry + aws, with sdk landing v0.103.0 → v0.104.1) propagated through the day. Two new SDK features beyond model enablement: **server-side fallbacks on refusal + a client-side fallbacks middleware**, and **Managed Agents deployment** support (env-var credentials). A new refusal category `frontier_llm` shipped in the day's final patch.

Secondary first-party: **[Claude Managed Agents](https://claude.com/blog/whats-new-in-claude-managed-agents)** added **scheduled deployments** (cron-fire new sessions, no scheduler to host) and **environment variables in vaults** with network-boundary key injection — the agent only sees a placeholder, real key attaches only on requests to allowlisted domains; Browserbase + KERNEL CLIs unlock first-time browser capabilities for Managed Agents.

Policy: a new **30-day retention requirement for Mythos-class models** ([support article](https://support.claude.com/en/articles/15425996-data-retention-practices-for-mythos-class-models)) applies to ZDR workspaces in Claude Console, ZDR Claude Code in Enterprise, and ZDR access via AWS Bedrock / Google Cloud Agent Platform / Microsoft Foundry. **No opt-out for Mythos-class** on those surfaces; consumer plans and standard Enterprise are unaffected.

Two in-window status incidents — one **major** (Fable missing from Claude Code Desktop model picker before v2.1.170) and one **minor** (Opus 4.6 elevated errors). HN: the Fable 5 launch thread is one of the largest Anthropic threads in months (2,276 points, 1,766 comments). **No new research publications in window** (anthropic.com/research, red.anthropic.com, alignment.anthropic.com all unchanged).

---

## Claude Code

**One release in window.**

**v2.1.170 — 2026-06-09 17:23:09 UTC** ([release](https://github.com/anthropics/claude-code/releases/tag/v2.1.170), commit 6a9c2db, by ashwin-ant). Only two changelog bullets — a thin release whose purpose is Fable 5 enablement:

- **Claude Fable 5 access** — "Update to version 2.1.170 for access." Older Claude Code Desktop installs silently miss the option in the model picker (this is what caused the major status incident below).
- **VS Code integrated terminal transcript fix** — sessions launched from the VS Code integrated terminal (or any shell inheriting Claude Code environment variables) were not saving transcripts and were absent from `--resume`. Fixed. _If you've recently noticed missing past sessions in VS Code-launched runs, this is the cause; transcripts created before 2.1.170 are not recoverable._

Everything else (`--safe-mode`, `/cd`, `disableBundledSkills`, managed-MCP allowlist enforcement, OTEL cert-path trust check, Vertex/Foundry idle-timeout restoration, etc.) shipped yesterday in v2.1.169 and was covered in the prior briefing.

**Status incidents in window** ([status.claude.com](https://status.claude.com/)) — two:

- **2026-06-09 18:24–21:03 UTC — Fable not available in the Claude Code Desktop model picker.** Impact: **major**. 2 h 39 m. Resolution requires upgrading to Claude Code v2.1.170. [stspg.io/jb2jyj5prfmm](https://stspg.io/jb2jyj5prfmm)
- **2026-06-09 20:32–22:10 UTC — Opus 4.6 elevated errors.** Impact: **minor**. Affected claude.ai, API, Code, Cowork (all degraded then operational). [stspg.io/l9wbh8vqh3k2](https://stspg.io/l9wbh8vqh3k2)

_Practitioner note:_ The "Fable not in picker" incident classification as **major** is unusual for a release-bound issue — worth noting that Anthropic now treats post-launch model-picker visibility as a major-impact event, presumably because customers expected immediate access on subscription tiers where it shipped free through Jun 22.

---

## API & Models

**Claude Fable 5** — API model id `claude-fable-5`, available immediately on Claude API and consumption-based Enterprise. Pricing **$10/MTok input, $50/MTok output**. The launch post does **not** specify the output-token limit, total context window, or the exact knowledge cutoff; the model page ([anthropic.com/claude/fable](https://www.anthropic.com/claude/fable)) is the place to confirm before pinning a build.

**Claude Mythos 5** — API model id `claude-mythos-5`, same pricing, **Project Glasswing partners only** (cyber safeguards lifted). Anthropic states bio/chem safeguards will subsequently be lifted for select trusted biology researchers; that surface is not yet open.

**Pricing context:** "Less than half the price of Claude Mythos Preview" — a meaningful cost reduction for partners who had been running on Preview, and the first time a Mythos-class price has been listed publicly.

**Subscription rollout:**
- Pro, Max, Team, seat-based Enterprise: Fable 5 **included free through 2026-06-22**.
- **2026-06-23**: subscription Fable 5 moves to usage credits.
- Eventually "restored to standard plans" per the launch post — timeline not specified.
- Standard Enterprise: see the data-retention note below before enabling.

**Safety classifiers:** Three filters (cyber, bio-chem, distillation) route flagged prompts to an Opus 4.8 fallback. Per the launch post: "more than 95% of Fable sessions involve no fallback at all"; fallback triggers in "less than 5% of sessions." _Practitioner implication:_ if you're benchmarking Fable 5 on cyber or bio-adjacent prompts and the response quality dips inexplicably, you may be silently hitting the Opus 4.8 fallback rather than Fable. There is no documented per-response signal for "this was a fallback" yet.

**Misalignment claim:** "Similar to that of Opus 4.8." Worth verifying against the [system card PDF](https://www-cdn.anthropic.com/d00db56fa754a1b115b6dd7cb2e3c342ee809620.pdf) before any production decision predicated on Fable being safer than Opus 4.8.

**New refusal category: `frontier_llm`** — added in `anthropic-sdk-python` v0.109.1 / `anthropic-sdk-typescript` sdk-v0.104.1 (both 23:55 UTC). If your code branches on `stop_reason` or refusal category, add a handler — this is plausibly the category emitted when a Fable session is routed to the Opus 4.8 fallback, though that's not explicitly documented in the SDK changelogs.

---

## SDKs

A full Fable-5-driven cascade through the day. Bumping in lockstep is recommended.

**`anthropic-sdk-python`** — three releases:

- **v0.108.0 — 2026-06-09 16:37:30 UTC** ([release](https://github.com/anthropics/anthropic-sdk-python/releases/tag/v0.108.0)). Feat: `claude-mythos-5` and `claude-fable-5` model support, **server-side fallbacks on refusal**. Feat: **client-side fallbacks middleware** for providers that don't support server-side fallbacks (i.e., Bedrock / Vertex / Foundry first-class consumers can rely on server-side; raw API consumers and intermediaries get the middleware).
- **v0.109.0 — 2026-06-09 20:04:14 UTC** ([release](https://github.com/anthropics/anthropic-sdk-python/releases/tag/v0.109.0)). Feat: **Managed Agents deployments** and environment-variable credentials.
- **v0.109.1 — 2026-06-09 23:55:10 UTC** ([release](https://github.com/anthropics/anthropic-sdk-python/releases/tag/v0.109.1)). Fix: API adds `frontier_llm` refusal category.

**`anthropic-sdk-typescript`** — a 7-release monorepo cascade:

- **sdk-v0.103.0 — 2026-06-09 16:36:30 UTC** ([release](https://github.com/anthropics/anthropic-sdk-typescript/releases/tag/sdk-v0.103.0)). Feat: `claude-mythos-5` + `claude-fable-5`, server-side refusal fallbacks, client-side fallbacks middleware, and **`ctx.logger`** added to middleware (#55). Fix: 3p middleware ordering (#53).
- **bedrock-sdk-v0.30.2** (16:36:53 UTC) — 3p middleware ordering fix. [release](https://github.com/anthropics/anthropic-sdk-typescript/releases/tag/bedrock-sdk-v0.30.2)
- **vertex-sdk-v0.17.1** (16:36:40 UTC) — 3p middleware ordering fix. [release](https://github.com/anthropics/anthropic-sdk-typescript/releases/tag/vertex-sdk-v0.17.1)
- **foundry-sdk-v0.3.1** (16:37:02 UTC) — 3p middleware ordering fix. [release](https://github.com/anthropics/anthropic-sdk-typescript/releases/tag/foundry-sdk-v0.3.1)
- **aws-sdk-v0.4.2** (16:37:10 UTC) — 3p middleware ordering fix. [release](https://github.com/anthropics/anthropic-sdk-typescript/releases/tag/aws-sdk-v0.4.2)
- **sdk-v0.104.0 — 2026-06-09 20:04:21 UTC** ([release](https://github.com/anthropics/anthropic-sdk-typescript/releases/tag/sdk-v0.104.0)). Feat: **Managed Agents deployments** + env-var credentials.
- **sdk-v0.104.1 — 2026-06-09 23:55:23 UTC** ([release](https://github.com/anthropics/anthropic-sdk-typescript/releases/tag/sdk-v0.104.1)). Fix: `frontier_llm` refusal category.

**`claude-agent-sdk-typescript` [v0.3.170 — 2026-06-09 17:23:15 UTC](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.170)** — adds `claude-fable-5` model + `fable` alias. Parity with Claude Code v2.1.170. The experimental `usage_EXPERIMENTAL_MAY_CHANGE_DO_NOT_RELY_ON_THIS_API_YET()` method from v0.3.169 (covered yesterday) is still the relevant lever for cost introspection.

**`claude-agent-sdk-python` [v0.2.95 — 2026-06-09 17:40:09 UTC](https://github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.95)** — internal/other only: bundled Claude CLI bumped to 2.1.170. Python still does not have parity with the experimental `usage()` method.

_Practitioner takeaway:_ If you maintain consumers across both languages, the new middleware feature surface (server-side fallbacks + ctx.logger + Managed Agents deployments) is the biggest unforced cleanup opportunity since the Sonnet 4.6 / Opus 4.7 split — refactor refusal handling now while the API is still warm.

---

## Managed Agents (product surface)

**[New in Claude Managed Agents: run agents on a schedule and store environment variables in vaults](https://claude.com/blog/whats-new-in-claude-managed-agents)** — claude.com blog, 2026-06-09. Both features are **public beta on Claude Platform** today.

**Scheduled deployments.** Attach a cron schedule to an agent; each fire spawns a new session. No scheduler to build or host. Manual pause/resume/archive and on-demand trigger. Customer references: Rakuten, Actively AI, Ando.

**Environment variables in vaults.** Vaults previously held credentials for first-party connectors; now they hold raw env vars consumable by any CLI inside the sandbox. The post is explicit about the security model: **"The agent never sees your key because the sandbox only holds a placeholder. The real key is attached at the network boundary, and only on requests to domains you allow."** Browserbase and KERNEL CLIs are called out as unlocking browser capabilities for Managed Agents for the first time; Notion, Ramp, Sentry CLIs are also supported. Customer references: Notion, Browserbase, Milana.

_Practitioner takeaway:_ Network-boundary key injection is the right shape for an agent secret model — the placeholder pattern eliminates the prompt-injection leak path for any CLI that consumes `$ENV_VAR` rather than printing it. If you've been stuck not Managed-Agents-ing browser flows because of credential exposure, the Browserbase/KERNEL path is now first-class.

---

## Data retention policy change

**[Data retention practices for Mythos-class models](https://support.claude.com/en/articles/15425996-data-retention-practices-for-mythos-class-models)** — support article, effective 2026-06-09. **Mythos-class prompts and outputs are retained 30 days on every platform where these models are offered.** Direct quote: _"we are requiring limited data retention and review as part of our safety work. Prompts submitted to, and outputs generated by, Mythos-class models are retained for 30 days for trust and safety purposes, on every platform where these models are offered."_

**Scope (the meaningful change):**
- ZDR workspaces in Claude Console
- ZDR Claude Code in Claude Enterprise
- Claude via **AWS Bedrock, Google Cloud Agent Platform, or Microsoft Foundry with ZDR**

**No opt-out for Mythos-class on those surfaces** — affected ZDR orgs must enable retention to access covered models. Consumer plans (Free / Pro / Max) and standard Enterprise are unaffected (already retain). Other (non-Mythos) models stay under current terms.

Protections per the article: no employee access unless flagged for serious harm or by written customer request; reviews limited to approved reviewers with no export/copy/download; tamper-proof access logs; auto-deletion at 30 days except for active investigation or legal hold; customer-managed encryption keys and audit logs available.

_Practitioner takeaway:_ If you carry a ZDR contract on Bedrock / GCP Agent Platform / Microsoft Foundry as a procurement or compliance precondition (HIPAA, GDPR data-residency, customer data handling), Fable/Mythos 5 access is a deliberate trade-off, not a default. The companion HN thread "AWS Bedrock to require sharing data with Anthropic for Mythos and future models" (HN [48473166](https://news.ycombinator.com/item?id=48473166), 89 pts) is the customer-side surfacing of this same change.

---

## Research

**No new first-party research publications in window.** [anthropic.com/research](https://www.anthropic.com/research) latest remains the Jun 8 "[Paving the way for agents in biology](https://www.anthropic.com/research/agents-in-biology)" and Jun 5 "[Making Claude a chemist](https://www.anthropic.com/research/making-claude-a-chemist)" (both covered in earlier briefings). [red.anthropic.com](https://red.anthropic.com) latest remains Jun 8 "[Measuring LLMs' impact on N-day exploits](https://red.anthropic.com/2026/n-days/)" (yesterday). [alignment.anthropic.com](https://www.alignment.anthropic.com) latest remains May 2026. The Fable/Mythos 5 system card PDF is the only new research-adjacent first-party artifact in window — see [the PDF](https://www-cdn.anthropic.com/d00db56fa754a1b115b6dd7cb2e3c342ee809620.pdf) for benchmark and misalignment-evaluation details.

---

## Hacker News

Front-page-ish items in window (≥10 points, plus a few notable sub-threshold). All sourced via the HN Algolia API.

- **[Claude Fable 5](https://news.ycombinator.com/item?id=48463808) — 2,276 pts, 1,766 comments — 2026-06-09 16:58:01 UTC.** Launch announcement thread; one of the largest Anthropic-related HN threads in months. URL points to the [Anthropic post](https://www.anthropic.com/news/claude-fable-5-mythos-5).
- **[System Card: Claude Fable 5 and Claude Mythos 5 [pdf]](https://news.ycombinator.com/item?id=48463811) — 211 pts, 1 comment — 2026-06-09 16:58:13 UTC.** Companion thread to the launch; nearly all discussion is in the main thread above.
- **[AWS Bedrock to require sharing data with Anthropic for Mythos and future models](https://news.ycombinator.com/item?id=48473166) — 89 pts, 33+ comments — 2026-06-10 08:21:38 UTC.** Customer-side framing of the ZDR change. The URL field is empty on this HN item; substantive discussion is on HN itself.
- **[Claude Mythos 5 / Fable 5](https://news.ycombinator.com/item?id=48464023) — 17 pts, 2 comments — 2026-06-09 17:10:39 UTC.** Points to the [model page](https://www.anthropic.com/claude/fable).
- **[Anthropic requires 30 day data retention for Fable and Mythos](https://news.ycombinator.com/item?id=48464258) — 7 pts, 0 comments — 2026-06-09 17:23:40 UTC.** Direct link to the [support article](https://support.claude.com/en/articles/15425996-data-retention-practices-for-mythos-class-models).
- **[Claude Fable 5 and Mythos are pure marketing fluff](https://news.ycombinator.com/item?id=48465181) — 5 pts, 0 comments — 2026-06-09 18:16:16 UTC.** [Skeptical third-party take](https://singularitymoments.com/content/claude-fable-5-and-mythos-5-are-pure-marketing-fluff/) — I have not independently verified the post's claims.

Lower-vote but flag-worthy:

- **[Anthropic releases Claude Fable 5 (The Verge)](https://news.ycombinator.com/item?id=48463902) — 4 pts — 2026-06-09 17:04 UTC.** [Verge writeup](https://www.theverge.com/news/946725/anthropic-releases-claude-fable-5-mythos).
- **[Claude Fable 5: the first public Mythos-class model (Artificial Analysis)](https://news.ycombinator.com/item?id=48467906) — 4 pts — 2026-06-09 21:19 UTC.** [Independent benchmark writeup](https://artificialanalysis.ai/articles/claude-fable-5-mythos).
- **[Claude Fable 5 and Mythos 5 pricing](https://news.ycombinator.com/item?id=48465806) — 4 pts, 2 comments — 2026-06-09 18:54 UTC.** [Pricing roundup](https://www.aipricing.guru/news/claude-fable-5-mythos-5-pricing-june-2026/).
- **[One guy reverse-engineered Claude Desktop for Linux via Claude](https://news.ycombinator.com/item?id=48473096) — 4 pts — 2026-06-10 08:12 UTC.** [github.com/aaddrick/claude-desktop-debian](https://github.com/aaddrick/claude-desktop-debian). Unofficial; useful if you want Claude Desktop on Linux today.
- **[macOS menu bar gauges for your Claude Code quota](https://news.ycombinator.com/item?id=48473845) — 3 pts, 1 comment — 2026-06-10 09:43 UTC.** [github.com/grzegorz-raczek-unit8/claude-quota](https://github.com/grzegorz-raczek-unit8/claude-quota). Third-party tool.
- **[Anthropic is intentionally nerfing Fable when asked to develop other LLMs](https://news.ycombinator.com/item?id=48472249) — 3 pts — 2026-06-10 06:27 UTC.** [Reddit r/LocalLLaMA thread](https://old.reddit.com/r/LocalLLaMA/comments/1u1s2oz/anthropic_is_intentionally_nerfing_fable_when/). Likely the "distillation safeguard" classifier in action — see launch post's three-classifier description. Treat as community speculation about a documented behavior.

---

## Rumors & Leaks

The yesterday's "Claude Fable 5 releasing tomorrow" HN claim (item [48450521](https://news.ycombinator.com/item?id=48450521), 10 pts, Jun 8 19:36 UTC) is now **confirmed** by the Jun 9 launch. The leak landed exactly as described.

No new in-window rumors of comparable scope.

---

## Excluded / promising but out of window

- **System card PDF** ([anthropic.com/...PDF](https://www-cdn.anthropic.com/d00db56fa754a1b115b6dd7cb2e3c342ee809620.pdf)) — published in window with the launch but not separately summarized here. Worth a read for benchmark methodology and misalignment-evaluation details before relying on the launch-post claims in production.
- **Artificial Analysis writeup** ([artificialanalysis.ai/articles/claude-fable-5-mythos](https://artificialanalysis.ai/articles/claude-fable-5-mythos)) — third-party in-window benchmark take; not a substitute for the system card but useful as an independent calibration.
- **`anthropic-cookbook`, `courses`, `prompt-eng-interactive-tutorial`, `anthropic-quickstarts`, `dxt`** — no in-window releases.
- No new posts on `anthropic.com/engineering`, `red.anthropic.com`, or `alignment.anthropic.com` in window. If you were expecting a companion alignment post to the Fable 5 launch, it has not appeared yet.

---

Sources used (all in window unless noted):

- [anthropic.com/news/claude-fable-5-mythos-5](https://www.anthropic.com/news/claude-fable-5-mythos-5) — dated 2026-06-09
- [anthropic.com/claude/fable](https://www.anthropic.com/claude/fable) — model landing page, 2026-06-09
- [system card PDF](https://www-cdn.anthropic.com/d00db56fa754a1b115b6dd7cb2e3c342ee809620.pdf) — 2026-06-09
- [claude.com/blog/whats-new-in-claude-managed-agents](https://claude.com/blog/whats-new-in-claude-managed-agents) — 2026-06-09
- [support.claude.com/en/articles/15425996-data-retention-practices-for-mythos-class-models](https://support.claude.com/en/articles/15425996-data-retention-practices-for-mythos-class-models) — 2026-06-09
- [support.claude.com/en/articles/12138966-release-notes](https://support.claude.com/en/articles/12138966-release-notes) — entry dated 2026-06-09
- [github.com/anthropics/claude-code/releases/tag/v2.1.170](https://github.com/anthropics/claude-code/releases/tag/v2.1.170) — 2026-06-09T17:23:09Z
- [github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.170](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.170) — 2026-06-09T17:23:15Z
- [github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.95](https://github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.95) — 2026-06-09T17:40:09Z
- `anthropic-sdk-python` releases v0.108.0, v0.109.0, v0.109.1 — all 2026-06-09
- `anthropic-sdk-typescript` releases sdk-v0.103.0, sdk-v0.104.0, sdk-v0.104.1, bedrock-sdk-v0.30.2, vertex-sdk-v0.17.1, foundry-sdk-v0.3.1, aws-sdk-v0.4.2 — all 2026-06-09
- [status.claude.com](https://status.claude.com/) — 2 in-window incidents (1 major, 1 minor)
- HN Algolia API — items above
