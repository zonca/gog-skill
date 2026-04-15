# gog-skill

A skill that provides a CLI for Google Workspace integration. Access Gmail, Calendar, Drive, Sheets, Docs, Slides, Contacts, Tasks, and more directly from your AI assistant.

## What is this?

This skill enables AI agents to interact with Google Workspace services using the [`gog`](https://gogcli.sh) CLI tool. It allows the AI to:

- **Gmail**: Search, read, send emails, and manage threads
- **Calendar**: View events, create meetings, check availability
- **Drive**: Browse, upload, download, and share files
- **Sheets**: Read, write, and update spreadsheet data
- **Docs**: Read and export Google Docs
- **Slides**: Export presentations
- **Tasks**: Manage task lists
- **Contacts**: Search and manage contacts
- **And more**: Forms, Apps Script, Chat, Groups (Workspace)

## Installation

### Step 1: Install the gog CLI

Follow the official installation guide at [gogcli.sh/#install](https://gogcli.sh/#install)

For most users:

```bash
# macOS
brew install gog-cli

# Or download from https://gogcli.sh
```

### Step 2: Set up Google Cloud OAuth

1. Go to [Google Cloud Console](https://console.cloud.google.com/projectcreate) and create a new project
2. Enable the APIs you need (Gmail, Calendar, Drive, Sheets, People, Tasks)
3. Configure the OAuth consent screen at [OAuth branding](https://console.cloud.google.com/auth/branding)
4. Create an OAuth client at [OAuth clients](https://console.cloud.google.com/auth/clients)
   - Select **Application type**: "Desktop app"
   - Download the JSON credentials file

### Step 3: Authorize gog

```bash
# Store your OAuth credentials
gog auth credentials ~/Downloads/client_secret_*.json

# Authorize your Google account (opens browser)
gog auth add you@gmail.com
```

**For headless/WSL/SSH environments:**

```bash
gog auth add you@gmail.com --services user --manual
```

This prints a URL — open it in your browser, sign in, and paste the redirect URL back.

### Step 4: Install this skill

Copy the `SKILL.md` file to your AI agent's skills directory. The skill is automatically available when the `gog` CLI is installed and authenticated.

## Usage Examples

Once installed, you can ask your AI assistant to:

```
- "Show my last 5 unread emails"
- "Create a calendar event for tomorrow at 2pm"
- "Search my Drive for quarterly reports"
- "List all my tasks"
- "Export my Google Doc as PDF"
```

### Common Commands

```bash
# Gmail
gog gmail search 'is:unread' --max 10
gog gmail send --to "user@example.com" --subject "Hello" --body "Message"

# Calendar
gog calendar events --today
gog calendar create primary --summary "Meeting" --from "2026-04-16T14:00:00" --to "2026-04-16T15:00:00"

# Drive
gog drive ls
gog drive upload ./file.pdf --parent <folderId>

# Sheets
gog sheets get <spreadsheetId> "Sheet1!A1:D10"

# Tasks
gog tasks lists
gog tasks add <listId> --title "My Task" --due 2026-04-20
```

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

# Use specific account
gog gmail search 'is:unread' --account work
```

## Security Notes

- OAuth tokens are stored securely in your system keychain
- For headless environments, use file-based keyring with `GOG_KEYRING_PASSWORD`
- Never commit OAuth credentials or tokens to version control
- Use `GOG_ENABLE_COMMANDS` to restrict available commands in shared environments

## Resources

- **Official Docs**: https://gogcli.sh
- **GitHub**: https://github.com/gog-cli/gog

## License

This skill definition is provided as part of the AI skills collection. The `gog` CLI is a separate project with its own license.
