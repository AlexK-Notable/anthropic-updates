# Anthropic daily briefing — 2026-07-02

_Briefing window: 2026-07-01 11:06 UTC → 2026-07-02 11:06 UTC (~24 hours)._

Prior briefing: `anthropic-daily-update-2026-07-01.md` (window end 2026-07-01 11:06 UTC).

---

## Headline

Quiet day by comparison to yesterday's launch avalanche. The main story is the **Fable 5 redeployment landing in practice**: Anthropic flipped the "automatic model switching" mechanism on by default across all consumer surfaces (Claude/Cowork/Code/Design/M365/Teams/Slack), so cyber- and biology-adjacent Fable 5 requests silently retry on Opus 4.8 mid-conversation — with a new Help Center article confirming both the input-vs-midstream billing rules and the "sticky Opus" behavior for the rest of the thread. **Claude Code v2.1.198** shipped alongside, with **Claude in Chrome going GA**, `/agents` wizard removed, background agents now auto-committing/pushing/opening draft PRs, and a new `/dataviz` skill. On the security side, Armadin published a Cowork-for-Windows sandbox escape (root inside the VM + full network egress via two undocumented `spawn` params) that Anthropic classified as **not a security issue** because it requires local host code execution. HN chatter was low-signal and mostly Fable-flavor sentiment ("Sonnet 5 not frontier," "chart moved after it looked bad," "Fable jailbroken hours after restoration").

---

## Claude Code

### **NEW — v2.1.198** (released 2026-07-01 20:45 UTC)

- Source: [github.com/anthropics/claude-code/releases/tag/v2.1.198](https://github.com/anthropics/claude-code/releases/tag/v2.1.198) — mirrored to [code.claude.com/docs/en/changelog](https://code.claude.com/docs/en/changelog).
- **Claude in Chrome is now generally available** — no more waitlist. First-party GA nod for the Chrome MCP surface.
- **`/agents` wizard removed** — you're expected to ask Claude to manage subagents in-conversation or edit `.claude/agents/` files directly. Breaking for anyone who scripted the wizard.
- **Background agents in `claude agents` now commit, push, and open a draft PR** when they finish code work in a worktree, rather than pausing to ask. This is a real behavior change for long-running unattended sessions — if you weren't expecting drive-by PRs, adjust `.claude/settings.json` or your worktree conventions.
- **New `Notification` hook events** `agent_needs_input` / `agent_completed` for background agents. Practical for anyone wiring Claude Code to Slack/pager/desktop notifiers.
- **`/dataviz` skill added** — chart and dashboard design guidance with a runnable color-palette validator. Bundled with the CLI, not a marketplace skill.
- **Explore agent** now inherits the parent session's model (capped at Opus) instead of forcing Haiku. Larger context, better search, higher cost.
- **Subagents + context compaction inherit extended-thinking config** from the parent session — improves delegated-task quality but changes per-turn cost profile.
- **Gateway**: Claude Platform on AWS (`anthropicAws`) added as an upstream provider; model-not-found now advances the failover chain instead of dead-ending.
- **Reliability fixes**: mid-response `ECONNRESET` now retries with backoff instead of aborting the turn; background task panels no longer stick on "Running" after completion; `awsAuthRefresh` auto-runs on STS expiry.

**Why it matters**: three items are behavior changes you should notice before rolling out: (1) auto-PR behavior for background worktree agents (potential noise or accidental leaks), (2) Explore agent cost jumps significantly under Sonnet 5 pricing, (3) removal of `/agents` wizard breaks any tutorial/docs that reference it. The `/dataviz` skill is a small but real quality-of-life win for anyone generating charts inside CC.

### **NEW — `claude-code-action` v1.0.162** (2026-07-01 20:47 UTC)

- Companion GitHub Action bump matching v2.1.198. Standard cadence.

### Community items in window

- **[ZCode: Claude Code from the Makers of GLM](https://zcode.z.ai/cn)** — HN [48751752](https://news.ycombinator.com/item?id=48751752), **274 pts, 13 comments**, 2026-07-01 19:11 UTC. Zhipu AI shipped a Claude-Code fork/clone bundled with their GLM models (Chinese-market pitch, low comment count for a #1-adjacent story suggests curiosity without substantive engagement). Worth noting as a signal that CC is now the reference UX for terminal-based agents.
- **["Using network namespaces to discover how Claude Code scrapes"](https://patrickmccanna.net/inspecting-claude-codes-network-traffic-with-linux-namespaces-and-mitm-proxying-part-1/)** — HN [48746915](https://news.ycombinator.com/item?id=48746915), 3 pts. Blog on mitmproxying CC traffic via Linux net namespaces. Part 1; low HN pickup, but methodologically useful for anyone auditing what the shipped bundle sends (especially in the aftermath of the steganography story).
- **["Claudes newest update is putting a worse model to default"](https://news.ycombinator.com/item?id=48751958)** — 1 pt, low signal, complaint that v2.1.197's Sonnet 5 default felt worse than Sonnet 4.6. Consistent with Zvi's post below and yesterday's tokenizer story.
- **["I put Claude Code on my Garmin running watch"](https://github.com/fashton28/garmin-code)** — HN [48754085](https://news.ycombinator.com/item?id=48754085), 2 pts. Not practical but reflects the "CC-anywhere" meme.

---

## API & models

### **Fable 5 redeployment — the "automatic model switching" mechanic is now documented**

- **NEW — Support article: ["Why Claude switched models in your conversation with Fable 5"](https://support.claude.com/en/articles/15363606-why-claude-switched-models-in-your-conversation-with-fable-5)** (updated today). HN [48759255](https://news.ycombinator.com/item?id=48759255), 1 pt but high-signal for practitioners. Key details:
  - Fable 5's safeguards **block three categories**: offensive cybersecurity techniques (exploits/malware/attack tooling), biology/life-sciences queries (lab methods, molecular mechanisms), and extraction of the model's summarized thinking.
  - The safeguards review **memory, connector content, web-search results, and files** — not just the current user message. A block can be triggered by content the user did not type.
  - **On block, the conversation silently switches to Opus 4.8** and stays sticky-on-Opus for the rest of the thread. Users can switch back manually, but Anthropic warns that "the same safeguards may block the conversation again because the original request is still part of it."
  - **Billing**: blocked-on-input → charged only at Opus rates. Blocked midstream → **input + tokens streamed pre-block billed at Fable 5 rates, remainder at Opus rates**. This is a new pricing wrinkle; token accounting dashboards should be updated.
  - **Enabled by default** the first time you select Fable 5, across Claude.ai / Cowork / Code / Design / M365 / Teams / Slack / Mobile / Desktop. Toggle in Settings → Capabilities (or `/config` MODEL & OUTPUT in Claude Code).
  - **API is different**: automatic switching is **not automatic on the Claude API** — API customers must opt in and configure it.
- **[HN: "Fable 5 will default to Opus 4.8 for coding tasks"](https://news.ycombinator.com/item?id=48750456)** — 47 pts, 29 comments, 2026-07-01 17:34 UTC. Comment sentiment mixed — a lot of "so it's not really Fable 5 anymore for anyone doing security work."
- **[HN #1-ish: "Fable 5 is Back"](https://news.ycombinator.com/item?id=48752030)** — **379 pts, 370 comments**, 2026-07-01 19:35 UTC. Announcement thread; comment sentiment tracks the Sonnet 5 skepticism from yesterday, with the loudest sub-threads about the Opus routing behavior and about the "promotional access" pricing (below).
- **[HN: "Claude Fable 5 Promotional Access"](https://news.ycombinator.com/item?id=48751978)** — 100 pts, 82 comments, 2026-07-01 19:31 UTC. Support-center article. Fable 5 is promotionally free-then-metered for Premium subscribers for one week.
- **[Zvi Mowshowitz — "Claude Sonnet 5 Is Not Frontier But Has Its Uses"](https://thezvi.substack.com/p/claude-sonnet-5-is-not-frontier-but)** — HN [48755488](https://news.ycombinator.com/item?id=48755488), 8 pts. Zvi's substack (2026-07-01 22:41 UTC modified time). Framing: Sonnet 5 is a useful mid-tier workhorse but not the state-of-the-art general model.

**Why it matters**: for anyone building on Fable 5 via the consumer surfaces, your effective model at inference time is now **conditionally Opus 4.8** on any prompt whose context (including files/memory/connectors) touches cyber or bio. Cost and latency baselines you set yesterday are stale. For API customers this is opt-in — no surprise routing — but the pricing rules for midstream blocks are new and easy to miss.

### **["Fable Jailbroken Hours After Anthropic Lifted Restrictions"](https://news.ycombinator.com/item?id=48750400)**

- 3 pts, 1 comment, 2026-07-01 17:30 UTC. Elder_plinius on X claiming jailbreak within hours. Low HN signal, but predictable — worth flagging as a data point against the ">99% blocked" CAISI classifier claim in yesterday's briefing. No independent confirmation in window.

### SDK releases

Two waves inside the window, both patch-only.

- **Wave A (2026-07-01 19:36–19:47 UTC)** — Ruby SDK 1.53.0 → **1.54.0**, PHP SDK 0.34.0 → **0.35.0**. Anthropic backfilling the Sonnet 5 / Managed Agents wave for the two SDKs that lagged yesterday.
- **Wave B (2026-07-01 21:54–22:05 UTC)** — coordinated patch bumps across all SDKs: `anthropic-python` **v0.115.1**, `@anthropic-ai/sdk` **v0.109.1**, `anthropic-sdk-go` **v1.55.1**, `anthropic-sdk-java` **v2.47.1**, `anthropic-sdk-ruby` **v1.54.1**, `anthropic-sdk-php` **v0.35.1**, `anthropic-cli` **v1.15.0**. Post-launch patch consolidation; check each repo's release notes for the specific fixes.

### Docs release notes

- **[Jul 1, 2026](https://docs.claude.com/en/release-notes/api)**: "We've restored access to Claude Fable 5 and Claude Mythos 5. See [our statement](https://www.anthropic.com/news/redeploying-fable-5-mythos-5) for more information." (Note: URL in the docs is `redeploying-fable-5-mythos-5`; the newsroom title is just `redeploying-fable-5`.)
- No other API/docs release-notes entries in window.

### Status / reliability

- **NEW — [Jul 2 — Elevated errors on Claude Opus 4.8](https://status.claude.com/incidents/lcccgkzvj2yx)**. Investigating 00:38 UTC → Identified 00:58 UTC → Resolved 01:19 UTC. **~40 minutes.** This is the third Opus 4.8 error incident in a week — patternish. Might correlate with the Fable-block auto-reroute flood driving unexpected Opus load, but not attributed.
- **[Jul 1 19:26 UTC](https://status.claude.com/incidents/s9w82lp9dcn9)** — Fable 5 / Mythos 5 export-control suspension **resolved** on the status page, linking to `anthropic.com/news/redeploying-fable-5`. This closes the June 12 incident.

---

## Research & engineering

- **Nothing new on** [anthropic.com/research](https://www.anthropic.com/research) — most-recent still "Anthropic Economic Index report: Cadences" (Jun 26).
- **Nothing new on** [anthropic.com/engineering](https://www.anthropic.com/engineering) — no post since Apr 23. Still no Sonnet 5 engineering write-up two days after launch, which is unusual for a major model release.
- **[Anthropic HackerOne Cyber Jailbreak Program](https://hackerone.com/anthropic-cyber-jailbreak/?type=team)** — HN [48747840](https://news.ycombinator.com/item?id=48747840), 2 pts. Public HackerOne program landing page (mentioned in yesterday's Glasswing announcement but the HN submission surfaced today). Practical entry point if you red-team for pay.
- **[Anthropic to develop its own (additional) drugs](https://www.statnews.com/2026/06/30/anthropic-ai-drug-development/)** — HN [48752867](https://news.ycombinator.com/item?id=48752867), 2 pts, STAT News. Follow-on to the Claude Science launch — signals Anthropic itself will use Claude Science internally for drug discovery in addition to selling it. Low HN engagement but material as a strategic signal.

---

## Rumors & Leaks

- **["Anthropic Changed the Sonnet 5 Chart After It Made Sonnet Look Bad"](https://www.vincentschmalbach.com/anthropic-changed-sonnet-5-chart-after-it-made-sonnet-look-bad/)** — HN [48759181](https://news.ycombinator.com/item?id=48759181), 3 pts, 2026-07-02 10:26 UTC. Schmalbach again (the author whose China-router post drove yesterday's HN #1). Claim: Anthropic revised one of the Sonnet 5 launch benchmark charts after it drew unfavorable comparisons. Low HN signal; log as sentiment-barometer, not confirmed.
- **Steganography / "China-router fingerprint" story** — no material new development in window. No first-party Anthropic response, no news post, no v2.1.198 changelog line addressing it. The `patrickmccanna.net` netns-mitmproxy series (linked above) is the closest thing to methodological follow-up and it doesn't extend Schmalbach's findings.
- **["The Anthropic Fable Ban Is Over. The Battle over How to Tame AI Has Just Begun"](https://www.wsj.com/tech/ai/the-anthropic-fable-ban-is-over-the-battle-over-how-to-tame-ai-has-just-begun-e93f51d6)** — HN [48759022](https://news.ycombinator.com/item?id=48759022), 3 pts, WSJ, 2026-07-02 10:07 UTC. Mainstream framing of the Fable saga; paywalled. Low HN engagement but a signal on how the story lands with regulators and enterprise buyers.
- **["Claude Fable 5 was banned for 18 days – what happened"](https://freemalta.com/hub/library/claude-fable-5-was-banned-for-18-days-heres-what-actually-happened-and-why-mythos-is-different)** — HN [48754856](https://news.ycombinator.com/item?id=48754856), 3 pts. Timeline recap; low signal, no new facts, but useful shape-of-story reference if writing anything internal on the Fable episode.

---

## Other / community

### Security & vulnerabilities in window

- **NEW — [Armadin: "Exploiting Root Execution in Claude Cowork's Sandbox"](https://www.armadin.com/blog-posts/exploiting-root-execution-in-claude-coworks-sandbox)** — HN [48750483](https://news.ycombinator.com/item?id=48750483), 2 pts, dated 2026-07-01 (7.1.26 on the post). **Substantive Cowork-for-Windows sandbox escape.**
  - Target: Claude Desktop for Windows 1.9255.2.0. Cowork on Windows runs Claude Code inside a Hyper-V Ubuntu VM (invisible to hcdiag/normal Hyper-V tooling) with bubblewrap, seccomp, per-session unprivileged users, and a domain-allowlist proxy.
  - Attack chain: (1) DLL-sideload `USERENV.dll` next to `claude.exe` to obtain code exec inside an Authenticode-signed binary; (2) speak the undocumented named-pipe RPC to `CoworkVMService`; (3) call `spawn` with `isResume: true` and `name: "root"` — the daemon skips the "existing user" check and grants uid 0 with full capabilities inside the bwrap namespace; (4) `nsenter -t 1 -p -m -u -i -n` breaks out of bwrap into the VM's namespaces; (5) `allowedDomains: ["*"]` on the same `spawn` disables the egress proxy for that process.
  - **Disclosure timeline**: reported 2026-03-20; **Anthropic responded 2026-03-24 that it's not a security issue because exploitation requires local code execution on the host**. Armadin disagrees implicitly (they published four months later).
  - Defensive recommendations: uninstall Claude Desktop where Cowork isn't required, use AppLocker Packaged App rules on `Claude_*` to scope the population, Sysmon EID 7 on `claude.exe` loading `USERENV.dll` from outside `System32` as a high-signal detection.
- **["Red teamers turned Claude Desktop into a double agent to do their evil bidding"](https://www.theregister.com/security/2026/07/01/red_teamers_turned_claude_desktop/)** — HN [48754194](https://news.ycombinator.com/item?id=48754194), 4 pts. Register piece, 2026-07-01. Content couldn't be fetched cleanly in window; likely covers the same or an adjacent research thread. Worth reading directly.

**Why it matters**: even setting aside the "requires local code exec" caveat, the Armadin write-up is a useful reference for anyone deploying Cowork in an enterprise: the AppLocker + Sysmon guidance is directly actionable, and the RPC surface descriptions save you from having to enumerate `cowork-vm-service` yourself. It's also a small but real signal on how Anthropic's security team triages sandbox-escape research (they'll close it as "requires local RCE").

### Third-party MCP / tooling in window

- **[Show HN: Claude Desktop Switcher](https://matsumotory.github.io/claude-desktop-switcher/)** — HN [48756611](https://news.ycombinator.com/item?id=48756611), 2 pts. Tool for separating Claude Desktop identity/session state.
- **[Show HN: Agent Sessions — a model-agnostic Managed Agents alternative](https://www.agentsessions.dev/)** — HN [48754784](https://news.ycombinator.com/item?id=48754784), 2 pts. Third-party clone of Anthropic's Managed Agents surface.
- **[TS Compiler Graph MCP](https://github.com/samchon/ttsc/tree/master/packages/graph)** — HN [48750084](https://news.ycombinator.com/item?id=48750084), 3 pts. MCP server exposing TS compiler graph; claims 10× token reduction for CC/Codex. Practical for TS repos.
- **[recallplugin.dev](https://recallplugin.dev)** — HN [48747867](https://news.ycombinator.com/item?id=48747867), 2 pts. Claim: reduce token waste on CC session continuation.
- **[Recursive AI Research Skill](https://github.com/Toadoum/ai-research-skill)** — HN [48747615](https://news.ycombinator.com/item?id=48747615), 3 pts.
- **[HT-ML.app — Deploy HTML artifacts from CC/Codex](https://ht-ml.app/)** — HN [48749414](https://news.ycombinator.com/item?id=48749414), 1 pt.
- **[Claudoro (Pomodoro in CC statusline)](https://github.com/emson/claudoro)** — HN [48745590](https://news.ycombinator.com/item?id=48745590), 1 pt.

### Community write-ups

- **[Zvi Mowshowitz — Sonnet 5 review](https://thezvi.substack.com/p/claude-sonnet-5-is-not-frontier-but)** — see API & models section. The most substantive independent Sonnet 5 read of the window.
- **[TechCrunch/MIT Technology Review coverage](https://www.technologyreview.com/2026/07/01/1139996/the-download-anthropic-claude-science-california-carbon-manure/)** — MIT Tech Review's daily digest leads with Claude Science; HN [48757578](https://news.ycombinator.com/item?id=48757578), 2 pts. Reflects Claude Science getting mainstream-tech-press treatment now.
- **[testingcatalog — Early look at Claude Science](https://www.testingcatalog.com/early-look-at-anthropics-claude-science-app-for-researchers/)** — HN [48755995](https://news.ycombinator.com/item?id=48755995), 1 pt. Hands-on-ish walkthrough.
- **[Wired — "Claude Helped a Hacker Find a Way to Issue Tickets to US Music Festivals"](https://www.wired.com/story/claude-helped-a-hacker-find-a-way-to-issue-tickets-to-almost-every-us-music-festival/)** — HN [48746847](https://news.ycombinator.com/item?id=48746847), 5 pts. Real-world misuse narrative; picks up ammo for the "Fable/Mythos safeguards are needed" argument in the news cycle.
- **["Anthropic says Fable 5 will now flag and route harmless queries to Opus"](https://twitter.com/claudeai/status/2072402638247968855)** — HN [48752130](https://news.ycombinator.com/item?id=48752130), 6 pts. Anthropic's official X post confirming the auto-switch mechanic. Effectively an @AnthropicAI tweet-thread version of the Support-center article above.

---

## Excluded but worth knowing

- **[The Register — "Red teamers turned Claude Desktop into a double agent"](https://www.theregister.com/security/2026/07/01/red_teamers_turned_claude_desktop/)** — fetch failed in window; likely covers Armadin or an adjacent sandbox-escape thread. Worth an eyeball tomorrow.
- **First-party Anthropic response on the steganography / China-router story** — still none. Silence now spans 48 hours + a marquee launch + a #1 HN thread. If Anthropic breaks silence in the next window, that becomes the story.
- **[thenewstack.io](https://thenewstack.io/ai-agent-infrastructure-reliability/) — "Anthropic's Sonnet 5 system card says more about the future of AI than benches"** — HN [48745764](https://news.ycombinator.com/item?id=48745764), 1 pt. System-card-focused read; low signal, but worth catching if you procure for enterprise on the basis of safety-eval methodology.
- **Sonnet 5 engineering write-up** — Anthropic engineering blog still silent since Apr 23. Two days post-launch and no methodological write-up on the tokenizer change or adaptive-thinking default. Flagging again in case one drops.

---

## Sources

- [Claude Code v2.1.198 release](https://github.com/anthropics/claude-code/releases/tag/v2.1.198)
- [code.claude.com changelog](https://code.claude.com/docs/en/changelog)
- [claude-code-action v1.0.162](https://github.com/anthropics/claude-code-action/releases/tag/v1.0.162)
- [Support: Why Claude switched models in your conversation with Fable 5](https://support.claude.com/en/articles/15363606-why-claude-switched-models-in-your-conversation-with-fable-5)
- [Anthropic newsroom — Redeploying Fable 5](https://www.anthropic.com/news/redeploying-fable-5)
- [docs.claude.com API release notes](https://docs.claude.com/en/release-notes/api)
- [Status: Elevated errors on Opus 4.8 — Jul 2](https://status.claude.com/incidents/lcccgkzvj2yx)
- [Status: Fable 5 / Mythos 5 restored — Jul 1](https://status.claude.com/incidents/s9w82lp9dcn9)
- [anthropic-sdk-python releases](https://github.com/anthropics/anthropic-sdk-python/releases)
- [anthropic-sdk-typescript releases](https://github.com/anthropics/anthropic-sdk-typescript/releases)
- [anthropic-cli releases](https://github.com/anthropics/anthropic-cli/releases)
- [Armadin — Exploiting Root Execution in Claude Cowork's Sandbox](https://www.armadin.com/blog-posts/exploiting-root-execution-in-claude-coworks-sandbox)
- [Zvi — Claude Sonnet 5 Is Not Frontier But Has Its Uses](https://thezvi.substack.com/p/claude-sonnet-5-is-not-frontier-but)
- [Anthropic HackerOne Cyber Jailbreak Program](https://hackerone.com/anthropic-cyber-jailbreak/?type=team)
- [WSJ — The Anthropic Fable Ban Is Over](https://www.wsj.com/tech/ai/the-anthropic-fable-ban-is-over-the-battle-over-how-to-tame-ai-has-just-begun-e93f51d6)
- [MIT Tech Review — The Download](https://www.technologyreview.com/2026/07/01/1139996/the-download-anthropic-claude-science-california-carbon-manure/)
- [STAT — Anthropic to develop its own drugs](https://www.statnews.com/2026/06/30/anthropic-ai-drug-development/)
- [Wired — Claude helped a hacker issue music-festival tickets](https://www.wired.com/story/claude-helped-a-hacker-find-a-way-to-issue-tickets-to-almost-every-us-music-festival/)
- [Register — Red teamers turned Claude Desktop into a double agent](https://www.theregister.com/security/2026/07/01/red_teamers_turned_claude_desktop/)
- [HN — Fable 5 is Back (379 pts)](https://news.ycombinator.com/item?id=48752030)
- [HN — Fable 5 Promotional Access (100 pts)](https://news.ycombinator.com/item?id=48751978)
- [HN — Fable 5 defaults to Opus 4.8 for coding (47 pts)](https://news.ycombinator.com/item?id=48750456)
- [HN — ZCode: Claude Code from GLM makers (274 pts)](https://news.ycombinator.com/item?id=48751752)
