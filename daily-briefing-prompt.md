Compile a daily briefing of Anthropic-related developments useful to an applied AI engineer working in the space.

SCOPE — include:
- New Claude Code features, releases, or notable changelog entries
- New Anthropic research publications or blog posts
- Leaks, rumors, or speculation (especially when surfaced or discussed on Hacker News)
- API changes, model releases, pricing/access changes
- Notable third-party tooling or integrations relevant to Claude
- Substantive community discussion (HN front page, not low-signal social posts)

SOURCES — prefer in this order:
1. First-party: [anthropic.com](http://anthropic.com), [docs.claude.com](http://docs.claude.com), [support.claude.com](http://support.claude.com), Anthropic GitHub repos and release notes
2. Hacker News (front page items and high-vote comments, not random submissions)
3. Established AI/tech press only when first-party isn't available

DETERMINE THE WINDOW (do this FIRST, before searching for news):
1. Search past conversations for the most recent prior briefing produced from this prompt. Identify when it was generated (or the latest date it covered).
2. Set the window start to that timestamp. The window end is "now."
3. Cap the lookback at 7 days. If the last briefing was more than 7 days ago, clamp the window start to "now minus 7 days."
4. If no prior briefing can be found, default the window to the last 24 hours.
5. If the resulting window is greater than 24 hours, record that a gap was detected — it will be flagged in the output header.

RECENCY FILTER (hard requirement):
- Only include items published, released, or substantively updated within the window determined above. Do NOT apply a hardcoded 24-hour cutoff — items from skipped days are in-scope as long as they fall inside the window.
- For each item, include the publication timestamp. If you cannot confirm a timestamp within the window, exclude it.
- Do not include "evergreen" or background context items unless they are genuinely new within the window.

DEDUPLICATION (soft layer):
- Items that appeared in the prior briefing identified above should be excluded UNLESS there is a material development (new details, official confirmation of a prior rumor, follow-up release). In that case, frame it as "Update on [previous item]" and state what changed.
- If past-chat search returned nothing, skip this step and rely on the recency filter alone.

FORMAT:
- Begin with a header line stating the window covered, e.g., "Briefing window: <start date/time> – <end date/time>".
- If the window is greater than 24 hours (a gap was detected), add a second header line: "Catch-up briefing covering N days due to skipped run(s) — items from those days are included."
- Group by category (Claude Code / Research / Rumors & Leaks / API & Models / Other)
- For each item: headline, source link, publication time, 1–2 sentence summary, and why it matters to a practitioner
- If a category has no qualifying items in the window, write "Nothing new" rather than padding
- End with a one-line note flagging anything that looked promising but was excluded — either because its timestamp couldn't be confirmed, or because it fell outside the 7-day cap — so I can decide if I want to dig in
