# gog-skill

A skill that enables AI agents to interact with Google Workspace services (Gmail, Calendar, Drive, Sheets, Docs, Slides, Contacts, Tasks, and more).

## What is this?

This skill allows your AI assistant to:

- **Gmail**: Search, read, send emails, and manage threads
- **Calendar**: View events, create meetings, check availability
- **Drive**: Browse, upload, download, and share files
- **Sheets**: Read, write, and update spreadsheet data
- **Docs**: Read and export Google Docs
- **Slides**: Export presentations
- **Tasks**: Manage task lists
- **Contacts**: Search and manage contacts
- **And more**: Forms, Apps Script, Chat, Groups (Workspace)

## Prerequisites

This skill requires the `gog` CLI to be installed and authenticated on your system.

**Install and set up gog:** https://gogcli.sh

## Installing the Skill

Copy the `SKILL.md` file to your AI agent's skills directory:

```bash
# Example: Copy to your skills directory
cp SKILL.md ~/.agents/skills/gog/
```

The skill will be automatically available once:
1. The `gog` CLI is installed
2. You have authenticated with `gog auth add your@email.com`

## Usage Examples

Once installed, you can ask your AI assistant to:

- "Show my last 5 unread emails"
- "Create a calendar event for tomorrow at 2pm"
- "Search my Drive for quarterly reports"
- "List all my tasks"
- "Export my Google Doc as PDF"

## Configuration

### Environment Variables

| Variable | Description |
|----------|-------------|
| `GOG_ACCOUNT` | Default account email or alias |
| `GOG_KEYRING_BACKEND` | Keyring backend: `auto`, `keychain`, `file` |
| `GOG_KEYRING_PASSWORD` | Password for file-based keyring (headless/CI) |
| `GOG_JSON` | Default to JSON output |
| `GOG_ENABLE_COMMANDS` | Comma-separated allowlist for sandboxing |

### Multiple Accounts

```bash
# Add another account
gog auth add another@gmail.com

# Set an alias
gog auth alias set work work@company.com

# Use specific account in commands
gog gmail search 'is:unread' --account work
```

## Security Notes

- OAuth tokens are stored securely in your system keychain
- For headless environments, use file-based keyring with `GOG_KEYRING_PASSWORD`
- Never commit OAuth credentials or tokens to version control

## Resources

- **gog CLI Docs**: https://gogcli.sh
- **gog CLI GitHub**: https://github.com/gog-cli/gog
