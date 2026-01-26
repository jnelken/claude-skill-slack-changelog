---
skill: changelog
description: Generate a Slack-formatted changelog from GitHub PRs for a given date range
tags: [github, changelog, slack]
---

# Changelog Generator

Generate a Slack-formatted product update from GitHub pull requests merged within a specified date range.

## Instructions

When the user invokes this skill, they will provide a date range (start and end dates). Follow these steps:

1. **Parse the date range**: Extract start_date and end_date in YYYY-MM-DD format

2. **Fetch PRs from both repos**:
   ```bash
   gh pr list --repo Concentro-Inc/woodrow --author @me --state merged --search "merged:START_DATE..END_DATE" --json number,title,mergedAt,url,body --limit 100
   gh pr list --repo Concentro-Inc/api --author @me --state merged --search "merged:START_DATE..END_DATE" --json number,title,mergedAt,url,body --limit 100
   ```

3. **Analyze and group the PRs** by theme/topic:
   - UI/UX improvements
   - File Viewer features
   - Data type or schema changes
   - Bug fixes
   - AI features
   - Performance improvements
   - Code quality/refactoring
   - Infrastructure changes

4. **Format the output** as Slack-ready text:
   - Start with "Updates :star2:"
   - Use hyphens (-) for each bullet point (Slack auto-converts these to bullets)
   - Use :star2: emoji for features
   - Use :bug: emoji for bug fixes
   - Use :robot_face: emoji for AI-related features
   - Keep descriptions concise and user-focused (what changed, not how)
   - Combine related PRs into single bullet points when appropriate
   - Focus on user-facing changes; group technical/internal changes together

5. **Style guidelines**:
   - Write in past tense ("Added X", "Fixed Y", "Updated Z")
   - Be specific but concise
   - Highlight the user benefit when relevant
   - Group backend/API changes separately or integrate with frontend changes
   - De-emphasize ticket numbers (can include in parentheses if needed)

## Example Output

```
Updates :star2:
- Dark mode is now live! Removed feature flag and updated all surfaces to support theme switching
- File Viewer now shows better messaging for processing states and properly displays extracted values
- Added text data type support for variables (both frontend and backend)
- Sidebar nav icons are now clickable when collapsed
- :bug: Fixed scroll area viewport sizing and thread sidebar truncation
- :robot_face: AI-generated chat titles now appear immediately
- Code cleanup: Migrated types repo, ran eslint fixes
```

## Usage

Invoke this skill by saying:
- "Generate changelog from Jan 1 to Jan 8"
- "Create a changelog for the last week"
- "Show me what I shipped from December 15 to December 20"
