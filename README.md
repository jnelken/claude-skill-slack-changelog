# Slack Changelog Generator - Claude Code Skill

A Claude Code skill that automatically generates Slack-formatted product updates from your GitHub pull requests.

## What it does

This skill fetches merged PRs from your GitHub repositories within a specified date range and transforms them into a polished, Slack-ready changelog with:

- Grouped updates by theme (UI/UX, Bug Fixes, AI Features, etc.)
- Appropriate emojis (:star2: for features, :bug: for bugs, :robot_face: for AI)
- Concise, user-focused descriptions
- Combined related changes for better readability
- Multi-user support (generate changelogs for your teammates)
- Optional Slack webhook integration for automatic posting

## Installation

1. Copy `changelog.md` to your Claude Code skills directory:
   ```bash
   cp changelog.md ~/.claude/skills/
   ```
   Or for project-specific installation:
   ```bash
   cp changelog.md /path/to/your/project/.claude/skills/
   ```

2. The skill will be automatically available in Claude Code

## Configuration

### Option 1: Config File (Recommended for Teams)

Create a `.changelog-config.json` file in your project root or skill directory:

```json
{
  "users": {
    "jake": "jake@example.com",
    "anna": "anna@example.com",
    "tom": "tom@example.com"
  },
  "repos": [
    "your-org/repo1",
    "your-org/repo2"
  ],
  "slackWebhook": "https://hooks.slack.com/services/YOUR/WEBHOOK/URL"
}
```

This allows you to:
- Generate changelogs for any team member by name
- Configure repositories once for everyone
- Optionally post directly to Slack

**Note:** You can copy `.changelog-config.example.json` as a starting point. Add `.changelog-config.json` to your `.gitignore` if you want to keep the Slack webhook URL private.

### Option 2: Edit the Skill Directly

If you don't need multi-user support, you can edit `changelog.md` and hardcode your repo paths in step 3:
```bash
gh pr list --repo YOUR-ORG/YOUR-REPO --author @me --state merged --search "merged:START_DATE..END_DATE" --json number,title,mergedAt,url,body --limit 100
```

## Usage

In Claude Code, invoke the skill with natural language:

### For yourself:
```
Generate changelog from Jan 1 to Jan 8
```

```
Create a changelog for the last week
```

### For teammates (requires config file):
```
Generate changelog for Anna from Jan 15 to Jan 22
```

```
Create a changelog for Tom from the last week
```

### Post directly to Slack (requires webhook in config):
```
Generate changelog for Anna from Jan 1 to Jan 8 and post to Slack
```

## Example Output

```
Updates :star2: (Jan 16-26)
Dark mode is now live! Removed feature flag and updated all surfaces to support theme switching
File Viewer now shows better messaging for processing states and properly displays extracted values
Added text data type support for variables (both frontend and backend)
:bug: Fixed scroll area viewport sizing and thread sidebar truncation
:robot_face: AI-generated chat titles now appear immediately

Dev updates :technologist:
Code cleanup: Migrated types repo, ran eslint fixes
Added knip for dead code detection - helps identify unused files and dependencies
Backend: Added Slack notifier for CI pipeline alerts
```

## Requirements

- [Claude Code](https://claude.com/claude-code)
- [GitHub CLI](https://cli.github.com/) (`gh`) installed and authenticated
- Access to the repositories you want to generate changelogs from

## Customization

You can customize the skill by editing `changelog.md`:

- Add or remove emoji types
- Adjust grouping themes
- Modify the output format
- Change the tone (technical vs. user-focused)
- Add additional repositories

## License

MIT
