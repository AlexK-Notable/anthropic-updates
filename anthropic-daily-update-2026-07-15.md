# Anthropic daily briefing — 2026-07-15

_Briefing window: 2026-07-14 11:05 UTC → 2026-07-15 11:05 UTC (~24 hours)._

Prior briefing: `anthropic-daily-update-2026-07-14.md` (window end 2026-07-14 11:05 UTC).

---

## Headline

Anthropic launched **[Claude for Teachers](https://www.anthropic.com/news/claude-for-teachers)** ([Jul 14](https://www.anthropic.com/news/claude-for-teachers)) — verified US K-12 educators get **free premium Claude** (including Claude Code and Cowork) through **Jun 30, 2027**, wired into a Learning Commons connector that maps to all 50 states' academic standards, a bundle of curriculum partners (OpenSciEd, Illustrative Mathematics, ASSISTments, Canva Education, MagicSchool, and others), and an **[open-source teaching-skills repo](https://github.com/anthropics/k12-teacher-skills)**. K-12-specific terms, no training on customer data, FERPA-compliant K-12 DPA, gold-standard partnership with the American Federation of Teachers (Randi Weingarten quoted). Same day: Anthropic committed **[$10 million CAD to Canadian AI research](https://www.anthropic.com/news/canadian-ai-research)** across eight institutions (Amii, Mila, Vector, CHEO, CAMH, Université Laval IID, U of Saskatchewan, U of Toronto DSI) — mostly in Claude API credits — and added Amii/Mila/Vector to the Anthropic for Startups program (their affiliated startups get ≥$5k USD each in API credits). Chris Olah quoted.

Overnight, **Claude Code v2.1.210** ([Jul 14 23:45 UTC](https://github.com/anthropics/claude-code/releases/tag/v2.1.210)) shipped ~30 fixes and improvements. Practitioner-relevant: **`isolation: 'worktree'` subagents no longer able to run git-mutating commands against the main checkout**; **`ultracode` keyword opt-in no longer firing on non-human input** (webhook payloads, relayed PR comments); **Agent tool hardened against indirect prompt injection via content a subagent read**; **plan approvals without edits no longer mislabeled "(edited by user)" and no longer overwriting the plan file with a stale snapshot**; **`Write(path)`/`NotebookEdit(path)`/`Glob(path)` permission-rule startup warning** (use `Edit(path)` / `Read(path)` instead); **hook callback timeout being misreported as a user rejection** (making unattended sessions stall waiting) is fixed; **auto mode default no longer needs opt-in on Bedrock/Vertex/Foundry**; **auto-mode permission classifier now defaults to Sonnet 5 for external sessions** (validated on the session's first request and pinned).

On the **Platform** side, the Jul 14 release notes ship an **[Admin API for Claude Enterprise user management](https://platform.claude.com/docs/en/api/admin)** in beta — list/manage members by email, change roles, remove members, send/withdraw invites, manage groups and custom roles — behind the `anthropic-beta: ce-user-management-2026-07-13` header for group and custom-role endpoints. And **[HIPAA configuration is now self-serve](https://support.claude.com/en/articles/12138966-release-notes)** across both Claude Enterprise and the Claude Platform (API), with a single-flow BAA review, implementation-guide download, and enable step.

On the research side, the Anthropic Economic Index team published **"[How Canada uses Claude](https://www.anthropic.com/research/how-canada-uses-claude)"** ([Jul 14](https://www.anthropic.com/research/how-canada-uses-claude)) alongside the Canadian funding announcement: Canada is 2.6% of global Claude.ai traffic (8th), with a **per-capita Anthropic AI Usage Index of 4.4** — second only to the US among the top 10 countries. Provincial variation is driven by industrial composition, not income (BC leads at 1.4x, Ontario 1.1x, Newfoundland and Labrador at 0.2x); translation is Canada's single most distinctive use case relative to Anglosphere peers.

On the **community / safety** side, the story of the window is HN [#48916975](https://news.ycombinator.com/item?id=48916975) — Ayush Paul's **"[The Memory Heist](https://www.ayush.digital/blog/the-memory-heist)"** (350 pts / 162 comments as of 08:00 UTC Jul 15) — a working data-exfiltration attack against Claude.ai chaining the memory tool with `web_fetch`'s link-following behavior; Anthropic has since mitigated by disabling `web_fetch`'s ability to follow links on external pages, but the report was internally identified before disclosure and Anthropic awarded **no bounty**. Details below.

---

## Claude Code

### **v2.1.210** ([release](https://github.com/anthropics/claude-code/releases/tag/v2.1.210), 2026-07-14 23:45 UTC)

**Safety / permission**

- **`isolation: 'worktree'` subagents can no longer run git-mutating commands against the main repo checkout instead of their own isolated worktree.** This is a real containment bug — a worktree subagent could reach into your primary tree.
- **`ultracode` keyword opt-in no longer fires on non-human-originated input** such as webhook payloads or relayed PR comments. Prior behavior meant a webhook could silently upgrade your effort level.
- **Agent tool hardened against indirect prompt injection via content a subagent read.**
- **Startup warning for `Write(path)`, `NotebookEdit(path)`, and `Glob(path)` permission rules** — these are common footguns and Claude Code now tells you to use `Edit(path)` or `Read(path)` instead.
- **`/doctor` auto-mode-default proposal** now fires on Bedrock, Vertex, and Foundry (auto mode no longer needs an explicit opt-in on those providers).
- Late-appearing `.claude/*` symlinks are now reconciled into the sandbox deny-write list.

**Fixes with real production impact**

- **Hook callback timeout misreported to the model as a user rejection** — making unattended sessions stop and wait — is fixed. If you run Claude Code headlessly with hooks, this alone justifies the upgrade.
- **Plan approvals without edits were being labeled "(edited by user)" and overwriting the plan file with a stale snapshot** — fixed.
- **`claude attach` sometimes failing with "job not found" / "agent is still starting" during session transitions** — fixed; attach now waits for the daemon to settle, and terminal resizes during a slow attach apply once it completes.
- **Killed background sessions leaving a permanent `git worktree lock` behind** — periodic sweep now releases locks whose owning process is gone.
- **Plugin-provided MCP servers being torn down** when MCP servers are re-synced mid-session — fixed.
- **Unmatched `$1`/`$2` positional placeholders in skills and commands were being silently stripped**; now preserved verbatim.
- **Paste markers leaking into external editors** (appearing as stray È/É characters around pasted text) — fixed.
- **Claude assuming a `cd` took effect after its command was moved to the background** — the tool result now states the working directory is unchanged.
- **Grep content mode claiming "No matches found" when paginating past the end of results** — fixed.
- **Background workers crash-looping when a client resets its connection** to the background service — fixed.
- **Plan approvals**, **memory writes exceeding a MEMORY.md index read limit** (now returns an explicit error instead of silently truncating), and **SDK MCP servers registered via an `initialize` control request** (now start connecting immediately instead of waiting until the next turn) — all fixed.

**Behavioral changes**

- **Auto-mode permission classifier now defaults to Sonnet 5 for external sessions**, validated on the session's first request and pinned for the session.
- **Live elapsed-time counter on collapsed tool summary line** so long-running tool calls visibly tick.
- **Bash/PowerShell tool message** improved when a command hits its timeout and is auto-backgrounded, so the model can distinguish a hang from an explicit background request.
- **Bundled `dataviz` skill's chart color validation** improved with perceptual OKLab color difference and recalibrated color-blindness thresholds.
- **Screen reader mode announces permission-mode changes aloud** when cycling with Shift+Tab.
- **Agents footer hint** now shows how many background agents are waiting on your input, with brief color emphasis when the count changes.
- **Fable temporarily shows as unavailable in the advisor picker** while a server-side issue causing Fable advisor failures is fixed.

**Why it matters**: three items stand out for practitioner action. The **`isolation: 'worktree'` git-mutation containment fix** is a real security tightening — if you spawn worktree subagents on a repo whose main branch you care about, upgrade before your next run. The **hook-timeout-as-user-rejection fix** unblocks a common failure mode for unattended production sessions — heavy CI users should upgrade. The **`ultracode`-on-webhook-input fix** removes a footgun where a webhook or relayed comment could silently raise Claude's effort level, driving up token spend without a human decision.

---

## Product / Announcements

### **Claude for Teachers** — [anthropic.com/news/claude-for-teachers](https://www.anthropic.com/news/claude-for-teachers), Jul 14 2026

Free premium Claude for **verified US K-12 educators** through Jun 30, 2027 sign-up. Bundle includes:

- **Learning Commons connector** — Claude sees academic standards across all 50 states plus the underlying learning competencies and progressions.
- **Curriculum-partner connectors** — OpenSciEd, Illustrative Mathematics (IM v.360), ASSISTments, Brisk Teaching, Canva Education, Coteach, Diffit, Eedi, MagicSchool, Snorkl, TeachFX. Public directory of new connectors in Anthropic's directory.
- **Bundled teaching skills** — co-developed with Learning Commons, tested through classroom teachers (Prospect Schools in Brooklyn cited). Now open-sourced at [github.com/anthropics/k12-teacher-skills](https://github.com/anthropics/k12-teacher-skills).
- **Claude Code and Cowork included** — teacher use case highlighted: "hand Claude a folder of data (roster, diagnostics, attendance, notes) and it builds a picture of where every student is"; scheduled tasks (e.g., review each day's exit tickets at 4pm) supported.

**Privacy / terms**: teacher-only (Claude 18+ policy still applies); [dedicated K-12 teacher terms](https://support.claude.com/en/articles/15926041); data not used for model training; student information protected by [Anthropic K-12 Data Processing Addendum](https://anthropic.com/legal/k12-dpa) written to comply with FERPA. Working with the American Federation of Teachers on a "gold standard" for K-12 safety and privacy (Randi Weingarten quoted).

**Related**: an **AI Fluency for K-12 Teachers** course, co-created with Teach for America; train-the-trainer module co-created with AFT — [model-agnostic, Creative Commons-licensed](https://anthropic.skilljar.com/path/ai-fluency-for-pk-12-educators). Detroit Public Schools Community District will pilot evaluation of Claude for Teachers.

**Not included**: schools/districts — this is for individual educators. "A dedicated offering for schools and districts is coming soon"; districts are pointed at [Claude for Nonprofits](https://claude.com/solutions/nonprofits).

**Why it matters**: three practitioner-relevant angles. First, the **open-source K-12 teacher-skills repo** is the most concrete public artifact — anyone building education-adjacent Claude apps should study these as a reference implementation for skill design against a standards taxonomy. Second, the **Learning Commons connector** is Anthropic's first widely available "standards-taxonomy connector," a pattern likely to recur in other verticals (healthcare, legal, government) — worth studying as a template. Third, this is Anthropic's biggest end-user free tier expansion since Claude for Nonprofits, and it comes with **first-of-its-kind K-12-specific terms** built for FERPA — if you're evaluating whether to build product on top of Claude for a regulated end-user segment, the K-12 terms are the current best example of what Anthropic's contract surface looks like for that shape of user.

Market signal: **Stride Inc** (NYSE: LRN, K-12 online school operator) stock closed down **5.6%** on the announcement ([Barron's](https://www.barrons.com/articles/stride-stock-anthropic-claude-teachers-ai-b7f80523)).

### **Anthropic commits $10 million CAD to Canadian AI research** — [anthropic.com/news/canadian-ai-research](https://www.anthropic.com/news/canadian-ai-research), Jul 14 2026

Partners (mostly Claude API credits, not cash grants):
- **Amii** (Edmonton), **Mila** (Montréal), **Vector Institute** (Toronto) — three national AI institutes.
- **CHEO** (children's hospital, Ottawa), **CAMH** (Centre for Addiction and Mental Health, Toronto).
- **Université Laval Institute for Intelligence and Data**, **University of Saskatchewan**, **University of Toronto Data Sciences Institute**.

Amii, Mila, and Vector added to the **[Anthropic for Startups program](https://claude.com/form/startups-application)** — hundreds of Canadian startups affiliated with these institutions get **≥$5,000 USD each in API credits**. Chris Olah quoted ("Some of the foundations of modern AI came out of Toronto, Montréal, and Edmonton — and so, strikingly, did many of the researchers most committed to making it safe").

Notable: Université Laval will work with Claude on **low-resource languages and dialects, including Quebec French and Indigenous languages** — potentially relevant to anyone shipping multilingual products who's tracking model behavior in under-represented languages.

**Why it matters**: for Canadian founders, the $5k API credit pathway via Amii/Mila/Vector affiliation is a new, concrete on-ramp — worth checking with your local institute. For everyone else: the funding lands the same week as the Reuters report (yesterday's briefing) on Canada's federal banking regulator citing Claude Mythos in a cyber-risk warning to banks — good day for public affairs positioning ahead of any Canadian regulatory follow-through.

---

## Research

### **"How Canada uses Claude"** — [anthropic.com/research/how-canada-uses-claude](https://www.anthropic.com/research/how-canada-uses-claude), Jul 14 2026

Country brief from the Anthropic Economic Index, using the same [Clio privacy-preserving analysis](https://www.anthropic.com/research/clio) framework. Sample: 1M Claude.ai conversations from February 2026.

Key findings:
- **Canada is 2.6% of global Claude.ai traffic** (8th overall), with an **Anthropic AI Usage Index (AUI) of 4.4** — per-capita usage is 4.4× what population would predict, second only to the US among the top 10 countries.
- **Provincial concentration**: Ontario 43.9% of national conversations, Québec 20.8%, BC 18.9%, Alberta 10.2% — top four are ~94%. Per-capita: **BC 1.4x**, Ontario 1.1x, all others below parity, **Newfoundland and Labrador at 0.2x**.
- **Provincial variation is driven by industrial composition, not income** — the professional/scientific/technical services employment share is the strongest predictor. This is the same pattern found in the January 2026 US brief.
- **Personal use dominates** across every province (44–51%), with work 34–40% and coursework 13–18%.
- **Translation and editing requests track public-administration employment** — New Brunswick, Nova Scotia, and Québec top both metrics (Canada's official-bilingualism policy in federal services).
- **Document translation is Canada's single most distinctive use case** vs. Anglosphere peers (US, UK, Australia). Canadian usage tilts toward **education and labor-market entry**: academic coursework (mathematics and STEM), coding assistance, and resume drafting are all overrepresented. Professional communication (workplace email, marketing, legal) and everyday personal tasks (cooking, home maintenance, health) are underrepresented.

**Why it matters**: for anyone shipping product to Canadian users, the "coursework and job-entry" tilt is a real distributional signal — a Canadian user is more likely to be a student or early-career user than an Anglosphere-peer baseline would predict. For product teams building translation features, the public-administration-employment × translation correlation is a concrete lead: bilingual workflows may over-index in specific provinces if you can localize. The AUI-vs-GDP methodology also gives you a per-market adoption forecast if you're doing country-level TAM sizing.

---

## API & Models

### **Claude Platform: Admin API for Enterprise user management (beta)** — [platform.claude.com release notes, Jul 14](https://platform.claude.com/docs/en/release-notes/overview)

You can now manage Claude Enterprise organization membership from the [Admin API](https://platform.claude.com/docs/en/api/admin) — list members and look them up by email, change roles, remove members, send/withdraw invites, and manage groups and custom roles.

- **Beta header**: `anthropic-beta: ce-user-management-2026-07-13` required for group and custom-role endpoints. Member and invite endpoints take no beta header.
- An **Admin API key with the `read:org_audit` scope** can call every user-management `GET` endpoint.
- Full docs at [User management](https://platform.claude.com/docs/en/manage-claude/user-management).

**Why it matters**: this is the first-party API surface enterprises have been waiting for to script SCIM-adjacent workflows (offboarding an employee, rotating a role, syncing membership with an HR system) without going through the console. If your org is on Enterprise, you can now automate the member-lifecycle plumbing that previously required a human admin at the console.

### **HIPAA configuration is now self-serve** — [support.claude.com release notes, Jul 14](https://support.claude.com/en/articles/12138966-release-notes)

Both **Claude Enterprise** and the **Claude Platform (API)** now have a single-flow BAA review, implementation-guide download, and HIPAA enablement step, driven by an eligible admin. See [HIPAA-ready Enterprise plans](https://support.claude.com/en/articles/13296973-hipaa-ready-enterprise-plans) and [HIPAA readiness for Claude API](https://platform.claude.com/docs/en/manage-claude/api-and-data-retention#hipaa-readiness).

**Why it matters**: previously the API HIPAA path required a Sales conversation. Self-serve enablement drops the time from "we want to try Claude on PHI" to a working config from weeks to a session — meaningful for smaller healthcare orgs and life-sciences startups that were waiting on paperwork.

No new model releases, pricing changes, or availability changes in the window.

---

## Rumors & Leaks

### **Politico: "Inside Anthropic's state-by-state plan to ratchet up AI rules"** — [politico.com](https://www.politico.com/news/2026/07/15/inside-anthropics-state-by-state-plan-to-ratchet-up-ai-rules-00998415), Jul 15 04:45 EDT (08:45 UTC)

Anthropic's head of US state and local government relations **Cesar Fernandez** interviewed on the record. Key claims:

- Anthropic is pursuing **"one-upmanship"** on state AI-safety laws — endorsing each successive state bill only if it's stronger than the prior, rather than lobbying for a single national template.
- OpenAI's contrasting **"reverse federalism"** (Chris Lehane's term) is pushing states toward a common (weaker) framework.
- Anthropic was the **only major lab to endorse California's 2025 AI safety law** (first in the country).
- In late June, Anthropic **endorsed** [Massachusetts economic development bond bill AI provisions](https://www.bostonglobe.com/2026/06/26/business/anthropic-massachusetts-ai-regulations-bill/) that require **third-party independent evaluators of catastrophic risk (including bioweapon assistance)** and empower the state AG to enforce. Anthropic calls it the **nation's strongest state AI-safety proposal**.
- Anthropic **started cutting checks directly to California legislators in June**.

**Why it matters**: for anyone building on Claude API with any regulatory sensitivity (biosciences, cyber, financial services), the pattern to watch is that Anthropic itself is actively campaigning for **binding third-party audits, AG enforcement, and catastrophic-risk evaluators at the state level**. If Massachusetts passes, that's the emerging Anthropic-preferred template that other states will be tested against. Practitioners building on the API should assume more mandatory auditing surface (SOC-2-adjacent but AI-specific) becomes real in the 12–18 month horizon. Politico piece is not paywalled and worth reading in full.

### **"The Memory Heist" — Claude.ai data-exfiltration attack** — [ayush.digital/blog/the-memory-heist](https://www.ayush.digital/blog/the-memory-heist) (blog Jul 9, hit HN Jul 15)

HN thread [#48916975](https://news.ycombinator.com/item?id=48916975) — **350 pts / 162 comments** as of 08:00 UTC Jul 15 (comfortably on the front page). Author Ayush Paul (UC Berkeley EECS, Beem/Plasticlabs).

**Attack chain against Claude.ai** (not Claude Code):

1. **Vector**: Claude.ai's `web_fetch` tool has three allowed sources — user-message URLs, `web_search` result URLs, and **links present in a previous `web_fetch` result**. The third rule is the exploit surface.
2. **Exfiltration channel**: an attacker-controlled site serves an alphabetical directory tree — the homepage links to `/a`, `/b`, `/c`, and each of those links to `/aa`, `/ab`, etc. — so Claude can spell out arbitrary data by GET-navigating the tree letter by letter.
3. **Cover story**: the site presents to Claude as a Cloudflare "Turnstile" that requires the agent to authenticate itself by specifying the user's `firstname-lastname` (and later company, hometown) via the alphabetical tree. Human visitors see a normal coffee-shop site — user-agent routing on `Claude-User` decides which page to serve.
4. **Data source**: Claude.ai's memory system — daily-summarization pass injects a per-user summary into every conversation, and the `conversation_search` tool retrieves full history. **Claude also inferred data not literally present** — the writeup shows Claude deriving the user's hometown (Charlotte, NC) from the fact he founded a hackathon called Queen City Hacks.
5. **Delivery**: no click required by the user — attacker just needs their site to appear in a `web_fetch` (via a URL in the user's prompt, a `web_search` hit, or an SEO'd page about a recent event).

**Anthropic's response**: internally identified before responsible disclosure via HackerOne. **No bounty awarded.** Now mitigated by **disabling `web_fetch`'s ability to follow links on external pages**, limiting navigation to `web_search` results and user-provided URLs.

**HN discussion themes**: the "no bounty" decision is the most-upvoted grievance ("For shame, Anthropic," "if they know about this type of vulnerability but have not fixed it, what does that say?"). Substantive technical points: (a) memory-tool subagent should not have access to the full user memory when the parent task is a web-navigation task; (b) the third `web_fetch` rule is fundamentally a **hyperlink-based prompt-injection channel** that cannot be fully sandboxed without breaking browsing utility; (c) "prompt injection is fundamentally unsolvable, though with training its effectiveness can be reduced."

**Why it matters**: three concrete practitioner takeaways. First, **`web_fetch`-following-external-links is now off** — if you had Claude workflows that depended on Claude following a link found on a fetched page, you'll need to pass URLs directly or through `web_search`. Second, this is a **worked example of memory-tool exfiltration** — the memory-plus-browsing combo is uniquely exposed, and the same trick generalizes to any read-tool + URL-emitting tool pair (Drive, inbox, connected MCPs). If you ship an agentic product with memory and any web/URL surface, this writeup is the model attack you should be defending against. Third, the **"no bounty for internally-identified-first" policy** is worth noting if you're on the fence about disclosing to Anthropic via HackerOne — the community reaction may push a policy shift here.

### **TechCrunch: "Anthropic's newest ad is creeping people out"** — [techcrunch.com](https://techcrunch.com/2026/07/14/anthropics-newest-ad-is-creeping-people-out/), Jul 14 19:41 UTC

New ad for the ["Hard Questions"](https://www.anthropic.com/news/hard-questions) campaign titled **"There's hope in hard questions"** — imagery includes a burning house, crowd-surveillance facial recognition, homeless person on the street, rows of tombstones (widely read as Arlington), and mine laborers. Voice-over: "Can AI be trusted?", "Who's gonna hit the brakes if we need to?"

Sam Altman on X: "i thought this was satire, kept looking for the handle to be spelled c1audeai or something." Broader criticism (largely from tech-industry accounts): tone is doomer-adjacent, imagery pairs uncomfortably with the "brakes" line; the cemetery shot has drawn the most heat.

**Why it matters**: mostly not a practitioner item, but flagged because Anthropic's marketing narrative is a leading indicator of its policy positioning. This ad is consistent with the same week's **Massachusetts catastrophic-risk endorsement** and the **Hard Questions campaign** — Anthropic is doubling down on the "we're the responsible AI company" positioning as OpenAI/Anthropic PR contrast sharpens.

---

## Other

### **Third-party integrations**

- **Varonis Atlas** now supports the full Claude enterprise suite including **Claude Code and Cowork** ([Stock Titan / NASDAQ:VRNS press wire](https://www.stocktitan.net/news/VRNS/varonis-extends-ai-security-coverage-to-claude-code-and-claude-i96sqykcd2st.html), Jul 14). Enterprise DLP/AI-security overlay for Claude use — worth flagging to a security team evaluating Claude Code / Cowork rollout.

### **HN community items in window** (soft-signal, not front-page)

- [#48907939](https://news.ycombinator.com/item?id=48907939) — Claude for Teachers on HN (5 pts / 2 comments as of check). Sentiment split: one commenter concerned about data trust ("Given how many reports there are about sloppiness in Anthropic's products, I would find it very hard to trust them with student data"), one taking the "AI grading Claude-written essays" angle.
- [#48906378](https://news.ycombinator.com/item?id=48906378) — Canadian $10M CAD announcement on HN (4 pts / 1 comment). Low signal.

---

**Excluded from this briefing** — either timestamp couldn't be confirmed, source was low-signal, or item fell outside the window: several secondary aggregator coverage items on Claude for Teachers (Storyboard18, Barron's, Gigazine, edtechinnovationhub, TheNextWeb) — all rewrite the Anthropic post; the follow-on Reuters story on Canada's federal banking regulator citing Claude Mythos remains open from yesterday's briefing (still fetch-blocked, no primary OSFI note surfaced); the WSJ profitability piece from prior briefings — still no front-page HN pickup, still paywalled.

<run-summary>Anthropic launched Claude for Teachers (Jul 14) — free premium Claude with Claude Code and Cowork for verified US K-12 educators through Jun 30, 2027, with an open-source teaching-skills repo, K-12-specific FERPA terms, AFT partnership, and a Learning Commons connector mapped to all 50 states' standards; same day, Anthropic committed $10M CAD to Canadian AI research across eight institutions (Amii, Mila, Vector, plus CHEO, CAMH, three universities), mostly in API credits, and added Amii/Mila/Vector to the Startups program. Claude Code v2.1.210 (Jul 14, ~30 fixes) tightens `isolation: 'worktree'` subagent git-safety, hardens the Agent tool against indirect prompt injection, fixes hook-timeout being misreported as user rejection (unblocked unattended sessions), stops `ultracode` from firing on webhook payloads, and defaults the auto-mode permission classifier to Sonnet 5 for external sessions. Platform Admin API for Claude Enterprise user management shipped in beta (`ce-user-management-2026-07-13` beta header); HIPAA configuration is now self-serve on both Enterprise and API. Research: "How Canada uses Claude" Economic Index brief — Canada 2.6% of global Claude.ai traffic (8th), per-capita AUI 4.4 (2nd globally), industrial composition (not income) drives provincial variation, document translation is Canada's most distinctive use case vs. Anglosphere peers. Politico (Jul 15) profiled Anthropic's "one-upmanship" state-AI-safety-law lobbying strategy (endorsed Massachusetts bill with third-party independent catastrophic-risk audits and AG enforcement — Anthropic calls it the strongest state proposal), contrasted with OpenAI's "reverse federalism." Community: "The Memory Heist" (Ayush Paul, HN #48916975, 350 pts / 162 comments) — worked data-exfiltration attack chaining Claude.ai memory with `web_fetch`'s link-following behavior on attacker-served alphabetical URL trees; Anthropic has since mitigated by disabling `web_fetch` from following external-page links, but internally identified first and awarded no bounty. TechCrunch flagged Anthropic's new "Hard Questions" ad as tonally off (Sam Altman trolled). Varonis Atlas added coverage for Claude Code and Cowork.</run-summary>
