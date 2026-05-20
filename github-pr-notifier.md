# GitHub PR Notifier

## What it does

Receives GitHub `pull_request` webhook events and posts formatted Slack messages to your `#engineering` channel (or any channel you configure). Handles two events:

- **PR Opened** — posts the PR title, author, repo, and lines changed
- **PR Merged** — posts the PR title, who merged it, and the target branch

## Trigger

HTTP POST webhook — register in GitHub under **Repository → Settings → Webhooks**, listening for `Pull requests` events.

## Required Credentials

| Credential | n8n Type | Where to get it |
|------------|----------|----------------|
| Slack Bot Token | `Slack API` | [api.slack.com/apps](https://api.slack.com/apps) → Bot Token (`xoxb-...`), scope: `chat:write` |

## Required Setup

1. Create a Slack App with the `chat:write` bot scope and install it to your workspace.
2. Invite the bot to the channel you want notifications in (`/invite @YourBotName`).
3. In n8n, create a **Slack API** credential with your bot token.
4. After importing and activating, copy the webhook URL from the **GitHub Webhook** node.
5. Register the URL in GitHub (**Repository → Settings → Webhooks → Add webhook**), content type `application/json`, event: `Pull requests`.
6. Change `channel` in both Slack nodes from `engineering` to your actual channel name/ID if different.

## How to Import

1. Open your n8n instance.
2. Click **Workflows** → **Add Workflow** → **Import from File**.
3. Select `github-pr-notifier.json`.
4. Assign your **Slack API** credential to both Slack nodes.
5. Update the `channel` field in both Slack nodes to match your workspace.
6. Click **Activate** and register the webhook URL in GitHub.

## Nodes Overview

| Node | Type | Purpose |
|------|------|---------|
| GitHub Webhook | `webhook` | Receives all pull_request events from GitHub |
| Filter Opened or Merged | `if` | Routes to opened-PR notification when `action == "opened"` |
| Filter Merged | `if` | Routes to merged-PR notification when `action == "closed"` and `merged == true` |
| Notify PR Opened | `slack` | Posts a Slack message for newly opened PRs |
| Notify PR Merged | `slack` | Posts a Slack message for merged PRs |

## Expected Output

**On PR opened:**
```
↗ New PR Opened
*Fix: null pointer in user auth flow*
By alice in acme/backend
+142 / -38 lines
```

**On PR merged:**
```
🔀 PR Merged
*Fix: null pointer in user auth flow*
Merged by bob into main
```
