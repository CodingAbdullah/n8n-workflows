# GitHub CI Failure Alert

## What it does

Receives GitHub `workflow_run` webhook events. When a workflow run completes with a `failure` conclusion, it sends a detailed Slack alert to your `#engineering-alerts` channel including the workflow name, branch, commit message, triggering actor, and a direct link to the failed run.

Successful runs and in-progress events are silently ignored.

## Trigger

HTTP POST webhook — register in GitHub under **Repository → Settings → Webhooks**, listening for `Workflow runs` events.

## Required Credentials

| Credential | n8n Type | Where to get it |
|------------|----------|----------------|
| Slack Bot Token | `Slack API` | [api.slack.com/apps](https://api.slack.com/apps) → Bot Token (`xoxb-...`), scope: `chat:write` |

## Required Setup

1. Create a Slack App with `chat:write` scope and install it to your workspace.
2. Invite the bot to your alerts channel (`/invite @YourBotName`).
3. Create a **Slack API** credential in n8n.
4. After importing and activating, copy the webhook URL from the **GitHub Webhook** node.
5. Register it in GitHub (**Repository → Settings → Webhooks → Add webhook**), event: `Workflow runs`.
6. Change `channel` in the **Send Slack Alert** node from `engineering-alerts` to your actual channel.

## How to Import

1. Open your n8n instance.
2. Click **Workflows** → **Add Workflow** → **Import from File**.
3. Select `github-ci-failure-alert.json`.
4. Assign your **Slack API** credential to the **Send Slack Alert** node.
5. Update the `channel` value if needed.
6. Click **Activate** and register the webhook URL in GitHub.

## Nodes Overview

| Node | Type | Purpose |
|------|------|---------|
| GitHub Webhook | `webhook` | Receives all workflow_run events from GitHub |
| Check Workflow Completed | `if` | Filters to only `action == "completed"` events |
| Check Run Failed | `if` | Filters to only runs where `conclusion == "failure"` |
| Build Alert Message | `set` | Composes the Slack alert text with run details |
| Send Slack Alert | `slack` | Posts the alert to the configured Slack channel |

## Expected Output

```
🔴 CI Failure
Workflow: Build and Test
Branch: `feature/new-auth`
Commit: `a3f92bc` — Add JWT token refresh logic
Triggered by: alice
Run: View Run
```
