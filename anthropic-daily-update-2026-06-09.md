# Anthropic daily briefing — 2026-06-09

_Briefing window: 2026-06-08 11:05 UTC → 2026-06-09 11:05 UTC (~24 hours). Audience: applied AI engineer working in the Anthropic ecosystem._

Prior briefing: `anthropic-daily-update-2026-06-08.md` (window end 2026-06-08 11:05 UTC). No skipped runs.

## Headline

Two first-party items dominate the window. (1) **Claude Code v2.1.169 — 2026-06-08 21:57 UTC** ([release](https://github.com/anthropics/claude-code/releases/tag/v2.1.169)) ships `--safe-mode` (start with all CLAUDE.md / plugins / skills / hooks / MCP servers disabled, via flag or `CLAUDE_CODE_SAFE_MODE`), a new `/cd` command that switches the session's working directory without breaking the prompt cache, a `disableBundledSkills` setting / `CLAUDE_CODE_DISABLE_BUNDLED_SKILLS` env var, a security fix for managed `allowedMcpServers`/`deniedMcpServers` policies that were not being enforced on reconnect / IDE-typed configs / `--mcp-config` / first-session-after-install, restoration of a default 5-minute idle timeout on Vertex/Foundry (opt out with `API_FORCE_IDLE_TIMEOUT=0`), and another security fix preventing untrusted project settings from setting OTEL client-certificate paths without trust confirmation. Lockstep [`claude-agent-sdk-typescript` v0.3.169](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.169) (same minute) adds an experimental `usage_EXPERIMENTAL_MAY_CHANGE_DO_NOT_RELY_ON_THIS_API_YET()` method on `Query` returning structured session cost, plan rate-limit, and local usage-behaviors data, plus an `sse` option in `BrowserQueryOptions` as a WebSocket alternative. (2) **Anthropic Red published [Measuring LLMs' impact on N-day exploits](https://red.anthropic.com/2026/n-days/)** on **2026-06-08** — Mythos Preview autonomously built **8 working code-execution exploits across 18 recent Firefox SpiderMonkey CVEs** (~12 hours total, first exploit in under an hour after Mozilla's patch landed — 18 days before stable Firefox 148 shipped) and **8 full local-EoP chains on 21 Windows kernel CVEs** (six-hour wall clock, ~$2,200 in PoCs and ~$15,700 in chains, ≈$2k per privilege escalation), explicitly framed as the end of the "N-day = weeks" assumption. **No new model in window.** **No new entries in the Claude support release notes** ([latest still Jun 2](https://support.claude.com/en/articles/12138966-release-notes)). **Five minor status incidents** in window. **HN:** Bruce Schneier's Project Glasswing post hit 39 points and 404 Media's Microsoft-supply-chain-malware-against-Claude-and-Gemini story hit 18 points (Jun 8).

---

## Claude Code

**One release in window.**

**v2.1.169 — 2026-06-08 21:57:18 UTC** ([release](https://github.com/anthropics/claude-code/releases/tag/v2.1.169)). The first 2.1.169 since the v2.1.163–168 burst already covered in the prior briefing. Practitioner-relevant items:

- **`--safe-mode` flag and `CLAUDE_CODE_SAFE_MODE` env var** — start Claude Code with _all_ customizations (CLAUDE.md, plugins, skills, hooks, MCP servers) disabled. For debugging "is it me or the harness?" without yak-shaving config.
- **`/cd` command** — switch the session's working directory mid-session without breaking the prompt cache. This previously required a restart, throwing away cache and history.
- **`disableBundledSkills` setting and `CLAUDE_CODE_DISABLE_BUNDLED_SKILLS` env var** — hide bundled skills, workflows, and built-in slash commands from the model. Useful when you've shipped a project-specific replacement and want to suppress the default and avoid token bloat in the system prompt.
- **Enterprise security: managed `allowedMcpServers` / `deniedMcpServers` now enforced on reconnect, on IDE-typed configs, on `--mcp-config` servers during the first session after install, and before remote settings have loaded.** Also fixes slow cold starts for orgs without remote settings. _If your org has a managed MCP allowlist, anything before 2.1.169 had real bypass paths._
- **Untrusted project settings can no longer set OTEL client-certificate paths without trust confirmation.** Closes a path to local cert exfiltration via a hostile repo.
- **Default 5-minute idle timeout restored on Vertex / Foundry** so a stalled stream aborts instead of hanging indefinitely. Opt out with `API_FORCE_IDLE_TIMEOUT=0`. _If you noticed Vertex/Foundry jobs hanging on long generations after a recent update, this is the fix and the new lever._
- **`claude agents --json` now emits `id` and `state` fields, plus `--all` to include completed sessions** (previously omitted blocked and just-dispatched background sessions). Background sessions now also preserve `--ide`, `--chrome`, `--bare`, `--remote-control`, and other flags across retire→wake; respawn state validation hardened.
- **macOS perf: ~30–50 ms per-turn UI stall at turn start fixed** for users logged in with claude.ai credentials. Reduced CPU during streaming and spinner animations.
- **`claude -p` Windows hang on slash-command/skill scan fixed** (regression in 2.1.161).
- Remote-managed settings with one invalid entry now apply the remaining valid policies (was silently dropping the whole payload). Org admin foot-gun gone.
- Up/Down keys in long wrapped inputs now traverse visual rows before jumping into history; history recall enters at the nearest edge.
- Background agents now honor project-level settings `env` values (e.g., `ANTHROPIC_MODEL`) when dispatched onto a pre-warmed worker.
- Plugin `.in_use` PID lock files were accumulating without bound; stale markers from crashed sessions are now swept once per day. Long-running Cowork/Code installs were leaking these.
- MCPB plugin cache no longer spuriously invalidated on Windows.
- Remote Control no longer stuck on "reconnecting" when an OAuth token refresh races a session resume.
- `/workflows` opens immediately even mid-turn.
- `TaskCreate`: malformed inputs auto-repaired; validation errors for unloaded tools now include the schema. Should reduce the very common "tool not found" abandonment on agent runs.
- "CLAUDE.md is too long" warning threshold scales with the model's context window — relevant if you're on Opus 4.8 with a much bigger window than Sonnet.
- Auto-updater on Windows stops retrying within a session once `claude.exe` is held by another process. Less log noise on EDR-locked machines.

**Status incidents in window** ([status.claude.com](https://status.claude.com/)) — five, all minor:

- **2026-06-08 17:15–19:22 UTC — Opus 4.7 elevated errors.** 2 h 7 m. Largest reliability event of window. claude.ai, API, Code, Cowork. [stspg.io/qhh9nm2syrzr](https://stspg.io/qhh9nm2syrzr)
- **2026-06-08 19:10–19:52 UTC — Haiku 4.5 elevated errors.** 42 m. Same four surfaces. [stspg.io/1yggzs0zxvn4](https://stspg.io/1yggzs0zxvn4)
- **2026-06-08 21:05–21:56 UTC — Opus 4.7 elevated errors.** 51 m. Second Opus 4.7 incident in 6 h. [stspg.io/9ph775tb1mgl](https://stspg.io/9ph775tb1mgl)
- **2026-06-08 22:20–22:36 UTC — Opus 4.8 elevated errors.** 16 m. [stspg.io/b1ym53l7ntdq](https://stspg.io/b1ym53l7ntdq)
- **2026-06-09 03:13–03:42 UTC — Opus 4.8 (Fast Mode) elevated errors.** 29 m. Second Opus 4.8 incident in 5 h. [stspg.io/pck33z3r9h36](https://stspg.io/pck33z3r9h36)

_Practitioner note:_ Opus 4.7, 4.8, and Haiku 4.5 all hit elevated-error windows on the same UTC day. None were major, but production Opus consumers should have `fallbackModel` configured (shipped in v2.1.166).

---

## API & Models

**No new model in window.** Opus 4.8 remains newest first-party; Mythos Preview remains gated to Project Glasswing partners.

**No SDK pricing changes.** **No API spec changes** beyond what `anthropic-sdk-python` v0.107.1 (Jun 7, outside window) shipped.

**`anthropic-sdk-python` and `anthropic-sdk-typescript`** — no in-window releases. Most recent: python v0.107.1 (Jun 7, prior briefing), typescript sdk-v0.102.0 (Jun 6, prior briefing).

---

## SDKs

**`claude-agent-sdk-typescript` [v0.3.169 — 2026-06-08 21:57:15 UTC](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.169).** Two additions:

- **`usage_EXPERIMENTAL_MAY_CHANGE_DO_NOT_RELY_ON_THIS_API_YET()` method on `Query`** — returns structured **session cost, plan rate-limit, and local usage-behaviors data**. _First programmatic hook for reading session cost and plan rate-limit usage from inside the SDK._ Method name signals the API is explicitly unstable; do not pin a dashboard on it yet but it's the long-requested cost-introspection surface.
- **`sse` option (`SSEOptions`) on `BrowserQueryOptions`** as an alternative to WebSockets. Useful for browser-SDK consumers behind proxies or in environments where WebSocket upgrade is hostile (corporate firewalls, some serverless edge runtimes).

**`claude-agent-sdk-python` [v0.2.94 — 2026-06-08 22:10:16 UTC](https://github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.94).** _Internal/Other changes only_ — bundled Claude CLI bumped to 2.1.169. No public surface change. _Python parity with the experimental `usage()` method is not in this release._

---

## Research

**Two first-party publications in window.** Both substantive.

**[Measuring LLMs' impact on N-day exploits](https://red.anthropic.com/2026/n-days/)** — Anthropic Red (Winnie Xiao, Tim Abbott, Nicholas Carlini, Newton Cheng, David Forsythe, Keane Lucas, Milad Nasr, Shikhar Sakhuja), dated **June 8, 2026** explicitly on the page. Concrete numbers:

- **Firefox SpiderMonkey N-days:** 18 patches from Firefox 148 / 149 (released Feb 24 / Mar 24); 90+ days of public history at evaluation. Agent runs in a Linux container, no internet, gets the public diff (regression test stripped), component, severity, and ASan-instrumented vulnerable + patched `jsshell` builds. **PoC counts (3 trials/CVE):** Opus 4.5 → 2 PoCs; Opus 4.8 → 11; **Mythos Preview → 14**. **Mythos Preview's first PoC in ~12 min, 13/14 within 40 min, total ~3 h for all 14.** Consistency (50 trials each on 18 CVEs): **Mythos Preview hit 7 vulns on all 50 trials; Opus 4.8 and 4.6 hit one vuln each that consistently.** Working exploit (reads a randomized secret only on the vulnerable build): Sonnet 4.6 → 1; Opus 4.6 → 1; **Opus 4.8 → 2; Mythos Preview → 8.** **Mythos Preview's first working exploit in <1 h, all 8 in ~12 h — 18 days before stable Firefox 148 shipped the fix.**
- **Windows kernel N-days:** 21 LPE patches from Jan–Feb 2026 (all after every tested model's knowledge cutoff). Agent gets vulnerable + patched binaries, public debug symbols, Ghidra decomp of vulnerable build, Ghidriff function diff, public MSRC advisory; runs against a live Windows Server 2025 VM as `lowpriv`. **PoCs (3 trials/CVE):** Sonnet 4.6 → 13; Opus 4.7 → 13; Opus 4.8 → 15; **Mythos Preview → 18 (all 18 PoCs in 6 h, ~$2,200 API).** **Full `lowpriv → SYSTEM` chains: only Mythos Preview produced any — 8 chains, ~$15,700 API (≈$2k each).** Opus 4.8 got arbitrary read, arbitrary write, and a KASLR leak in several trials, but did not chain.
- **Calibration claim Microsoft will not like:** Mythos Preview produced PoCs for 13 of the 14 vulns Microsoft rated "Exploitation Less Likely" / "Exploitation Unlikely," including a full privilege escalation for one rated "Exploitation Unlikely."
- **Patch-cadence takeaway:** With Windows Autopatch reaching 90% of enrolled devices on day 7 and forced reboot on day 11, Mythos Preview finishes all eight chain exploits before patches reach the fleet.

_Why it matters to a practitioner:_ If you ship product on a monthly patch cadence with multi-week staged rollouts — or you run a CI/CD pipeline that pulls dependencies on a similar lag — your threat model is now obsolete. The post's framing ("N-hour, not N-day") is the recommended elevator pitch for getting cadence shortened internally. Also useful evidence for anyone arguing for memory-safe rewrites of hot paths.

**[Paving the way for agents in biology](https://www.anthropic.com/research/agents-in-biology)** — by Laura Luebbert, based on research by Nasri, Gurev, Varilly, Ramesh, O'Leary, Cool, Renard, Sabeti, Luebbert ([arXiv preprint 2606.06749](https://arxiv.org/pdf/2606.06749)). Dated **Jun 8, 2026** on the page. The argument: agent unreliability on scientific data tasks is often an _infrastructure_ problem, not a reasoning one. Concrete benchmark (VirBench: 120 viral-sequence-retrieval queries across 40 pathogens, ground truth manually verified): without a deterministic retrieval tool, **agent mean accuracy ranged 16.9% → 91.3%** across Claude Sonnet 4, Opus 4.7, Biomni OSS, Edison Analysis, GPT-5.2-pro, GPT-5.5; even the strongest were not run-to-run reproducible (one example: Sonnet 4 returned 106 / 15 / 5 sequences on three identical-prompt runs against an expected 266). **Adding `gget virus` (a deterministic NCBI-Virus retrieval layer they built in collaboration with NCBI) pushed every agent above 90%, with GPT-5.5 at 99.7%, and effectively eliminated run-to-run variance.** Notable footnote 6: "Claude Sonnet 4 represents the latest publicly available Anthropic model that can be used for this evaluation, due to subsequent biosafety-related access restrictions on newer models." _If you're building agentic pipelines over messy external data, the headline practitioner takeaway is: deterministic retrieval tools collapse the model-choice axis — cheaper models with the right harness beat expensive models without one, and remove the run-to-run reproducibility problem for free._

---

## Other first-party content

**[Building intelligent apps for Apple platform with Claude in the Foundation Model](https://claude.com/blog/claude-for-foundation-models)** — claude.com blog, surfaced on HN at 2026-06-08 22:04 UTC (publication date on the post itself not surfaced as a numeric field, but HN timestamp falls in window). Walkthrough of using Claude inside Apple's Foundation Models framework — relevant if you ship iOS/macOS apps and want first-party-style on-device + cloud-Claude fallback. Worth a skim if Apple platforms are in your roadmap.

**[How Anthropic enables self-service data analytics with Claude](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude)** — claude.com blog, HN-posted 2026-06-08 13:09 UTC. Internal-data-team case study. Engineering-blog tier, not research; useful pattern reference if you're building an internal Claude-over-warehouse setup.

---

## Hacker News

Front-page-ish items in window (≥10 points):

- **[Anthropic's Project Glasswing Update](https://www.schneier.com/blog/archives/2026/06/anthropics-project-glasswing-update.html) — Schneier blog — 39 pts, 5 comments — 2026-06-08 12:33 UTC.** Schneier's commentary on the [Project Glasswing initial update](https://www.anthropic.com/research/glasswing-initial-update) (Anthropic post is from May 22, _outside_ window, but the discussion is in window). Worth reading if you've been tracking the Glasswing partner program (Mythos Preview gating).
- **[Microsoft Hacked to Deliver Malware to Claude and Gemini Users](https://www.404media.co/microsoft-hacked-to-deliver-malware-to-claude-and-gemini-users/) — 404 Media — 18 pts, 0 comments — 2026-06-08 18:34 UTC.** Supply-chain story claiming Microsoft was compromised in a way that delivered malware to Claude and Gemini end users. _I have not independently verified the underlying claim._ See also [Claude Dynamic Workflows Inaccurate Permissions Docs](https://www.promptarmor.com/resources/claude-dynamic-workflows-use-incorrect-permissions) (PromptArmor, 2026-06-08 17:42 UTC), which is unrelated but in the same security-flavored cluster.
- **[Show HN: Rayline routes Claude Code subagents to on-device and cheaper models](https://rayline.ai/) — 11 pts, 8 comments — 2026-06-08 17:32 UTC.** Third-party tool for routing subagent calls to cheaper / on-device models. Relevant if you're cost-optimizing Claude Code multi-agent workflows; not first-party-endorsed.
- **[Claude Fable 5 by Anthropic, releasing tomorrow](https://news.ycombinator.com/item?id=48450521) — 10 pts, 10 comments — 2026-06-08 19:36 UTC.** _Rumor / leak claim_ — no first-party confirmation. The HN item itself returned an empty page on fetch (likely flagged or removed). Flagging here per scope; treat as unverified.

Lower-vote but flag-worthy:

- **[How to have Claude Code run 28× longer before auto-compaction](https://github.com/wiztek-llc/context-ledger/tree/main) — 3 pts — 2026-06-09 01:30 UTC.** Third-party context-ledger pattern for Claude Code; useful idea, not first-party.
- **[Lean — Two Claude Code skills that stop the AI from over-engineering](https://github.com/albertobarnabo/lean) — 2 pts — 2026-06-08 19:53 UTC.** Possible reusable skills if you find Claude Code over-engineering.

---

## Rumors & Leaks

- **"Claude Fable 5 releasing tomorrow"** — HN claim only, no Anthropic confirmation, original HN item appears flagged. If true, would imply a Fable model line (otherwise unconfirmed in any first-party surface). **Treat as unverified speculation pending a first-party post on [anthropic.com/news](https://www.anthropic.com/news).**

---

## Excluded / promising but out of window

- **[Making Claude a chemist](https://www.anthropic.com/research/making-claude-a-chemist)** — Anthropic research post, dated **Jun 5, 2026** on the page itself. Surfaced on HN on Jun 8 (3 pts) but the underlying publication is outside the window. _Excluded by the recency-filter rule; was covered in the Jun 8 briefing's pre-window context._
- **`anthropic-sdk-python` v0.107.1 (Foundry `x-api-key` fix)** and **`anthropic-sdk-typescript` sdk-v0.102.0 (middleware ordering fix)** — covered in prior briefing.
- HN Algolia index appeared partially incomplete on first pass — items above Jun 8 ~12:00 UTC were initially missed by a date-range query and only surfaced via a second numeric-filter query. **If you find a Jun 8/9 HN item not listed here, it may be an indexing gap rather than a true absence.**

---

Sources used (all in window unless noted):

- [github.com/anthropics/claude-code/releases/tag/v2.1.169](https://github.com/anthropics/claude-code/releases/tag/v2.1.169) — published 2026-06-08T21:57:18Z
- [github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.169](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.169) — published 2026-06-08T21:57:15Z
- [github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.94](https://github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.94) — published 2026-06-08T22:10:16Z
- [red.anthropic.com/2026/n-days/](https://red.anthropic.com/2026/n-days/) — dated 2026-06-08
- [anthropic.com/research/agents-in-biology](https://www.anthropic.com/research/agents-in-biology) — dated 2026-06-08
- [claude.com/blog/claude-for-foundation-models](https://claude.com/blog/claude-for-foundation-models) — HN-timestamp 2026-06-08 22:04 UTC (publication timestamp not surfaced)
- [claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude](https://claude.com/blog/how-anthropic-enables-self-service-data-analytics-with-claude) — HN-timestamp 2026-06-08 13:09 UTC
- [status.claude.com/api/v2/incidents.json](https://status.claude.com/) — 5 in-window incidents
- [support.claude.com/en/articles/12138966-release-notes](https://support.claude.com/en/articles/12138966-release-notes) — no in-window entries (latest still Jun 2)
- HN Algolia API: in-window items above
