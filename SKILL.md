---
name: gog
description: Google Workspace CLI for Gmail, Calendar, Drive, Sheets, Docs, Slides, Contacts, Tasks, and more.
triggers: gmail, email, calendar, google drive, spreadsheet, sheets, docs, slides, contacts, tasks, send email, search email
allowed-tools: Bash(gog:*)
compatibility: Linux, macOS, Windows
---

## Quick Start

```bash
# Check if installed
gog version

# Check auth status
gog auth list --no-input
```

**If not installed:** See [gogcli.sh/#install](https://gogcli.sh/#install) for installation.

**Usage:** Always use `--json --no-input` flags. Use `--account email` for specific accounts.

## Authentication

**IMPORTANT:** OAuth requires human interaction — the user must open a URL in their browser. Agents cannot complete this step.

### Check Auth Status

```bash
gog auth list --no-input 2>&1
```

- **No accounts:** User needs to set up auth (see below)
- **Keyring error:** Set `GOG_KEYRING_PASSWORD` env var (headless/WSL/CI)

### First-Time Setup (requires human)

**Step 1: Create Google Cloud project and OAuth credentials**

1. Go to https://console.cloud.google.com/projectcreate and create a project
2. Enable APIs you need (Gmail, Calendar, Drive, Sheets, People, Tasks)
3. Configure OAuth consent screen: https://console.cloud.google.com/auth/branding
4. Create OAuth client: https://console.cloud.google.com/auth/clients
   - Type: "Desktop app", download the JSON file

**Step 2: Add test users (if app is in "Testing" mode)**

If OAuth app is not published, every account must be added as a test user:
- Go to: Google Cloud Console → APIs & Services → OAuth consent screen → **Audience**
- Click "Add users" under "Test users" and add each email

**Step 3: Store credentials and authorize**

```bash
# Store the OAuth client JSON
gog auth credentials ~/Downloads/client_secret_....json

# Authorize account (requires human to open browser)
gog auth add you@gmail.com

# For headless/WSL/SSH (no browser):
gog auth add you@gmail.com --services user --manual
# Prints URL → user opens in browser → signs in → copies redirect URL back

# Or two-step remote flow:
gog auth add you@gmail.com --services user --remote --step 1   # Prints auth URL
# User authorizes, copies redirect URL, then:
gog auth add you@gmail.com --services user --remote --step 2 --auth-url '<redirect-url>'

# Test
gog gmail labels list --account you@gmail.com --no-input
```

### Keyring Backends

Tokens are stored in a keyring:

- **macOS:** Keychain Access (automatic)
- **Linux with desktop:** Secret Service / GNOME Keyring (automatic)
- **Linux headless / WSL / CI:** File-based keyring — **requires `GOG_KEYRING_PASSWORD`**

```bash
# Check current backend
gog auth keyring

# Switch to file backend (for headless/CI)
gog auth keyring file

# Set password (add to shell profile for persistence)
export GOG_KEYRING_PASSWORD='your-password-here'
```

### Multiple Accounts

```bash
# Add another account
gog auth add another@gmail.com

# Use account alias
gog auth alias set work work@company.com
gog gmail search 'is:unread' --account work

# Or set default via env
export GOG_ACCOUNT=you@gmail.com
```

### Service Accounts (Workspace only)

```bash
gog auth service-account set you@yourdomain.com --key ~/Downloads/service-account.json
```

### Auth Commands

```bash
gog auth status                           # Show current auth state
gog auth list                             # List stored accounts
gog auth list --check                     # Validate stored tokens
gog auth remove <email>                   # Remove a stored account
gog auth keyring                          # Show/set keyring backend (auto|keychain|file)
```

### Environment Variables

- `GOG_ACCOUNT` — Default account email or alias
- `GOG_KEYRING_BACKEND` — Keyring backend: `auto`, `keychain`, `file`
- `GOG_KEYRING_PASSWORD` — Password for file-based keyring (required for headless/WSL/CI)
- `GOG_JSON` — Default JSON output
- `GOG_ENABLE_COMMANDS` — Comma-separated allowlist of commands (sandboxing)

## Common Commands

### Gmail
```bash
gog gmail search 'is:unread' --max 10
gog gmail get <messageId> --json
gog gmail send --to "a@b.com" --subject "Hi" --body "Text"
gog gmail thread get <threadId>
gog gmail thread get <threadId> --download          # Download attachments
```

### Calendar
```bash
gog calendar events --today
gog calendar events --week
gog calendar create primary --summary "Meeting" --from "2026-03-01T09:00:00" --to "2026-03-01T10:00:00"
gog calendar create primary --summary "Offsite" --from "2026-04-01" --all-day
gog calendar event <calendarId> <eventId>
gog calendar freebusy --calendars "primary,work@example.com" --from "..." --to "..."
```

### Drive
```bash
gog drive ls
gog drive search "report" --max 10
gog drive get <fileId>                    # Metadata
gog drive download <fileId>
gog drive upload ./file.pdf --parent <folderId>
gog drive upload ./file.docx --convert    # Convert to Google format
gog drive share <fileId> --to user --email user@example.com --role writer
```

### Sheets
```bash
gog sheets get <spreadsheetId> "Sheet1!A1:D10"
gog sheets get <spreadsheetId> "Sheet1!A:Z" --json
gog sheets update <spreadsheetId> "Sheet1!A1:B2" --values-json '[["Name","Value"],["x","y"]]'
gog sheets append <spreadsheetId> "Sheet1!A:B" --values-json '[["new","row"]]'
gog sheets clear <spreadsheetId> "Sheet1!A1:Z100"
gog sheets metadata <spreadsheetId>       # Sheet names, tabs
```

### Docs
```bash
gog docs cat <docId>
gog docs cat <docId> --all-tabs
gog docs export <docId> --format pdf --out ./doc.pdf
gog docs export <docId> --format docx --out ./doc.docx
```

### Slides
```bash
gog slides info <presentationId>
gog slides export <presentationId> --format pdf --out ./deck.pdf
```

### Tasks
```bash
gog tasks lists
gog tasks list <listId> --max 50
gog tasks add <listId> --title "Task" --due 2026-03-01
gog tasks done <listId> <taskId>
gog tasks undo <listId> <taskId>
```

### Contacts
```bash
gog contacts search "Alice" --max 10
gog contacts list --max 100
gog contacts create --given "John" --family "Doe" --email "john@example.com" --phone "+1234567890"
gog contacts update people/<resourceName> --given "Jane" --email "jane@example.com"
```

### Forms
```bash
gog forms get <formId>
gog forms responses list <formId> --max 20
```

### Apps Script
```bash
gog appscript get <scriptId>
gog appscript run <scriptId> myFunction --params '["arg1", 123]'
```

### Chat (Workspace only)
```bash
gog chat spaces list
gog chat messages send spaces/<spaceId> --text "Build complete!"
gog chat dm send user@company.com --text "ping"
```

### Groups (Workspace only)
```bash
gog groups list
gog groups members engineering@company.com
```

### People
```bash
gog people me
gog people search "Alice" --max 5
```

### Time
```bash
gog time now
gog time now --timezone UTC
```

## Tips
- Use `--json` for parseable output
- Use `--no-input` to prevent prompts
- Use `--force` for destructive ops (delete, clear)
- Gmail search: `from:`, `to:`, `subject:`, `has:attachment`, `newer_than:7d`, `is:unread`, `label:`
- Calendar times: RFC3339, dates (`2026-03-01`), or relative (`today`, `tomorrow`)
