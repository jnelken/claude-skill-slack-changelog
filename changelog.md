---
skill: changelog
description: Generate a Slack-formatted changelog from GitHub PRs for a given date range
tags: [github, changelog, slack]
---

# Changelog Generator

Generate a Slack-formatted product update from GitHub pull requests merged within a specified date range.

## Instructions

When the user invokes this skill, they will provide a date range and optionally a user name. Follow these steps:

1. **Parse the input**:
   - Extract start_date and end_date in YYYY-MM-DD format
   - Extract user name if provided (e.g., "for Anna", "for Tom") - defaults to current user (@me)

2. **Read the config file**:
   - Read `.changelog-config.json` from the skill directory or repo root
   - Get the user's email and GitHub username from config (e.g., config.users.anna.email, config.users.anna.github)
   - Get the list of repos from config.repos
   - Get the Slack webhook URL from config.slackWebhook (if posting to Slack)

3. **Fetch both PRs AND direct commits from all repos**:

   **For PRs** (most developers use this workflow):
   ```bash
   gh pr list --repo REPO_NAME --state all --search "is:pr is:merged merged:START_DATE..END_DATE" --json number,title,mergedAt,author,url,body --limit 100 | jq '[.[] | select(.author.login == "GITHUB_USERNAME")]'
   ```

   **For direct commits** (some developers commit directly to main):
   ```bash
   gh api "/repos/REPO_NAME/commits?since=START_DATET00:00:00Z&until=END_DATET23:59:59Z" --jq '.[] | select(.commit.author.email == "EMAIL") | {sha: .sha[0:7], message: .commit.message, date: .commit.author.date, url: .html_url}'
   ```

   **Combine both sources** - merge PRs and direct commits, removing duplicates if a commit is part of a PR

4. **Analyze and categorize the PRs** into two groups:

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

5. **Format the output** as plain text Slack mrkdwn (NO bold, italics, or list formatting) with TWO sections:

   **Section 1: "Updates :star2: (DATE_RANGE)"** (user-facing changes)
   - Include the date range in the header (e.g., "Jan 16-26" or "Dec 15-20")
   - Use plain newlines to separate each item (no hyphens, no list syntax)
   - Use :star2: emoji for features
   - Use :bug: emoji for bug fixes
   - Use :robot_face: emoji for AI-related features
   - Keep descriptions concise and user-focused (what changed, why it matters)
   - Combine related PRs into single bullet points when appropriate
   - IMPORTANT: Use plain text only - no markdown bold (*), italics (_), hyphens (-), or other formatting

   **Section 2: "Dev updates :technologist:"** (developer/infrastructure changes)
   - Use plain newlines to separate each item (no hyphens, no list syntax)
   - Group similar items together (e.g., all ESLint changes, all dependency upgrades)
   - Be specific about what tooling was added/improved
   - Explain the benefit where relevant (reduces cognitive load, improves debugging, etc.)
   - IMPORTANT: Use plain text only - no markdown formatting or list syntax

6. **Style guidelines**:
   - Use plain text Slack mrkdwn format only (no bold, italics, or markdown formatting)
   - Write in past tense ("Added X", "Fixed Y", "Updated Z")
   - Be specific but concise
   - Highlight the user benefit when relevant
   - Group backend/API changes separately or integrate with frontend changes
   - De-emphasize ticket numbers (can include in parentheses if needed)

7. **Post to Slack (optional)**:
   - If the user requests "post to Slack" or "send to Slack", use the webhook from config
   - Escape any special characters in the changelog text for JSON
   - Post using curl:
   ```bash
   curl -X POST -H 'Content-type: application/json' \
     --data "{\"text\":\"CHANGELOG_TEXT_HERE\"}" \
     SLACK_WEBHOOK_URL
   ```
   - Confirm successful posting or show error message

## Example Output

```
Updates :star2: (Jan 16-26)
Major routing overhaul: Generated routes from single source of truth dictionary, meaning we should (almost) never have broken links again in the app
Consolidated Button components for uniform look and feel of all buttons in the app
:bug: Fixed popover/modal z-index conflicts using visibility-based hiding (preserves form state when opening modals from popovers)
:bug: Fixed chat messages going missing due to improper cache invalidation
:bug: Fixed extractor field ordering being overwritten when updating configs
File explorer UX -- clicking on a filename was kind of difficult, i made the surface area larger to click

Dev updates :technologist:
:bug: Fixed Datadog sourcemap matching for better error tracking in production
Added custom ESLint rules to flag when we're calling things "entity" in the UI
Added Storybook for our component design system documentation and visual testing
Added knip for dead code detection - helps identify unused files and dependencies, reduce cognitive load
Backend: Added dotenv-expand for cleaner env variable management (supports `${VAR}` interpolation)
Backend: Added Slack notifier for CI pipeline alerts
Upgraded zod to v4, removed Sentry, updated react-router
```

## Usage

Invoke this skill by saying:
- "Generate changelog from Jan 1 to Jan 8" (uses current user)
- "Generate changelog for Anna from Jan 15 to Jan 22"
- "Create a changelog for Tom from the last week"
- "Show me what I shipped from December 15 to December 20"
- "Generate changelog for Anna from Jan 1 to Jan 8 and post to Slack"
