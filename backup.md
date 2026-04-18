# Slash Command Backups

Original prompts before `$ARGUMENTS` migration (2026-04-15).

---

## ingest.md

"Read the schema in CLAUDE.md. Then process [FILENAME] from raw/. Read it fully, discuss key takeaways with me, then: create a summary page in wiki/, update wiki/index.md, update all relevant concept and entity pages, add backlinks, flag any contradictions, and append to wiki/log.md."

---

## lint.md

Run a full health check on wiki/ per the lint workflow in CLAUDE.md. Output to wiki/lint-report-[date].md with severity levels (🔴 errors, 🟡 warnings, 🔵 info). Suggest 3 articles to fill the biggest knowledge gaps.

---

## query.md

Read wiki/index.md. Based on what's in the knowledge base, answer: [YOUR QUESTION]. Cite which wiki pages informed your answer. If this reveals new connections worth preserving, create a new page in wiki/ and update the index.
