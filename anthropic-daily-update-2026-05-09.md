# Anthropic Daily Briefing — 2026-05-09

Window: items published, released, or substantively updated in the last ~24 hours (since 2026-05-08 00:00 UTC). Past briefings reviewed for dedup go back to 2026-04-18; nothing between 2026-05-02 and 2026-05-07 was covered, so anything older than the 24-hour window is excluded by the recency filter rather than by dedup.

---

## Claude Code

**v2.1.136 shipped — biggest functional change this week.** ([changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md), May 8, 2026)
A large multi-area release. The items most relevant to a practitioner:

- **MCP server disappearance bug fixed.** MCP servers configured in `.mcp.json`, plugins, and `claude.ai` connectors were silently disappearing after `/clear` in the VS Code extension, JetBrains plugin, and Agent SDK. If you've been re-adding servers all week, you can stop now — upgrade to ≥2.1.136.
- **`settings.autoMode.hard_deny`** added — classifier rules that block actions unconditionally in auto mode. Useful if you've been trying to harden auto-mode policy beyond the standard allow/ask/deny tiers.
- **OTEL session-quality survey re-enabled** for enterprises via `CLAUDE_CODE_ENABLE_FEEDBACK_SURVEY_FOR_OTEL`. Relevant if you're capturing CC telemetry through OpenTelemetry and lost survey responses earlier.
- **Concurrent OAuth refresh fix** — multiple MCP servers refreshing tokens at once were overwriting each other's credentials and triggering login loops. Worth knowing if you've seen flaky MCP auth on a multi-server setup.
- `--resume`/`--continue` now works with project paths containing underscores; plan-mode file-write blocking respects allow rules; extended-thinking + redacted blocks no longer error after tool calls.

**v2.1.137** ([changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md))
Single fix: VS Code extension failing to activate on Windows. Pin to ≥2.1.137 if the team is on Windows + VS Code.

**v2.1.138** ([changelog](https://github.com/anthropics/claude-code/blob/main/CHANGELOG.md))
"Internal fixes" only — no user-visible changes documented.

## Research

**"Teaching Claude Why"** — Alignment Science Blog ([alignment.anthropic.com](https://alignment.anthropic.com/2026/teaching-claude-why/), May 8, 2026; mirrored at [anthropic.com/research](https://www.anthropic.com/research/teaching-claude-why))
Authors: Jonathan Kutasov and Adam Jermyn, with Julius Steen, Minh Le, Sam Bowman, Sam Marks, Jan Leike, Amanda Askell, Chris Olah, Evan Hubinger, and Sara Price. Core claim: training Claude on documents that *explain why* an action is misaligned generalizes better than SFT on demonstrations of aligned behavior alone, with the strongest results from doing both. They attribute Haiku 4.5 / Opus 4.7's perfect score on the agentic-misalignment eval (vs. up to 96% blackmail rate on Opus 4) primarily to safety-training gaps, not capability gaps. Why it matters to a practitioner: this is the public methodology behind why blackmail-style failures dropped to 0% on the 4.5/4.7 generation — useful framing if you're designing your own alignment evals or arguing for "principles + demonstrations" over pure SFT in fine-tuning work.

## Rumors & Leaks

Nothing new in the 24-hour window. No HN front-page items dated May 8–9 on Anthropic that I could confirm with timestamps; the "Hardening Firefox with Claude Mythos Preview" thread on hacks.mozilla.org (367 pts / 162 comments) was submitted ~May 6–7, just outside the window.

## API & Models

**Anthropic ↔ Akamai $1.8B compute deal** — Bloomberg ([article](https://www.bloomberg.com/news/articles/2026-05-08/anthropic-inks-1-8-billion-computing-deal-with-akamai), May 8, 2026)
A second large compute deal in three days, after the SpaceX/Colossus capacity deal announced May 6. Direct practitioner relevance is limited, but it's the second concrete step in the post-SpaceX capacity expansion that justified doubling Pro/Max/Team rate limits — i.e., the rate-limit relief looks supply-backed rather than promotional.

No new entries on [platform.claude.com release notes](https://platform.claude.com/docs/en/release-notes/overview) since May 6 (multiagent sessions + Outcomes public beta, vault background refresh, webhooks).

## Other

**Fortune: "Anthropic grew 80-fold in a single quarter"** ([article](https://fortune.com/2026/05/08/anthropic-80fold-growth-quarter-renting-elon-musk-data-center/), May 8, 2026)
Business-press reframing of the SpaceX/Akamai capacity story with new growth numbers attached. Useful only if you need a stat for a deck — secondary press, not first-party.

---

## Excluded but worth a look

- **"Hardening Firefox with Claude Mythos Preview"** ([hacks.mozilla.org](https://hacks.mozilla.org/2026/05/behind-the-scenes-hardening-firefox/)) — Mozilla's behind-the-scenes on the 271 fixes Mythos surfaced. On HN front page (367 pts / 162 comments) but submitted ~May 6–7, outside the 24h window. The Apr 24 briefing already covered the headline number; this is the engineering-side detail, worth reading if you skipped that one.
- **"Higher usage limits + SpaceX/Colossus deal"** ([anthropic.com/news/higher-limits-spacex](https://www.anthropic.com/news/higher-limits-spacex), May 6) — first-party confirmation of the capacity-expansion rumor flagged in the Apr 30 briefing. Outside 24h, but if you missed the May 6 announcement: peak-hours limit reduction removed for Pro/Max, 5-hour rate limits doubled for Pro/Max/Team/seat-Enterprise.
- **Code w/ Claude 2026 event** (May 6) — Simon Willison's [live blog](https://simonwillison.net/2026/May/6/code-w-claude-2026/) is the highest-signal recap I found. Outside 24h.
