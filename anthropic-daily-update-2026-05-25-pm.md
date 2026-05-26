# Anthropic daily briefing — 2026-05-25 (afternoon update)

_Briefing window: 2026-05-25 11:07 UTC → 2026-05-25 23:45 UTC (~12.6 hours since the morning briefing on the same day). Audience: applied AI engineer working in the Anthropic ecosystem._

## Headline

**One in-window first-party post**: Anthropic published Chris Olah's remarks on Pope Leo XIV's encyclical "Magnifica humanitas." Policy-PR framing, but contains a notable interp-research attestation ("internal states that functionally mirror joy, satisfaction, fear, grief, and unease") and pulled meaningful HN traction (67 pts / 87 comments). **No new Claude Code releases, SDK releases, platform release notes, status incidents, or research-blog posts in the window.**

---

## Claude Code

**Nothing new.** No new releases, no CHANGELOG entries, no SDK releases since the morning briefing. Latest Claude Code remains v2.1.150 (2026-05-23 04:03 UTC); latest claude-agent-sdk-python v0.2.87 and claude-agent-sdk-typescript v0.3.150 (both 2026-05-23). Verified via `gh api repos/anthropics/claude-code/releases`.

User-reported issue traffic in the GitHub tracker is high today (≥18 issues filed between 11:00 UTC and 23:45 UTC), but no clustered regression pattern that rises to "briefing-worthy" — most look like individual bug reports. One worth a passing glance if you run Sonnet 4.6 on Desktop 2.1.149: [#62314 "Sonnet 4.6 routes every request to long-context tier on Claude Desktop 2.1.149 (429 'Usage credits required')"](https://github.com/anthropics/claude-code/issues/62314), 3 comments, no Anthropic confirmation yet. Treat as anecdotal until corroborated.

## Research

**Nothing new** in the window. `anthropic.com/research` most recent post remains "Project Glasswing: An initial update" (2026-05-22); `alignment.anthropic.com` most recent remains SLEIGHT-Bench. No engineering-blog posts since 2026-04-23.

## API & Models

**Nothing new in the window.**

- `platform.claude.com/docs/en/release-notes/overview` most recent entry remains 2026-05-19 (MCP tunnels research preview, self-hosted sandboxes for Managed Agents, in-session MCP/tool config updates, 100K-token output spillover) — already covered.
- `status.claude.com` shows no new incidents after the Opus 4.7 resolution at 10:39 UTC (covered in the morning briefing). All systems operational at briefing time.
- No new model announcements, pricing changes, or deprecation notices.

## Rumors & Leaks

**Update on the Mythos rumor — The Register picks up the Project Glasswing post and frames it as a public-release signal.**

- **theregister.com — "Anthropic to release Mythos-class models to the public"** ([link](https://www.theregister.com/security/2026/05/25/anthropic-to-release-mythos-class-models-to-the-public/5245596), published 2026-05-25 08:09 UTC, HN submission 20:29 UTC, 13 pts / 2 comments). Re-reads Anthropic's 2026-05-22 "Project Glasswing: An initial update" post and emphasizes the line about working "with critical partners — including US and allied governments — to expand Project Glasswing to additional partners" plus Anthropic's stated intent to eventually release Mythos publicly "once we've developed the far stronger safeguards we need." Cites Mythos's stated scan results (1,000+ open-source projects, 6,202 high/critical vulns).

_What's actually new vs. the morning briefing:_ nothing first-party. This is downstream press on the 2026-05-22 first-party update (which was flagged in the morning briefing's "Excluded but worth a look"). The Register's framing — "Mythos public release is the eventual plan" — is the only delta, and it's their inference from Anthropic's wording, not a fresh Anthropic statement.

_Why it matters for practitioners:_ low new information. Worth knowing because it's the first major IT-press piece treating the public-release framing as the lede; expect more downstream coverage in the next 24–48h. Still no model-string sighting confirmation, no API access, no timeline.

## Other (first-party, third-party tooling, community)

**First-party — "Anthropic co-founder Chris Olah's remarks on Pope Leo XIV's encyclical 'Magnifica humanitas'"** ([anthropic.com](https://www.anthropic.com/news/chris-olah-pope-leo-encyclical), 2026-05-25; HN submission [48270497](https://news.ycombinator.com/item?id=48270497) at 19:12 UTC, 67 pts / 87 comments).

Olah delivered remarks at the Vatican presentation of Pope Leo XIV's AI encyclical. Three points he flagged: (1) AI's effect on the global poor, (2) "moral vision for human flourishing," (3) interpretability research progress. The interp claim is the only line with substantive practitioner content: he says Anthropic's interp team has found _"structures that mirror results from human neuroscience"_ and _"internal states that functionally mirror joy, satisfaction, fear, grief, and unease."_

_Why it matters:_ this is a public, first-party attestation about specific interp findings, sourced from a co-founder at a public event. It does not point to a paper or post, so it cannot be operationally consumed today — but it's a forward signal that an interp publication along these lines is likely coming, and it confirms (from Olah) that the "functional emotional states" framing is something Anthropic is willing to assert publicly. HN traction (67 pts, 87 comments) is real but the discussion is mostly about ethics/PR framing, not technical content. The comment thread mostly debates whether the framing is meaningful engagement vs. corporate positioning.

Press echoes (in window, all sub-3 pts on HN — listing for completeness, not because traction qualifies):

- [Reuters — "Anthropic's Olah says AI must be guided from outside Big Tech"](https://www.reuters.com/world/europe/anthropics-olah-says-ai-must-be-guided-outside-big-tech-2026-05-25/) (2026-05-25 15:35 UTC, 3 pts on HN)
- [Forbes — "Anthropic Cofounder Joins Pope Leo, Warns of AI Job Losses"](https://www.forbes.com/sites/aliciapark/2026/05/25/anthropic-billionaire-cofounder-joins-pope-leo-warns-ai-job-losses-will-spark-moral-imperative-of-historic-proportions/) (2026-05-25 15:36 UTC, 2 pts)
- [AP — "Pope calls for robust regulation of AI"](https://apnews.com/article/pope-ai-tech-trump-vatican-anthropic-d92d0108730d146baa46da041b8523da) (2026-05-25 21:49 UTC, 2 pts)
- [National Catholic Reporter — "Pope Leo, Anthropic co-founder call for church-tech ethics partnership"](https://www.ncronline.org/vatican/vatican-news/pope-leo-anthropic-co-founder-call-church-tech-ethics-partnership-magnifica) (2026-05-25 18:54 UTC, 1 pt)
- [Religion News Service — "The Vatican-Anthropic relationship that's reshaping the AI ethics debate"](https://religionnews.com/2026/05/22/why-anthropic-is-helping-unveil-the-popes-new-encyclical-on-ai/) (2026-05-25 23:39 UTC, 1 pt)

**HN front page**: zero Anthropic/Claude/Mythos titles on the HN front page at briefing time (`hn.algolia.com` `tags=front_page` filter with the briefing window returned `nbHits: 0`). The Olah post at 67/87 is the strongest signal but didn't reach the front page.

In-window-but-below-threshold items (listing only — none warrant deeper coverage):

- [Lenny's Newsletter — "How the engineer behind Claude Cowork uses Claude"](https://www.lennysnewsletter.com/p/how-i-ai-how-the-engineer-behind) — 2026-05-25 16:20 UTC, 3 pts / 1 comment. Personal interview, no first-party content.
- [Blog post — "Why Ctrl+V won't paste images in Claude Code on WSL"](https://rajveerbachkaniwala.com/blog/2026/05/24/on-the-difficulty-of-pasting-a-picture/) — 2026-05-25 14:41 UTC, 3 pts. Useful if you hit this; otherwise noise.
- HN [#48270403 "Ask HN: How is all new software not broken?"](https://news.ycombinator.com/item?id=48270403) — passing references to Claude in comments; not Anthropic-focused.

---

## Excluded but worth a look

**[Apple security advisory — macOS Tahoe 26.5, CVE-2026-28952](https://support.apple.com/en-us/127115)** (released 2026-05-11, surfaced on HN 2026-05-25 23:40 UTC at 1 pt). Apple credits **"Calif.io in collaboration with Claude and Anthropic Research"** for a kernel integer-overflow vulnerability that could cause unexpected system termination, addressed via improved input validation. **Why it's worth a look:** this appears to be the first publicly visible major-OS-vendor attribution of a vulnerability disclosure to "Claude and Anthropic Research" — concrete external confirmation of the kind of work Project Glasswing/Mythos is described as doing. Outside the recency window (advisory dates to 2026-05-11) and below traction threshold on HN today (1 pt), but never appeared in any prior briefing, so flagging it. If you're tracking Glasswing/Mythos evidence, this is a load-bearing real-world data point. Worth scanning [the full Apple advisory](https://support.apple.com/en-us/127115) for other Claude-credited CVEs in the same release.

_Promising-but-excluded for recency:_ none beyond the Apple advisory. All other Mythos-rumor primary sources were already addressed in the morning briefing.

---

## Sources (in-window items only)

- [Anthropic news — "Chris Olah's remarks on Pope Leo XIV's encyclical 'Magnifica humanitas'"](https://www.anthropic.com/news/chris-olah-pope-leo-encyclical) (2026-05-25)
- [HN discussion of the Olah post](https://news.ycombinator.com/item?id=48270497) (2026-05-25 19:12 UTC, 67 pts / 87 comments)
- [The Register — "Anthropic to release Mythos-class models to the public"](https://www.theregister.com/security/2026/05/25/anthropic-to-release-mythos-class-models-to-the-public/5245596) (2026-05-25 08:09 UTC publish; 20:29 UTC HN submission)
- [Reuters — "Anthropic's Olah says AI must be guided from outside Big Tech"](https://www.reuters.com/world/europe/anthropics-olah-says-ai-must-be-guided-outside-big-tech-2026-05-25/) (2026-05-25 15:35 UTC)
- [Forbes — Olah/Pope/AI job losses](https://www.forbes.com/sites/aliciapark/2026/05/25/anthropic-billionaire-cofounder-joins-pope-leo-warns-ai-job-losses-will-spark-moral-imperative-of-historic-proportions/) (2026-05-25)
- [AP — Pope AI encyclical](https://apnews.com/article/pope-ai-tech-trump-vatican-anthropic-d92d0108730d146baa46da041b8523da) (2026-05-25)
- [status.claude.com](https://status.claude.com/) (verified: no incidents after 10:39 UTC)
- [platform.claude.com release notes](https://platform.claude.com/docs/en/release-notes/overview) (verified: no 2026-05-25 entries)
- [github.com/anthropics/claude-code/releases](https://github.com/anthropics/claude-code/releases) (verified via `gh api`: no releases after 2026-05-23)
- [HN Algolia — "anthropic" in window](https://hn.algolia.com/api/v1/search_by_date?query=anthropic&tags=story&numericFilters=created_at_i%3E1779706800&hitsPerPage=30) (consulted; nothing above the threshold beyond the Olah post)

## Method / verification notes

- Window-start anchor: morning briefing at `/home/komi/notes/anthropic-updates/anthropic-daily-update-2026-05-25.md` (file mtime 2026-05-25 04:11 PDT = 11:11 UTC; briefing self-reports a window end of 11:07 UTC). Window-end anchor: `date -u` at compose time (2026-05-25 ~23:45 UTC). Window length ≈ 12.6h — under the 24h cap, no gap-detection header needed.
- First-party sources verified: anthropic.com/news, anthropic.com/research, alignment.anthropic.com, anthropic.com/engineering, platform.claude.com/docs/en/release-notes/overview, status.claude.com.
- GitHub releases verified via `gh api` for `anthropics/claude-code`, `claude-agent-sdk-python`, `claude-agent-sdk-typescript`, `anthropic-sdk-python`, `anthropic-sdk-typescript`.
- HN traction sampled via `hn.algolia.com/api/v1/search_by_date` with `created_at_i > 1779706800` (= 2026-05-25 11:00 UTC), for queries `anthropic`, `claude`, `mythos`. Front-page filter (`tags=front_page`) returned zero hits.
