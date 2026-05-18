# Anthropic Daily Briefing — 2026-05-18

Window: 2026-05-17 ~17:00 UTC → 2026-05-18 ~17:00 UTC (last 24 hours)
Prior briefing on file: 2026-05-14. Items released since then but outside the 24h window are flagged at the end rather than included as today's news.

---

## Claude Code

**Nothing new** within the 24h window.

(The most recent shipped version is v2.1.143, published 2026-05-15 22:28 UTC — outside the 24h window. See "Outside the window but new since last briefing" at the bottom.)

## Research

**Nothing new** within the 24h window.

## Rumors & Leaks

**Nothing new** within the 24h window.

## API & Models

**Claude Haiku 4.5 — elevated errors incident (resolved).**
Source: [status.claude.com](https://status.claude.com/)
Timestamps: investigating 2026-05-18 06:12 UTC → monitoring 07:28 UTC → resolved 08:07 UTC (≈1h55m total).
What happened: Anthropic posted an incident scoped to `claude-haiku-4-5` indicating elevated errors. A fix was deployed and verified before close.
Why it matters: Haiku 4.5 is the default low-latency model for high-volume agentic workloads (classification, routing, code-execution side-queries). A ~2-hour error elevation on a single model with no peer fall-through is the kind of failure mode worth catching in production health checks — confirm your apps fail over to Sonnet 4.6 (or your local fallback) on 5xx and don't simply retry-loop against the same model ID.
No first-party post-incident report has been published as of 17:00 UTC; the status entry is the only artifact.

## Other

**Code with Claude London — livestream tomorrow (2026-05-19).**
Source: [claude.com/code-with-claude/london](https://claude.com/code-with-claude/london), [register-livestream](https://claude.com/code-with-claude/register-livestream)
The London leg of Anthropic's developer conference goes live tomorrow with a free virtual livestream and recordings to follow. In-person applications closed. A second event ("Code with Claude: Extended London," 2026-05-20) is for indie devs / early-stage founders, with no livestream — recordings only.
Why it matters: SF 2026 produced the doubled-Pro/Max limits, Mythos Preview, and the Cowork research preview announcements. The London keynote is the next plausible vehicle for material product or model announcements this week — worth blocking time tomorrow rather than catching it after.
(Note: the announcement itself is older than 24h — included here as a calendar pointer rather than as a new item.)

---

## Excluded but worth a look (outside the recency window)

- **Claude Code v2.1.142 (2026-05-14 22:55 UTC) and v2.1.143 (2026-05-15 22:28 UTC)** — neither was covered in a prior daily briefing because no briefing ran 2026-05-15 / 16 / 17. Material entries for an applied AI engineer: **Fast mode now defaults to Opus 4.7** (was 4.6; pin with `CLAUDE_CODE_OPUS_4_6_FAST_MODE_OVERRIDE=1`), **`MCP_TOOL_TIMEOUT` now actually raises per-request fetch timeouts on remote HTTP/SSE MCP servers** (was capped at 60s regardless), `worktree.bgIsolation: "none"`, plugin dependency enforcement, and the 8-block stop-hook cap (`CLAUDE_CODE_STOP_HOOK_BLOCK_CAP`). [Source: CHANGELOG.md](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- **HN front page: "Show HN: Semble — Code search for agents that uses 98% fewer tokens than grep"** — 346 pts, 115 comments, posted 2026-05-17 15:37 UTC (just outside a strict 24h window). Tree-sitter chunking + semantic/lexical hybrid, CPU-only, ships as an MCP server for Claude Code/Cursor/Codex/OpenCode. Claims 218× faster indexing than transformer baselines at 99% quality. v0.1.7, 1.9k stars, MIT. [github.com/MinishLab/semble](https://github.com/MinishLab/semble), [HN thread](https://news.ycombinator.com/item?id=48169874)
- **Code with Claude London (2026-05-19)** — see above; included as a calendar pointer, not a new item.

---

## Sources

- [status.claude.com](https://status.claude.com/) — Haiku 4.5 incident
- [anthropic.com/news](https://www.anthropic.com/news) — no new items in window
- [anthropic.com/research](https://www.anthropic.com/research) — no new items in window
- [alignment.anthropic.com](https://alignment.anthropic.com/) — no new items in window
- [platform.claude.com release notes](https://platform.claude.com/docs/en/release-notes/overview) — no new items in window (last entry 2026-05-12)
- [anthropics/claude-code releases](https://github.com/anthropics/claude-code/releases) — last release v2.1.143 (2026-05-15)
- [Claude Code CHANGELOG.md](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md)
- [Code with Claude London](https://claude.com/code-with-claude/london)
- [Hacker News — Semble thread](https://news.ycombinator.com/item?id=48169874)
- [MinishLab/semble](https://github.com/MinishLab/semble)
