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

3. **Analyze and categorize the PRs** into two groups:

   **User-facing updates** (product features, UX improvements, user-visible bug fixes):
   - UI/UX improvements
   - New features
   - User-visible bug fixes
   - Performance improvements users can feel
   - AI features

   **Developer updates** (tooling, infrastructure, code quality):
   - ESLint/linting rules
   - Testing infrastructure
   - Build/CI/CD changes
   - Code refactoring that doesn't affect users
   - Dependency upgrades
   - Developer tooling (Storybook, knip, etc.)
   - Monitoring/debugging improvements (Datadog, etc.)

4. **Format the output** as Slack-ready text with TWO sections:

   **Section 1: "Updates :star2: (DATE_RANGE)"** (user-facing changes)
   - Include the date range in the header (e.g., "Jan 16-26" or "Dec 15-20")
   - Use hyphens (-) for each bullet point (Slack auto-converts these to bullets)
   - Use :star2: emoji for features
   - Use :bug: emoji for bug fixes
   - Use :robot_face: emoji for AI-related features
   - Keep descriptions concise and user-focused (what changed, why it matters)
   - Combine related PRs into single bullet points when appropriate

   **Section 2: "Dev updates :technologist:"** (developer/infrastructure changes)
   - Use hyphens (-) for each bullet point
   - Group similar items together (e.g., all ESLint changes, all dependency upgrades)
   - Be specific about what tooling was added/improved
   - Explain the benefit where relevant (reduces cognitive load, improves debugging, etc.)

5. **Style guidelines**:
   - Write in past tense ("Added X", "Fixed Y", "Updated Z")
   - Be specific but concise
   - Highlight the user benefit when relevant
   - Group backend/API changes separately or integrate with frontend changes
   - De-emphasize ticket numbers (can include in parentheses if needed)

## Example Output

```
Updates :star2: (Jan 16-26)
- Major routing overhaul: Generated routes from single source of truth dictionary, meaning we should (almost) never have broken links again in the app
- Consolidated Button components for uniform look and feel of all buttons in the app
- :bug: Fixed popover/modal z-index conflicts using visibility-based hiding (preserves form state when opening modals from popovers)
- :bug: Fixed chat messages going missing due to improper cache invalidation
- :bug: Fixed extractor field ordering being overwritten when updating configs
- File explorer UX -- clicking on a filename was kind of difficult, i made the surface area larger to click

Dev updates :technologist:
- :bug: Fixed Datadog sourcemap matching for better error tracking in production
- Added custom ESLint rules to flag when we're calling things "entity" in the UI
- Added Storybook for our component design system documentation and visual testing
- Added knip for dead code detection - helps identify unused files and dependencies, reduce cognitive load
- Backend: Added dotenv-expand for cleaner env variable management (supports `${VAR}` interpolation)
- Backend: Added Slack notifier for CI pipeline alerts
- Upgraded zod to v4, removed Sentry, updated react-router
```

## Usage

Invoke this skill by saying:
- "Generate changelog from Jan 1 to Jan 8"
- "Create a changelog for the last week"
- "Show me what I shipped from December 15 to December 20"
