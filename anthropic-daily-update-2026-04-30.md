# Anthropic Daily Briefing — 2026-04-30

**Window:** April 29 ~00:00 UTC → April 30 ~23:59 UTC (last 24 hours)
**Dedup note:** Prior briefings (most recently `anthropic-daily-update-2026-04-30.md` written earlier today, and 2026-04-29) covered Claude Mythos / Project Glasswing, Opus 4.7 GA, the npm source leak, the April 23 Claude Code postmortem, the Pro-plan pricing flap, the Emotion Concepts research paper, Claude Design / Cowork GA, and the Goldman Sachs Hong Kong access story. Items below are either new or material updates on those threads.

---

## API & Models

### Claude Security — public beta launched
- **Source (first-party product page):** [claude.com/product/claude-security](https://claude.com/product/claude-security)
- **Coverage:** [SiliconANGLE — Anthropic announces Claude Security public beta](https://siliconangle.com/2026/04/30/anthropic-announces-claude-security-public-beta-find-fix-software-vulnerabilities/) · [SecurityWeek](https://www.securityweek.com/anthropic-unveils-claude-security-to-counter-ai-powered-exploit-surge/) · [The New Stack](https://thenewstack.io/anthropics-claude-security-beta/) · [Inc.](https://www.inc.com/chloe-aiello/anthropics-powerful-new-cybersecurity-tool-is-designed-to-find-vulnerabilities-in-your-code-and-patch-them/91338485) · [Help Center: Use Claude Security](https://support.claude.com/en/articles/14661296-use-claude-security)
- **Published:** 2026-04-30
- **Summary:** Anthropic moved its Claude Code Security research preview (Feb 2026) into public beta as a standalone product, "Claude Security," powered by **Opus 4.7**. It traces data flows across an entire codebase like a security researcher rather than pattern-matching, generates patches, and now supports **scheduled recurring scans**. Available to **Claude Enterprise** customers today; Team and Max "coming soon." Launch partners integrating Opus 4.7 into their platforms: **CrowdStrike, Palo Alto Networks, SentinelOne, Trend Micro (TrendAI), Wiz**.
- **Why it matters:** This is the *defensive* counterpart to Mythos and the first generally-purchasable artifact of Anthropic's cyber stack. For applied teams it gives a sanctioned route to put Opus 4.7 on a security backlog without standing up your own Claude Code agent harness — and the partner list is the first concrete evidence that the major EDR/CSPM vendors are repackaging Anthropic models inside their products.

### 1M-token context window beta retired on Sonnet 4 / Sonnet 4.5
- **Source:** [Migrate Before April 30, 2026 (writeup)](https://pasqualepillitteri.it/en/news/1451/anthropic-1m-context-beta-retirement-april-30-2026) · [Claude API context-windows docs](https://platform.claude.com/docs/en/build-with-claude/context-windows) · [Claude Platform release notes](https://platform.claude.com/docs/en/release-notes/overview)
- **Effective date:** 2026-04-30
- **Summary:** The 1M-token beta header (`context-1m-2025-08-07`) is retired today for `claude-sonnet-4` and `claude-sonnet-4-5`. Requests with the header continue to be accepted syntactically but the window silently falls back to **200K**. Background: 1M was promoted to GA on Sonnet 4.6 / Opus 4.6 at standard pricing on **March 13, 2026**, which made the older betas redundant.
- **Why it matters:** If you're still pinned to Sonnet 4 or 4.5 for cost or eval-stability reasons and were relying on the 1M beta, you have a silent regression starting today — no error, just a truncated context. The only GA 1M alternatives are Sonnet 4.6 (same $3/$15 as 4.5) or Opus 4.6 (~67% premium). Worth grepping codebases for the beta header.

---

## Claude Code

### v2.1.119 / v2.1.120 — eight regressions, v2.1.120 pulled
- **Sources:** [Survival checklist gist (yurukusa)](https://gist.github.com/yurukusa/a866b4cd2976486156a00c190c39cef6) · [Releasebot — claude-code April 2026](https://releasebot.io/updates/anthropic/claude-code) · [npm versions](https://www.npmjs.com/package/@anthropic-ai/claude-code?activeTab=versions) · [Changelog](https://code.claude.com/docs/en/changelog)
- **Window:** filings 2026-04-24 → ongoing through 2026-04-30
- **Summary:** Within ~24 hours v2.1.119 and v2.1.120 shipped eight regressions: auto-update break, silent model swap, `--resume`/`--continue` crash, UI duplication, WSL2 `/mcp` freeze, `CLAUDE.md`-ignored regression, broken `sandbox.excludedCommands`, macOS worktree hang. Auto-update is rolling affected clients **back from 2.1.120 → 2.1.119**. v2.1.119 itself did add useful items: `/config` settings persisted to `~/.claude/settings.json` with project/local/policy override precedence, a `prUrlTemplate` setting, `CLAUDE_CODE_HIDE_CWD`, `--from-pr` support for **GitLab MR / Bitbucket PR / GitHub Enterprise**, and PowerShell auto-approval matching Bash.
- **Why it matters:** Second visible quality wobble in two weeks (postmortem was April 23). If you depend on `--resume` or worktrees on macOS, pin to **2.1.117** until 2.1.121 ships. The `--from-pr` GitLab/Bitbucket/GHE support is the headline new capability worth knowing about for non-GitHub shops.

### "HERMES.md" in commit messages routes to Extra Usage billing — viral on HN
- **Source:** [GitHub issue #53262](https://github.com/anthropics/claude-code/issues/53262) · [HN thread (1,227 pts / 515 comments)](https://news.ycombinator.com/item?id=47952722) · [GIGAZINE writeup](https://gigazine.net/gsc_news/en/20260430-hermes-claude-code/) · [AIToolly](https://aitoolly.com/ai-news/article/2026-04-30-claude-code-bug-hermesmd-in-commit-messages-triggers-unexpected-extra-usage-billing)
- **HN posted:** 2026-04-29 18:54 UTC; still on the front page through April 30.
- **Summary:** A case-sensitive substring match on `HERMES.md` inside a git commit message causes Claude Code v2.1.119 to bypass plan quotas and route the request to **Extra Usage** (paid) billing. One report: $200 of Extra Usage credits drained while the underlying Max 20x plan sat at 13% utilization. Issue is open; no postmortem yet.
- **Why it matters:** Live, unpatched billing footgun that triggers from *commit-message text* — i.e. data the user routinely doesn't audit. Until fixed, audit any project where commits or PRs might mention `HERMES.md`. This and v2.1.120 are stacking on top of an already-tense quality narrative; expect another postmortem.

### `ANTHROPIC_API_KEY` breaks Claude Code in cloud environments
- **Source:** [GitHub issue #54497](https://github.com/anthropics/claude-code/issues/54497) · [HN ask thread](https://news.ycombinator.com/item?id=47966935)
- **Posted:** 2026-04-30 19:11 UTC
- **Summary:** If `ANTHROPIC_API_KEY` is set in a cloud env (e.g. for tests that look for it), Claude Code "fails to do anything." Reporter additionally suspects this contributes to the unexplained Extra Usage flagged elsewhere.
- **Why it matters:** Lots of CI configs export `ANTHROPIC_API_KEY` as a default — this collides with Claude Code's auth selection logic. Workaround: rename the var (e.g. `MY_APP_ANTHROPIC_KEY`) for non-Claude-Code consumers in the same shell.

### "Trust boundaries" research write-up: Claude Code vs Gemini CLI
- **Source:** [HN post](https://news.ycombinator.com/item?id=47963671) · [Writeup repo](https://github.com/kunn007/claude-code-trust-boundaries)
- **Posted:** 2026-04-30 15:07 UTC
- **Summary:** Researcher reported two related findings about repo-level config influencing agent permissions in non-interactive mode. **Google rated the parallel Gemini CLI behavior CVSS 10.0 and patched it; Anthropic classified the Claude Code behavior as working as designed**, on the rationale that non-interactive mode delegates the trust decision to the automation caller (similar to Make/npm/Cargo).
- **Why it matters:** If you're running Claude Code headlessly in CI against untrusted repos (forks, Dependabot PRs, third-party contributors), Anthropic's stance means *you* own that boundary — repo config can expand permissions. Two vendors have now drawn this line in opposite places, which is itself worth tracking.

---

## Research

Nothing new today. The most recent first-party publication, "Evaluating Claude's bioinformatics research capabilities with BioMysteryBench" ([anthropic.com/research/Evaluating-Claude-For-Bioinformatics-With-BioMysteryBench](https://www.anthropic.com/research/Evaluating-Claude-For-Bioinformatics-With-BioMysteryBench)), was published **2026-04-29** and covered in yesterday's briefing. No follow-up post on `alignment.anthropic.com` or `transformer-circuits.pub` in the last 24 hours.

---

## Rumors & Leaks / Policy

### Update on Mythos: NSA is testing it against Microsoft software
- **Source:** [Bloomberg — NSA testing Anthropic's Mythos to find flaws in Microsoft tech](https://www.bloomberg.com/news/articles/2026-04-30/nsa-testing-anthropic-s-mythos-to-find-flaws-in-microsoft-tech)
- **Published:** 2026-04-30
- **Summary:** NSA officials studying Mythos have reportedly been "impressed by its speed and efficiency in searching for potential security flaws" in widely-deployed software, including Microsoft products.
- **Why it matters:** Concrete confirmation that Mythos has a USG defensive customer; pairs with the Glasswing consortium picture from earlier this month and reframes the "no public release" stance as *defense-aligned access*, not *no access*.

### Update on Mythos: White House opposes Anthropic's expansion plan to ~70 organizations
- **Source:** [WSJ — White House Opposes Anthropic's Plan to Expand Access to Mythos](https://www.wsj.com/tech/ai/white-house-opposes-anthropics-plan-to-expand-access-to-mythos-model-dc281ab5) · [Bloomberg](https://www.bloomberg.com/news/articles/2026-04-30/white-house-opposes-anthropic-plan-for-mythos-access-wsj-says) · [CNN](https://www.cnn.com/2026/04/30/us/video/the-white-house-is-reportedly-working-around-its-own-restrictions-on-ai-company-anthropic-cnc) · [Bloomberg explainer](https://www.bloomberg.com/news/articles/2026-04-30/mythos-why-anthropic-s-ai-model-is-sparking-global-alarm)
- **Published:** 2026-04-30
- **Summary:** Members of the Trump administration told Anthropic they don't agree with the company's plan to grant Mythos access to roughly 70 companies and organizations. CNN separately reports the White House is "actively working on ways to get around its own restrictions on the AI company" — i.e. the existing Anthropic blacklist.
- **Why it matters:** The Mythos rollout is now an active interagency political fight, not just a safety story. For practitioners this affects the realistic timeline for any wider Mythos / Glasswing access — likely longer.

### Update: Trump officials drafting executive plan to bring Anthropic back
- **Sources:** [Axios — Trump officials draft plan to bring Anthropic back amid Pentagon fight](https://www.axios.com/2026/04/29/trump-anthropic-pentagon-ai-executive-order-gov) · [HN post](https://news.ycombinator.com/item?id=47953120)
- **Published:** 2026-04-29
- **Summary:** Axios reports a draft executive-order workaround to lift restrictions on Anthropic in DoD contexts. Companion CNBC story (April 28, edge of window) confirms DoD has expanded Gemini usage to fill the gap, which is the leverage Anthropic loses if the workaround fails.
- **Why it matters:** Anthropic's federal access posture is now in motion week-over-week. If you ship Anthropic-backed products into FedRAMP or DoD-adjacent customers, this is the file to track.

### Critique: "Anthropic's argument for Mythos SWE-bench improvement contains a fatal error"
- **Source:** [philosophicalhacker.com](https://www.philosophicalhacker.com/post/anthropic-error/) · HN: [item 47953026](https://news.ycombinator.com/item?id=47953026)
- **Published:** 2026-04-29
- **Summary:** Independent critique arguing the headline SWE-bench delta in Anthropic's Mythos comms is methodologically flawed. Low engagement (2 points on HN) but the first substantive technical pushback on Mythos's marquee benchmark claim.
- **Why it matters:** If you're citing Mythos benchmark numbers in internal pitches or evals, read this before you do.

---

## Other (business / community)

### Anthropic in talks for a $50B round at $850–900B valuation
- **Source:** [TechCrunch — Sources: Anthropic could raise a new $50B round at $900B](https://techcrunch.com/2026/04/29/sources-anthropic-could-raise-a-new-50b-round-at-a-valuation-of-900b/) · HN: [47956591](https://news.ycombinator.com/item?id=47956591), [47963966](https://news.ycombinator.com/item?id=47963966)
- **Published:** 2026-04-29
- **Summary:** Anthropic has received multiple preemptive offers for ~$50B at $850–900B post-money. Decision expected at the May board meeting. Separate HN post (April 30) flags Anthropic has overtaken OpenAI on secondary markets.
- **Why it matters:** Sets the price ceiling on AI infra deals for Q2; signals the cash runway behind the 5GW Amazon compute deal and the $40B Google deal previously reported.

### "Anthropic wants to be the AWS of agentic AI"
- **Source:** [The New Stack](https://thenewstack.io/anthropic-agents-managed-aws-claude/) · HN: [47966190](https://news.ycombinator.com/item?id=47966190)
- **Posted:** 2026-04-30
- **Summary:** Synthesis piece tying together Project Deal (the Claude-run marketplace experiment, April 24), the Managed Agents engineering posts, and Glasswing as the strategic shape of "buy agents from Anthropic."
- **Why it matters:** Useful one-link summary if you need to brief a less-technical stakeholder on where Anthropic's product direction is going beyond the model itself.

### Show HN: Pu.sh — 400-line shell coding-agent harness
- **Source:** [HN front-page item 47968112](https://news.ycombinator.com/item?id=47968112) · [pu.dev](https://pu.dev/)
- **Posted:** 2026-04-30 20:55 UTC, 65+ points and climbing.
- **Summary:** A 400-line POSIX shell coding-agent harness — supports Anthropic + OpenAI APIs, 7 tools (bash, read, write, edit, grep, find, ls), REPL, auto-compaction, checkpoint/resume, pipe mode, 90 no-API tests. Author credits Pi (pi.dev) for the architecture and notes "Pi/Claude/Codex wrote the awk."
- **Why it matters:** Useful reference for *minimum-viable* agent harness shape and tool surface. Reasonable template if you want a tiny portable agent inside CI containers without npm.

### HN community discussion: rethinking Claude Code usage post-postmortem
- **Source:** [HN ask thread 47957402](https://news.ycombinator.com/item?id=47957402)
- **Posted:** 2026-04-30 02:34 UTC
- **Summary:** Long Ask-HN write-up arguing the right mental model for Claude Code is "cost-per-output of an engineer," not "stay under the cap." Concrete tactics: pick model deliberately (Opus for arch decisions, Sonnet for code), use `/effort` levels, three reusable prompt patterns (give the model an out, invert speed bias, make it write back to `claude.md`/skills), and a two-agent spec-review/code-review split.
- **Why it matters:** Concrete operational patterns from a practitioner — easy to lift into your own CLAUDE.md.

### HN ask: prepaid Extra Usage credits vanished on Pro lapse
- **Source:** [HN 47961574](https://news.ycombinator.com/item?id=47961574)
- **Posted:** 2026-04-30 12:44 UTC
- **Summary:** User reports $40 of prepaid Extra Usage credits disappeared when their Pro subscription expired. Anthropic docs tie Extra Usage to an active Pro plan but don't spell out the cancellation/expiry behavior; no warning, no payout flow.
- **Why it matters:** Pairs with the HERMES.md billing bug as the second active "money disappears" complaint of the day. If you administer team Claude billing, audit Extra Usage balances before any plan changes.

---

## Excluded due to recency filter (flagged for follow-up)

- **"Claude for Creative Work"** — anthropic.com post (HN [47942386](https://news.ycombinator.com/item?id=47942386), 152 pts) was posted **2026-04-28 23:46 UTC**, just outside the 24-hour window. Worth digging into tomorrow if it lingers on the front page.
- **"Long-running Claude for scientific computing"** — anthropic.com/research, posted to HN April 27. Outside window.
- **Anthropic Project Deal** (Claude-run marketplace experiment) — original post April 24, multiple HN reposts through April 27. Outside window; flagged in yesterday's briefing.
- **Steve Blank — "Anthropic Mythos: We've Opened Pandora's Box"** (steveblank.com, April 28) — outside 24-hour window.

---

*Generated 2026-04-30. Recency cutoff: items dated 2026-04-29 ~00:00 UTC or later. Sources prioritized: anthropic.com / claude.com / docs.claude.com / Anthropic GitHub → Hacker News front-page items → established AI/tech press.*
