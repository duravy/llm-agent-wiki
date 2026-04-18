Run a full health check on wiki/ per the lint workflow in CLAUDE.md.

Use the date from $ARGUMENTS if provided, otherwise use today's date, to name the output file: wiki/lint-report-[date].md

Output severity levels:
- 🔴 errors — contradictions, broken links, missing sources
- 🟡 warnings — stale claims, orphan pages, missing cross-references
- 🔵 info — concepts mentioned but never explained, minor gaps

At the end, suggest 3 articles to fill the biggest knowledge gaps in the wiki.
