# Future Plans

## Historical Changelog Storage

### Overview
Currently, the skill only stores metadata about the last generated changelog (timestamp, date range, PR count). We should add a feature to store the full historical changelogs.

### Proposed Implementation

**1. Store each generated changelog** with:
- Timestamp
- User name
- Date range covered
- Full changelog content (formatted text)
- PR/commit count breakdown

**2. Storage options:**
- Option A: `.changelog-history/` directory with individual files per changelog
  - Format: `.changelog-history/2026-02-02_anna_jan26-feb2.txt`
- Option B: Single JSON file with all changelogs
  - Format: `.changelog-history.json` with array of changelog objects
- Option C: Both - JSON for querying, text files for easy reading

**3. Add retrieval commands:**
- "Show me the last 5 changelogs"
- "Show Anna's changelog from Jan 26"
- "Show all changelogs from last week"
- "List all changelogs for Tom"

### Benefits
- Review what was posted before
- Track who shipped what over time
- Generate reports or summaries
- Avoid duplicate posts
- Historical record for performance reviews

### Implementation Notes
- Update `.changelog-metadata.json` to include reference to latest entries
- Add `.changelog-history/` to `.gitignore` (keep private)
- Consider compression for long-term storage
- Add cleanup command for old entries (e.g., archive older than 6 months)
