# Slack Changelog Generator - Claude Code Skill

A Claude Code skill that automatically generates Slack-formatted product updates from your GitHub pull requests.

## What it does

This skill fetches merged PRs from your GitHub repositories within a specified date range and transforms them into a polished, Slack-ready changelog with:

- Grouped updates by theme (UI/UX, Bug Fixes, AI Features, etc.)
- Appropriate emojis (:star2: for features, :bug: for bugs, :robot_face: for AI)
- Concise, user-focused descriptions
- Combined related changes for better readability

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

By default, this skill fetches PRs from:
- `Concentro-Inc/woodrow`
- `Concentro-Inc/api`

To use with your own repositories, edit `changelog.md` and update the repo paths in step 2:
```bash
gh pr list --repo YOUR-ORG/YOUR-REPO --author @me --state merged --search "merged:START_DATE..END_DATE" --json number,title,mergedAt,url,body --limit 100
```

## Usage

In Claude Code, invoke the skill with natural language:

```
Generate changelog from Jan 1 to Jan 8
```

```
Create a changelog for the last week
```

```
Show me what I shipped from December 15 to December 20
```

## Example Output

```
Updates :star2:
• Dark mode is now live! Removed feature flag and updated all surfaces to support theme switching
• File Viewer now shows better messaging for processing states and properly displays extracted values
• Added text data type support for variables (both frontend and backend)
• :bug: Fixed scroll area viewport sizing and thread sidebar truncation
• :robot_face: AI-generated chat titles now appear immediately
• Code cleanup: Migrated types repo, ran eslint fixes
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
