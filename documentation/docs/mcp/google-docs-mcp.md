---
title: Google Docs Extension
description: Add the Google Docs MCP Server as a Goose Extension — read, write, create, and manage Google Docs and Drive files directly from Goose.
---

import Tabs from '@theme/Tabs';
import TabItem from '@theme/TabItem';
import CLIExtensionInstructions from '@site/src/components/CLIExtensionInstructions';
import GooseDesktopInstaller from '@site/src/components/GooseDesktopInstaller';

This tutorial covers how to add the **Google Docs MCP Server** — built by [Harold Moses](https://github.com/hmoses) — as a Goose extension, giving Goose full read and write access to your Google Docs and Google Drive files.

With this extension, Goose can read documents, make targeted edits, find-and-replace content, apply formatting, create new docs, list and search your Drive, share files, copy documents, and more — all without leaving your conversation.

:::tip Quick Install
<Tabs groupId="interface">
  <TabItem value="ui" label="Goose Desktop" default>
    [Launch the installer](goose://extension?cmd=python3&arg=%7BPATH_TO%7D%2Fserver.py&id=google-docs&name=Google%20Docs&description=Read%2C%20write%2C%20create%2C%20and%20manage%20Google%20Docs%20and%20Drive%20files&timeout=300)
  </TabItem>
  <TabItem value="cli" label="Goose CLI">
  **Command**
  ```sh
  python3 ~/goose-google-docs-extension/server.py
  ```
  </TabItem>
</Tabs>
:::

## What You Can Do

| Tool | Description |
|------|-------------|
| `google_docs_auth_status` | Check OAuth authentication status |
| `google_docs_authenticate` | Trigger the OAuth 2.0 login flow |
| `google_docs_read` | Read the full text of any Google Doc |
| `google_docs_get_metadata` | Get title, ID, and revision info |
| `google_docs_create` | Create a new doc with optional body text |
| `google_docs_append_text` | Append text to the end of a doc |
| `google_docs_replace_text` | Find and replace text across a doc |
| `google_docs_insert_text` | Insert text at a specific character index |
| `google_docs_delete_range` | Delete a character range |
| `google_docs_apply_bold` | Apply bold formatting to a range |
| `google_docs_set_heading` | Set heading level (H1–H6 or Normal) |
| `google_docs_batch_update` | Send raw Docs API batchUpdate requests |
| `google_docs_list` | List Google Docs in Drive (with search) |
| `google_docs_copy` | Duplicate a doc with a new title |
| `google_docs_delete` | Move a doc to trash |
| `google_docs_rename` | Rename a doc |
| `google_docs_share` | Share a doc with an email address |
| `google_docs_export` | Export doc as plain text or HTML |

## Prerequisites

- **Python 3.10+** — [Download here](https://www.python.org/downloads/)
- **uv** (recommended) or pip — [Install uv](https://docs.astral.sh/uv/#installation)
- A **Google Cloud project** with OAuth credentials (see setup below)

## Installation

### 1. Clone the extension

```sh
git clone https://github.com/hmoses/goose-google-docs-extension.git ~/goose-google-docs-extension
cd ~/goose-google-docs-extension
chmod +x install.sh
./install.sh
```

The installer will:
- Create a Python virtual environment in `.venv/`
- Install all dependencies (`mcp`, `google-auth`, `google-api-python-client`)
- Register the extension in `~/.config/goose/config.yaml`

### 2. Set up Google Cloud credentials

You need a free Google Cloud project to get OAuth credentials.

**Step 1 — Create a project:**

Go to [https://console.cloud.google.com/projectcreate](https://console.cloud.google.com/projectcreate) and create a new project.

**Step 2 — Enable APIs:**

Enable both of these in your project:
- [Google Docs API](https://console.cloud.google.com/apis/library/docs.googleapis.com)
- [Google Drive API](https://console.cloud.google.com/apis/library/drive.googleapis.com)

**Step 3 — Create OAuth credentials:**

1. Go to [Credentials](https://console.cloud.google.com/auth/clients/create)
2. Configure the OAuth consent screen if prompted (choose **External**, fill in app name and email)
3. Create an **OAuth client ID** — application type: **Desktop app**
4. Download the JSON file

**Step 4 — Place credentials:**

```sh
mv ~/Downloads/client_secret_*.json ~/.config/goose/google-docs-extension/credentials.json
```

**Step 5 — Add yourself as a test user:**

If your app is unverified, go to the [OAuth Consent Screen](https://console.cloud.google.com/auth/audience) → **Test users** → add your Gmail address.

## Configuration

<Tabs groupId="interface">
  <TabItem value="ui" label="Goose Desktop" default>

  <GooseDesktopInstaller
    extensionId="google-docs"
    extensionName="Google Docs"
    description="Read, write, create, and manage Google Docs and Drive files"
    type="stdio"
    command="python3"
    args={["PATH_TO/goose-google-docs-extension/server.py"]}
    timeout={300}
  />

  :::info
  Replace `PATH_TO` with the full path to where you cloned the extension, e.g. `/Users/yourname`.
  :::

  </TabItem>
  <TabItem value="cli" label="Goose CLI">

  <CLIExtensionInstructions
    name="Google Docs"
    description="Read, write, create, and manage Google Docs and Drive files"
    type="stdio"
    command="python3 ~/goose-google-docs-extension/server.py"
    timeout={300}
  />

  </TabItem>
</Tabs>

## Authenticate with Google

Once installed, restart Goose and authenticate:

### Goose Prompt
```
Check my Google Docs auth status, then authenticate.
```

### Goose Output

:::note Desktop

A browser window will open automatically. Log in with your Google account and grant the requested permissions. Your token will be saved locally and auto-refreshed going forward.

```
✅ Successfully authenticated with Google!
Token saved to: ~/.config/goose/google-docs-extension/token.json
```

:::

## Example Usage

### Read and Edit a Document

#### Goose Prompt
```
Read this Google Doc and update the summary section to emphasize my
Kubernetes experience: https://docs.google.com/document/d/YOUR_DOC_ID/edit
```

#### Goose Output

:::note Desktop

```
I've read the document "Engineering Resume — Harold Moses".

Here's what I'll update in the summary section...

✅ Replaced 1 occurrence of the summary paragraph.
✅ Applied bold formatting to key terms.
```

:::

---

### Create a New Document

#### Goose Prompt
```
Create a new Google Doc called "Q2 Planning Notes" with a brief intro paragraph.
```

#### Goose Output

:::note Desktop

```
✅ Created document: Q2 Planning Notes
🆔 ID: `1aBcDeFgHiJkLmNoPqRsTuVwXyZ`
🔗 URL: https://docs.google.com/document/d/1aBcDeFgHiJkLmNoPqRsTuVwXyZ/edit
```

:::

---

### List and Search Your Docs

#### Goose Prompt
```
List my 10 most recently modified Google Docs.
```

#### Goose Output

:::note Desktop

```
📁 Google Docs

- Engineering Resume — Harold Moses
  🆔 `1Khb9bchiKxiveSEg3DN8d2TRL4ZQy0k9ibhbICSUAw4`
  🕒 Modified: 2026-03-13
  🔗 https://docs.google.com/document/d/.../edit

- Q2 Planning Notes
  🆔 `1aBcDeFgHiJkLmNoPqRsTuVwXyZ`
  🕒 Modified: 2026-03-13
  🔗 https://docs.google.com/document/d/.../edit
```

:::

---

### Share a Document

#### Goose Prompt
```
Share my resume doc with alice@example.com as a writer.
```

#### Goose Output

:::note Desktop

```
✅ Shared document `1Khb9bchi...` with alice@example.com as writer.
```

:::

## Security & Privacy

- All credentials are stored **locally** at `~/.config/goose/google-docs-extension/`
- OAuth tokens are auto-refreshed and never sent anywhere except Google's own APIs
- Requested scopes:
  - `https://www.googleapis.com/auth/documents` — Read/write Docs
  - `https://www.googleapis.com/auth/drive` — List/manage Drive files
- Revoke access anytime at [https://myaccount.google.com/permissions](https://myaccount.google.com/permissions)

## Troubleshooting

**"credentials.json not found"**
Place your OAuth credentials at `~/.config/goose/google-docs-extension/credentials.json`.

**"Access blocked: app not verified"**
Add your email as a test user at [https://console.cloud.google.com/auth/audience](https://console.cloud.google.com/auth/audience).

**"Token expired"**
Delete `~/.config/goose/google-docs-extension/token.json` and re-run `google_docs_authenticate`.

**Extension not loading in Goose**
Re-run `./install.sh` and restart Goose. Check `~/.config/goose/config.yaml` for the `google-docs:` block.

## Credits

Built by **[Harold Moses](https://github.com/hmoses)** — source code available at [github.com/hmoses/goose-google-docs-extension](https://github.com/hmoses/goose-google-docs-extension).
