# Anthropic daily briefing — 2026-06-04

_Briefing window: 2026-05-29 11:06 UTC → 2026-06-04 11:06 UTC (~6 days). Audience: applied AI engineer working in the Anthropic ecosystem._

_**Catch-up briefing covering 6 days due to skipped run(s) — items from those days are included.**_ The prior briefing in `/home/komi/notes/anthropic-updates/` is `anthropic-daily-update-2026-05-29.md` (window end 2026-05-29 11:06 UTC); the briefing schedule then went dark until today. The 7-day cap was not hit (window = 6 days exactly).

## Headline

**Three first-party headline events in window.** (1) **Anthropic confidentially submitted a draft Form S-1 to the SEC on Jun 1** ([anthropic.com/news/confidential-draft-s1-sec](https://www.anthropic.com/news/confidential-draft-s1-sec); HN [#48358646](https://news.ycombinator.com/item?id=48358646) at **530 pts / 447 comments**, NYT [#48359035](https://news.ycombinator.com/item?id=48359035) at 26 pts) — a Rule 135 announcement, so no share count or price set yet; legally separates "we can IPO" from "we will." (2) **Project Glasswing expanded to ~150 new partners** in 15+ countries on Jun 2 ([news/expanding-project-glasswing](https://www.anthropic.com/news/expanding-project-glasswing); HN [#48369863](https://news.ycombinator.com/item?id=48369863), **178 pts / 247 comments**). Now covers power, water, healthcare, communications, and hardware; partners are using Mythos Preview both to find vulns and to write patches. Anthropic also explicitly reiterates that the Mythos-class general release is gated on "highly robust safeguards… that we (and, to our knowledge, all other AI developers) have yet to develop" — so the "Mythos in the coming weeks" line from the Opus 4.8 launch should be read as conditional, not committed. (3) **Frontier Red Team published a year-long MITRE ATT&CK mapping** on Jun 3 ([news/AI-enabled-cyber-threats-mitre-attack](https://www.anthropic.com/news/AI-enabled-cyber-threats-mitre-attack)) covering 832 banned accounts; the headline finding is that the share of actors classified medium-risk-or-higher jumped from 33% → 56% across the two halves of the year, and that the most durable risk signal is now whether attackers build scaffolding that lets the model chain attack stages autonomously, not how many ATT&CK techniques they touch. Supporting Claude Code activity: **six Claude Code releases (v2.1.157 → v2.1.162)** shipped in the window with lockstep `claude-agent-sdk-typescript` bumps; the **`workflow` trigger keyword was renamed to `ultracode`** in v2.1.160 (notable if you've tooled around the dynamic-workflows feature from last week); **Auto mode is now opt-in on Bedrock/Vertex/Foundry** for Opus 4.7 and 4.8 (v2.1.158, `CLAUDE_CODE_ENABLE_AUTO_MODE=1`); and **`claude-agent-sdk-python` resumed shipping** (v0.2.88, v0.2.89) after stalling at v0.2.87 since May 23 — TS and Python agent SDKs are now back in lockstep. **No new model in window.** A major Claude Code outage on 2026-06-03 04:17–07:36 UTC is the most consequential reliability event.

---

## Claude Code

**Six releases in window** (one per active day, weekend gap). All via [github.com/anthropics/claude-code/releases](https://github.com/anthropics/claude-code/releases) and the consolidated [CHANGELOG.md](https://raw.githubusercontent.com/anthropics/claude-code/main/CHANGELOG.md). Lockstep `claude-agent-sdk-typescript` versions (v0.3.157 → v0.3.162) shipped alongside; the Python agent SDK rejoined the lockstep at v0.2.88/0.2.89 (covered under API & Models).

**v2.1.157 — 2026-05-29 20:20 UTC** ([release](https://github.com/anthropics/claude-code/releases/tag/v2.1.157)). The largest of the six. Practitioner-relevant items:

- **Plugins in `.claude/skills` directories are now auto-loaded** — no marketplace required. **`claude plugin init <name>`** scaffolds a new plugin straight into `.claude/skills`. `/plugin` argument autocomplete added.
- **Per-session agent override:** the `agent` field in `settings.json` is honored for dispatched sessions, with `--agent <name>` to override at launch.
- **OpenTelemetry:** `tool_decision` events now include `tool_parameters` (bash commands, MCP/skill names) when `OTEL_LOG_TOOL_DETAILS=1`. Useful if you're building usage/observability dashboards.
- **Worktree hygiene:** Claude-managed worktrees are left unlocked when the agent finishes, so `git worktree remove`/`prune` can clean them up. `EnterWorktree` can switch between Claude-managed worktrees mid-session. Also fixes `--worktree` returning to canonical repo root instead of the linked worktree, and orphan-worktree leakage after the 30-day retention sweep.
- **Auto/bypass-mode regression fix:** sandbox network permission prompts no longer appear in auto and bypass-permissions modes when using the desktop app, IDE extensions, or SDK.
- **`claude agents` polish:** completed sessions retire when idle subagents are parked or have leaked backgrounded shells; Esc cancels slow "opening…"; copy-on-select reaches the system clipboard inside tmux with `set-clipboard on` (regression in 2.1.153); slash-command autocomplete in the dispatch input now matches substrings.
- **`--resume` fixes:** background subagents running when the previous Claude Code process exited are reported; the session picker no longer leaves residue in scrollback after exiting fullscreen.
- **Model picker:** "Newer version available" hint is no longer shown for the newest model in a family; the pinned-model row shows model description instead of raw ID.
- **WSL:** `alt+v` image paste, Windows 11 screenshot paste, and drag-from-Windows-Explorer image support.
- **Performance:** redundant message-rendering recomputation eliminated for long/resumed conversations; `/terminal-setup` now disables GPU acceleration in VS Code / Cursor / Devin Desktop integrated terminals to prevent garbled-text rendering.
- **Right-click paste fix in integrated terminals (VS Code, Cursor, Windsurf):** no longer duplicates the clipboard.
- **IDE fixes:** Stop button now actually stops a running background subagent; fast-mode indicator now appears on Opus 4.8 in VS Code.

**v2.1.158 — 2026-05-30 02:42 UTC** ([release](https://github.com/anthropics/claude-code/releases/tag/v2.1.158)). Single feature, but high-value for hyperscaler-deployed Claude Code:

- **Auto mode is now available on Bedrock, Vertex, and Foundry for Opus 4.7 and Opus 4.8**, opt-in via `CLAUDE_CODE_ENABLE_AUTO_MODE=1`. Previously Auto-mode was first-party only. _Why it matters:_ if you're routing through Bedrock for compliance/billing reasons, you no longer have to give up the auto-approval classifier that catches ~83% of overeager behaviors per Anthropic's own published number.

**v2.1.159 — 2026-05-31 19:42 UTC** ([release](https://github.com/anthropics/claude-code/releases/tag/v2.1.159)). _Internal infrastructure improvements (no user-facing changes)._ Skip-worthy.

**v2.1.160 — 2026-06-02 02:10 UTC** ([release](https://github.com/anthropics/claude-code/releases/tag/v2.1.160)). The most user-visible release of the window after v2.1.157:

- **Dynamic-workflow trigger keyword renamed `workflow` → `ultracode`.** The word "workflow" no longer triggers a run; asking for one in your own words still does. The trigger keyword is highlighted in violet in the prompt input. _Why it matters:_ if you have docs, scripts, or hooks that key off the word "workflow," update them. This also lands a config toggle to disable trigger-keyword behavior entirely (introduced in v2.1.157, now the default name).
- **New write-confirmation surface:** prompts before writing to shell startup files (`.zshenv`, `.zlogin`, `.bash_login`) and `~/.config/git/`. In `acceptEdits` mode, also prompts before writing build-tool configs that grant code execution (`.npmrc`, `.yarnrc*`, `bunfig.toml`, `.bazelrc`, `.pre-commit-config.yaml`, `.devcontainer/`, etc.). Directly addresses the supply-chain-via-config class of attacks discussed in [how-we-contain-claude](https://www.anthropic.com/engineering/how-we-contain-claude) (see Other).
- **`grep` satisfies the read-before-edit check.** Single-file `grep`/`egrep`/`fgrep` no longer requires a separate Read.
- **Removed `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE`** — the env var is now a no-op (deprecation flagged 5/28, removed today as scheduled).
- **Background-session reliability:** `claude --bg` no longer fails with "socket missing" during daemon cold-start; resumed sessions don't lose history or re-run the original prompt; agents view doesn't freeze on the auto-updater re-check; Windows input doesn't become unresponsive under heavy CPU load.
- **Auto-mode latency improved** by reducing reasoning on routine actions, lowering "could not evaluate this action" blocks.
- **Removed JetBrains plugin install suggestion from startup.**
- **WSL:** copy-on-select now writes to Windows clipboard via PowerShell interop instead of OSC 52 (works in MobaXterm now).
- **Voice mode:** no longer fails when project dir or branch name contains non-ASCII characters.

**v2.1.161 — 2026-06-02 21:58 UTC** ([release](https://github.com/anthropics/claude-code/releases/tag/v2.1.161)).

- **`OTEL_RESOURCE_ATTRIBUTES` values are now labels on metric datapoints** — you can slice usage metrics by team or repo. Pair with the v2.1.157 `tool_decision` `tool_parameters` improvements for a real OpenTelemetry observability story.
- **Parallel tool calls are now independent:** a failed Bash command in a batch no longer cancels other calls; each tool returns its own result. Important for SDK consumers running parallel tool flows.
- **`/mcp` collapses unused claude.ai connectors** behind a "Show unused connectors" row.
- **`claude agents` rows show `done/total`** when work is fanned out; peek shows the longest-running item.
- **Linux clipboard via `wl-copy`/`xclip`/`xsel`** (auto-detected); copies to both clipboard and PRIMARY selection for middle-click paste.
- **Secret-redaction in `claude mcp` list/get/add:** `${VAR}` references are no longer expanded, and credential headers and URL secrets are redacted. _Practitioner note:_ if you scraped previous `claude mcp` output into logs or shared a session screencap, treat those as having leaked secrets up to this version.
- **Worktree-isolation fix for Workflow agents in background sessions:** they were being blocked from editing files inside their own worktree.
- **`claude -p` stdout corruption fixed** when using `--output-format text` or `json` with background subagent output.
- **Telemetry-init race fix:** OTEL log events (`user_prompt`, `api_request`, `tool_result`, `tool_decision`) were being silently dropped before telemetry initialization completed.
- **`/autofix-pr`** no longer errors in jj workspaces / nested repos; `--resume` picker shows sessions in non-git-worktree dirs.

**v2.1.162 — 2026-06-03 21:31 UTC** ([release](https://github.com/anthropics/claude-code/releases/tag/v2.1.162)). The current latest. Best practitioner-relevant deltas:

- **`claude agents --json` adds `waitingFor`** — shows what a waiting session is blocked on (e.g. permission prompt). Useful for orchestration scripts.
- **`--tools` accepts `Grep`/`Glob` on native builds.** Previously these names were silently ignored on native builds with embedded search (a footgun if you depended on `canUseTool`/hooks for search interception); explicitly listing them now provides the dedicated tools again. The TS Agent SDK (v0.3.162) carries the matching default: native sessions now default to embedded `find`/`grep` in Bash unless you name the dedicated tools in `tools` or `allowedTools`.
- **Slash-command autocomplete behavior change:** clicking a slash command in autocomplete now **fills it into your prompt** instead of running it immediately — press Enter to run. Adjust muscle memory.
- **WebFetch permission rules now apply to built-in preapproved domains:** explicit `WebFetch(domain:...)` deny/ask/allow rules now take precedence over the preapproved-host auto-allow. If you'd been relying on the implicit allowlist, your `deny` rules now actually work — and conversely, your existing `allow` rules now narrow access for those domains too.
- **MCP per-server `timeout` config values < 1000 ms were being floored to 1 s and aborting every tool call.** Sub-1000 ms values are now ignored (falling back to `MCP_TOOL_TIMEOUT` or default), and `claude mcp get` annotates them. _If you have a `timeout: 500` lying around, it now silently defaults instead of killing every tool call._
- **`workspaceSymbol` LSP tool now accepts a `query` parameter** and passes it to the language server — previously returned no results.
- **Refusal stop_reason ends up on SDK transcripts:** refusal error messages now carry `stop_reason: "refusal"` and `stop_details` on the assistant message and in session transcripts, so SDK consumers can detect refusals without text-matching the error content. (Mirrors the May 28 `stop_details` refusal-categories work, but for the local-session path.)
- **API 400 fix:** classifier side-queries and MCP server descriptions containing emoji near a truncation boundary were producing `no low surrogate in string` errors.
- **Renamed Windsurf → Devin Desktop** in `/ide`, `/terminal-setup`, and `/scroll-speed` following the editor's rebrand.
- **Removed `Claude in Chrome enabled` and `marketplace installed` startup messages.**
- **`claude agents` width fixes:** live status text and session names no longer truncate at 60–120 / 40 columns on wide terminals.
- **Cross-session messaging (`SendMessage`):** silently broken when `CLAUDE_CODE_TMPDIR` or `$TMPDIR` pointed at a deep directory — fixed.
- **`EADDRINUSE` errors** from tools binding Unix sockets under `$TMPDIR` with `CLAUDE_CODE_TMPDIR` set to a deep path — fixed.

**Status incidents in window** ([status.claude.com](https://status.claude.com/)):

- **2026-06-03 04:17–07:36 UTC — major — "Issue affecting some Claude Code services."** The biggest Claude Code-specific incident in the window — ~3 hr 20 min. If you saw flaky Claude Code behavior in the European morning of Jun 3, this is the cause.
- **2026-06-03 07:10–07:38 UTC — minor — "Elevated errors on Opus 4.7."** Tail of the Jun 3 event.
- **2026-06-02 06:04–11:49 UTC — major — "Elevated errors on multiple models."** Roughly 5h 45m, multi-model.
- **2026-06-01 — four minor incidents:** Sonnet 4.6 degraded (18:28), Sonnet 4.6 elevated (14:40), Opus 4.7 elevated (12:15), Sonnet 4.5 elevated (09:08), Opus 4.7 elevated (06:48).
- **2026-05-30 22:58 UTC — minor — "Opus 4.7 elevated errors"** (resolved 5/31 00:16).
- **2026-05-29 18:35–19:12 UTC — major — "Elevated errors for Claude Opus 4.8."** The first **major** 4.8 incident; the prior briefing tracked a minor 4.8 incident 5/29 08:30 — this one is upgraded severity, ~2.5 days post-launch.

---

## API & Models

**No new model in window.** Opus 4.8 remains the newest first-party model; Mythos remains gated to Project Glasswing partners.

**`anthropic-sdk-python` and `anthropic-sdk-typescript` did not move in window** (last published versions are `v0.105.2` 2026-05-29 00:20 and `sdk-v0.100.1` 2026-05-29 00:08, both pre-window). The Opus-4.8-only Messages API features (mid-conversation system messages, refusal categories in `stop_details`) shipped in the prior briefing remain the latest API-level deltas.

**`claude-agent-sdk-python` resumed shipping after a 10-day pause.** Versions:

- **v0.2.88 — 2026-06-02 22:11 UTC** ([release](https://github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.88)). **Trio compatibility fix** for session stores: `TranscriptMirrorBatcher`, `session_resume`, and `sessions` were ported from raw `asyncio` to `anyio`, fixing a `TypeError: trio.run received unrecognized yield message` crash when passing `session_store=` to `query()` or `ClaudeSDKClient` under trio (#990). Internal: e2e CI moved from static API key to workload-identity-federation OIDC tokens. Bundled CLI bumped to 2.1.161.
- **v0.2.89 — 2026-06-03 21:44 UTC** ([release](https://github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.89)). Bundled CLI bumped to 2.1.162. No code changes.

_Why it matters:_ Python agent SDK is now back in lockstep with TS (both at the v2.1.162 / v0.3.162 / v0.2.89 cohort). The prior-briefing observation that "TS and Python agent SDKs remain a full Claude Code cycle apart" no longer holds.

**`claude-agent-sdk-typescript` (lockstep with Claude Code):**

- **v0.3.157, v0.3.158, v0.3.159** — parity bumps for Claude Code v2.1.157/158/159.
- **v0.3.160 — 2026-06-02 02:10 UTC** ([release](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.160)). **Fixed SDK hook callbacks swallowing abort signals:** aborting during a `PostToolUse` hook now ends the turn with a final `result` message instead of hanging the calling process. Important if you have abort handling in long-running agent runs.
- **v0.3.161 — 2026-06-02 21:58 UTC** ([release](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.161)). **`initialize` is now idempotent** — a second `initialize` returns the same success payload instead of an `Already initialized` error. `ControlResponse` gains an optional `pending_permission_requests` field, mirroring `ControlErrorResponse`. **`applyFlagSettings` now live-applies `agent` changes** — switching the active agent (or passing `null` to reset) takes effect on the next turn in a running session.
- **v0.3.162 — 2026-06-03 21:31 UTC** ([release](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.162)). **Refusal stop_reason / stop_details on transcripts** (matches Claude Code v2.1.162). **Default search-tool behavior change on native builds:** Agent SDK sessions now default to fast embedded `find`/`grep` in Bash, matching the interactive CLI, instead of registering the dedicated `Grep`/`Glob` tools. To keep the dedicated tools (e.g., for `canUseTool` or hooks interception), name them in the `tools` option or reference them in `allowedTools`. _If you have an SDK orchestrator with hook-based search auditing, verify it still fires after upgrading._

**Multi-cloud — Auto mode on Bedrock/Vertex/Foundry** (v2.1.158). Already covered under Claude Code; mentioned here because this is functionally an API-platform feature for organizations that route through hyperscalers.

**Support release notes** ([support.claude.com release-notes article](https://support.claude.com/en/articles/12138966-release-notes)) — nothing newly listed in window beyond the 2026-05-28 entries (Opus 4.8 launch, Enterprise connector custom roles) covered in the prior briefing.

**No first-party pricing changes** in window.

---

## Research

Two substantive first-party publications, both from the **Frontier Red Team / Policy** axis, both Jun 2–3.

**[Expanding Project Glasswing](https://www.anthropic.com/news/expanding-project-glasswing)** — Jun 2, 2026 (Announcements). Topline figures and what's new:

- **~50 → ~150 partner organizations**, in **15+ countries**, now covering **power, water, healthcare, communications, and hardware** (the original cohort was concentrated in software).
- Partners have already surfaced **>10,000 high-or-critical-severity flaws** (this number was first published in the May 22 Glasswing update).
- The first explicit pivot from "find vulns" to "**disclose, fix, and deploy patches**" — Mythos Preview is being used by partners to **write patches** and for **pre-release checks**, with an internal note that this is now the bottleneck.
- **Mythos-class general release is conditional, not committed:** _"To do so, we'll need highly robust safeguards that prevent the model's cyber capabilities from being misused — safeguards that we (and, to our knowledge, all other AI developers) have yet to develop."_ This walks back, or at least conditions, the "Mythos in the coming weeks" line from the 5/28 Opus 4.8 launch.
- Mentions a new **Cyber Verification Program** that would grant Mythos-class capabilities to additional orgs for specific cyberdefense tasks (no public details yet).
- **[Claude Security](https://claude.com/product/claude-security)** is publicly cited as the productized form of the Glasswing tools — uses Opus 4.8 to scan codebases and suggest patches. If you're building a security-scanning workflow against Anthropic, this is the supported product surface.

**[What we learned mapping a year's worth of AI-enabled cyber threats](https://www.anthropic.com/news/AI-enabled-cyber-threats-mitre-attack)** — Jun 3, 2026 (Policy / Frontier Red Team). Practitioner-relevant findings:

- **Sample:** 832 banned-for-malicious-cyber-activity accounts between March 2025 and March 2026 (subset of total bans, restricted to cases with enough detail to map onto MITRE ATT&CK). Partial results were published in **Verizon's 2026 Data Breach Investigations Report (DBIR)**.
- **AI is moving deeper into the attack life cycle.** AI-assisted phishing (initial access) fell 8.6%; AI for account discovery (post-compromise) rose 8.9%. Malware writing remains the most common AI use case (560/832 = 67.3%); lateral movement is much rarer (54/832 = 6.5%) but is the bigger risk signal.
- **Threat-level inflation:** medium-risk-or-higher share rose from **33% in the first six months → 56% in the second six months** (~1.7×).
- **Old heuristics broken:** number of techniques used and platform used (Claude Code vs API vs chat) **no longer correlate with risk**. The durable signal is **scaffolding architecture** — actors who chain stages sequentially with autonomous decision-making are the highest-risk class.
- **MITRE ATT&CK is now a poor fit.** The Nov 2025 [state-sponsored cyber espionage](https://www.anthropic.com/news/disrupting-AI-espionage) operation looked like a medium-risk actor by ATT&CK count (30 techniques / 13 tactics) but scored 100/100 on Anthropic's internal risk model. Anthropic says it's **in discussions with MITRE** about extending ATT&CK with agentic-orchestration techniques.
- **Companion interactive visualization:** [red.anthropic.com/2026/attack-navigator/](https://red.anthropic.com/2026/attack-navigator/) (referenced from the post; not directly verified for in-window timestamp, but contemporaneous with the news post).

_Why it matters to an applied engineer:_ if you ship anything that ingests, reasons about, or routes around model-generated activity (DLP, abuse monitoring, agent-orchestration auditing), the "number of techniques" heuristic is now publicly deprecated by the model vendor. Whether attack scaffolding chains stages is the signal to feature-engineer toward.

**No new content on [alignment.anthropic.com](https://alignment.anthropic.com/) or [anthropic.com/research](https://www.anthropic.com/research) homepage beyond these items.** The research-page top item remains the May 27 "Coding agents in the social sciences" (covered in prior briefing).

---

## Rumors & Leaks

**Anthropic IPO setup is no longer rumor — draft S-1 confirmed.** [anthropic.com/news/confidential-draft-s1-sec](https://www.anthropic.com/news/confidential-draft-s1-sec) (Jun 1, 2026). Filed confidentially under Rule 135; share count and price not yet set. Press coverage in window: [NYT](https://www.nytimes.com/2026/06/01/technology/anthropic-ipo.html) ([HN #48359035](https://news.ycombinator.com/item?id=48359035)). _Practitioner read:_ this is the corporate-structure move that typically precedes a 6–12-month IPO window. No price-discovery yet, so the rumored 11-figure valuations are still rumor — but the IPO option itself is now first-party confirmed.

**"Anthropic scales Claude Mythos to critical infrastructure in 15 countries"** — TechCrunch ([HN #48373544](https://news.ycombinator.com/item?id=48373544), 46 pts). _Resolved._ This is the same announcement as the Jun 2 Glasswing expansion above — TechCrunch's framing leans on the "15 countries" number, but the actual first-party number is "**more than fifteen**." Reclassified from rumor → press echo of confirmed news.

**"Mythos in the coming weeks" — softened.** As covered in Research above, the Jun 2 Glasswing post explicitly conditions Mythos GA on safeguards "that no AI developer has yet developed." The 5/28 launch line should now be read as conditional, not as a near-term schedule.

**Michael Burry sub-$1T valuation skepticism** ([businessinsider.com](https://www.businessinsider.com/big-short-michael-burry-spacex-anthropic-ipo-ai-bubble-claude-2026-6), [HN #48368187](https://news.ycombinator.com/item?id=48368187), 134 pts / 153 comments). Burry (of "The Big Short") publicly argued that neither SpaceX nor Anthropic is worth $1T. Not a rumor in the operational sense — just public counter-narrative to the $965B Series H post-money figure — but it's the highest-vote skeptic thread of the window, useful counter-framing if you're citing the Series H number in budget conversations.

**No new unverified-source rumors** found via HN front page in window.

---

## Other (community, third-party tooling, press)

**HN front page — "Can the stockmarket swallow Anthropic, SpaceX and OpenAI?"** ([Economist, #48364055](https://news.ycombinator.com/item?id=48364055), 2026-06-01 23:45 UTC, **712 pts / 1,257 comments**). By far the dominant general-audience discussion of the window. Treat as press echo of the S-1 filing (lands the same day); high-signal for "what does the macro picture look like" framing, low-signal for any operational AI-engineering takeaway.

**HN front page — "The ways we contain Claude across products"** ([anthropic.com/engineering/how-we-contain-claude](https://www.anthropic.com/engineering/how-we-contain-claude), [HN #48392082](https://news.ycombinator.com/item?id=48392082) 2026-06-04 00:27 UTC, **139 pts / 67 comments**). _Publication date is May 25, 2026_ — outside the window — and it was already cited in the May 27 and May 28 briefings. **However, the HN front-paging is in-window**, and the article is now the canonical citation for several of the v2.1.160/161/162 design choices (write-confirmation prompts for shell startup files / build-tool configs, defensive proxy that rejects attacker-provided API keys, the egress-allowlist-as-capability-grant principle). If you have a security or platform audience, the engineering post is the highest-leverage single read of the window. The footnote that **Claude Code auto mode blocks only 0.4% of benign commands but ~17% of overeager actions get through** is the cleanest published number for "should I run Auto mode" trade-offs.

**HN front page — "Anthropic scales Claude Mythos to critical infrastructure in 15 countries"** ([TechCrunch, #48373544](https://news.ycombinator.com/item?id=48373544), 46 pts) — press echo of the Glasswing expansion. Already covered above.

**HN front page — "Claude Opus 4.8 Max responding to an empty message"** ([xcancel.com/davidad](https://xcancel.com/davidad/status/2061858258046898518), [HN #48383564](https://news.ycombinator.com/item?id=48383564), 27 pts / 4 comments, 2026-06-03 13:14 UTC). Low-signal but worth noting: example of Opus 4.8's behavior with empty-prompt inputs. Lightweight community curiosity rather than a substantive report.

**HN — "Launch HN: Hyper (YC P26) – Company brain to power agentic development"** ([HN #48387095](https://news.ycombinator.com/item?id=48387095), 2026-06-03 17:39 UTC, 70 pts / 61 comments). Third-party "company memory layer for agents" — YC P26 startup. Per their HN post, they integrate via "lifecycle hooks in tools like **Claude Code, Cowork, Codex, and Cursor**" plus MCP for everything else. Practical signal: there's a wave of "context layer" startups targeting Claude Code / Cowork's hook surface; if you're considering shipping your own MCP server for company-doc context, this is the competitive landscape.

**Enterprise admin — Connector permissions in custom roles** ([support release note article, 5/28 entry](https://support.claude.com/en/articles/12138966-release-notes)). The May 28 release-note item from the Opus 4.8 launch remains the latest enterprise-admin entry; no new in-window entries on the support release-notes page.

**Frontier Red Team blog** referenced from the MITRE post: [red.anthropic.com/2026/attack-navigator/](https://red.anthropic.com/2026/attack-navigator/). Interactive ATT&CK technique visualization.

**Press echoes (lower signal, in-window):** [NYT "Anthropic Files to Go Public, Setting Stage for Huge I.P.O."](https://www.nytimes.com/2026/06/01/technology/anthropic-ipo.html) ([HN #48359035](https://news.ycombinator.com/item?id=48359035), 26 pts).

**No first-party Anthropic engineering or research blog post landed _within_ the window** beyond the two Frontier-Red-Team items above. The engineering post that drove HN traffic on Jun 4 was published outside the window.

---

## Excluded but worth a look

- **"The ways we contain Claude across products"** ([anthropic.com/engineering/how-we-contain-claude](https://www.anthropic.com/engineering/how-we-contain-claude), published May 25, 2026). Outside the window and covered in the May 27/28 briefings, but its Jun 4 HN front-paging (139 pts) makes it the highest-traffic Anthropic-engineering item of the window. If your audience hasn't read it yet, the v2.1.160–162 design choices are directly traceable to its principles (write-confirmation for shell config files; egress allowlist as capability grant; refusal stop_reason in transcripts).
- **`red.anthropic.com/2026/attack-navigator/`** — interactive ATT&CK visualization referenced from the Jun 3 MITRE post. Page is referenced from a confirmed-in-window first-party post, but I couldn't independently verify a publication timestamp on the red-team blog page itself. Probably safe to treat as in-window companion content; flagged here for the curious.
- **Verizon 2026 DBIR** — referenced from Anthropic's MITRE post as the venue where partial results were published. Off-Anthropic, no direct link verified in this pass, but worth pulling if you do threat-intel work.
- **Anthropic Milan office (May 27)** — flagged in the prior briefing's Excluded section as pre-window. Still pre-window; mentioning again only because it sits inside the catch-up span (5/29–6/4). If you missed last week's briefing entry: [news/milan-office-opening](https://www.anthropic.com/news/milan-office-opening), sixth European office.

---

## Sources (in-window items only)

- [Anthropic confidentially submits draft S-1 to the SEC](https://www.anthropic.com/news/confidential-draft-s1-sec) (Jun 1, 2026)
- [Expanding Project Glasswing](https://www.anthropic.com/news/expanding-project-glasswing) (Jun 2, 2026)
- [What we learned mapping a year's worth of AI-enabled cyber threats](https://www.anthropic.com/news/AI-enabled-cyber-threats-mitre-attack) (Jun 3, 2026) · [Frontier Red Team interactive attack navigator](https://red.anthropic.com/2026/attack-navigator/)
- [Introducing the Services Track and Partner Hub of the Claude Partner Network](https://www.anthropic.com/news/services-track-partner-hub) (Jun 3, 2026)
- Claude Code releases: [v2.1.157](https://github.com/anthropics/claude-code/releases/tag/v2.1.157) · [v2.1.158](https://github.com/anthropics/claude-code/releases/tag/v2.1.158) · [v2.1.159](https://github.com/anthropics/claude-code/releases/tag/v2.1.159) · [v2.1.160](https://github.com/anthropics/claude-code/releases/tag/v2.1.160) · [v2.1.161](https://github.com/anthropics/claude-code/releases/tag/v2.1.161) · [v2.1.162](https://github.com/anthropics/claude-code/releases/tag/v2.1.162) · [CHANGELOG.md](https://raw.githubusercontent.com/anthropics/claude-code/main/CHANGELOG.md)
- `claude-agent-sdk-typescript` v0.3.157 → [v0.3.162](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.162) (lockstep with Claude Code; notable changes in [v0.3.160](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.160), [v0.3.161](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.161), [v0.3.162](https://github.com/anthropics/claude-agent-sdk-typescript/releases/tag/v0.3.162))
- `claude-agent-sdk-python` [v0.2.88](https://github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.88) · [v0.2.89](https://github.com/anthropics/claude-agent-sdk-python/releases/tag/v0.2.89)
- [status.claude.com](https://status.claude.com/) — major "Issue affecting some Claude Code services" (2026-06-03 04:17 UTC); major "Elevated errors on multiple models" (2026-06-02 06:04 UTC); major "Elevated errors for Claude Opus 4.8" (2026-05-29 18:35 UTC); plus eight minor incidents
- HN: [Economist #48364055](https://news.ycombinator.com/item?id=48364055) · [S-1 #48358646](https://news.ycombinator.com/item?id=48358646) · [Glasswing #48369863](https://news.ycombinator.com/item?id=48369863) · [Burry #48368187](https://news.ycombinator.com/item?id=48368187) · [how-we-contain #48392082](https://news.ycombinator.com/item?id=48392082) · [TechCrunch Mythos #48373544](https://news.ycombinator.com/item?id=48373544) · [Hyper Launch HN #48387095](https://news.ycombinator.com/item?id=48387095) · [Opus 4.8 empty msg #48383564](https://news.ycombinator.com/item?id=48383564)
- HN Algolia: `search_by_date?numericFilters=created_at_i>1780052760,created_at_i<1780571160` (= 2026-05-29 11:06 UTC → 2026-06-04 11:06 UTC), queries `anthropic`, `claude`

## Method / verification notes

- **Window anchor:** prior briefing `anthropic-daily-update-2026-05-29.md` (self-reported window end 2026-05-29 11:06 UTC) used as window start. Window end = `date -u` at compose time = 2026-06-04 11:06 UTC. Length ≈ 6.0 days → gap detected (5 skipped briefings between 5/30 and 6/3); catch-up header added; under the 7-day cap.
- **GitHub verified via `gh release list`/`gh release view`** across the five repos: `claude-code` (v2.1.157–162 ✓), `claude-agent-sdk-typescript` (v0.3.157–162 ✓), `claude-agent-sdk-python` (v0.2.88, v0.2.89 ✓ — fully back in lockstep), `anthropic-sdk-python` (no in-window release; v0.105.2 from 5/29 00:20 is just outside window start), `anthropic-sdk-typescript` (no in-window release; sdk-v0.100.1 from 5/29 00:08 is just outside window start).
- **First-party news verified directly** by fetching each news post page (S-1, Glasswing expansion, MITRE/ATT&CK, Services Track). All four carried explicit on-page dates.
- **Dedup against prior briefing:** the May 25 "how-we-contain-claude" engineering post was already covered in the 5/27 and 5/28 briefings and is excluded from the main sections; included only as a community item under "Other" because the HN front-paging is in-window. The Opus 4.8 launch, Series H, mid-conversation system messages, and refusal-categories work were all covered previously and are not repeated.
- **Status incidents** pulled from `status.claude.com/api/v2/incidents.json` (10 in-window incidents identified: 3 major, 7 minor).
- **Platform release notes / support release notes** are JS-rendered; static content recovered from embedded HTML — no new in-window entries beyond 5/28 ones already covered in the prior briefing.
- **HN coverage** validated via Algolia `search_by_date` with explicit numeric created-at filters; minimum thresholds 10 pts / 5 comments for `anthropic` query and 15 pts / 8 comments for `claude` query (latter is broader and noisier).
- **Frontier Red Team interactive attack navigator** (`red.anthropic.com/...`) page timestamp not independently verified; included as companion content to the confirmed-in-window MITRE news post.
