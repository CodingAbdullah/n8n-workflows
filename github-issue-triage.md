# GitHub Issue Triage

## What it does

Listens for GitHub `issues` webhook events. When a new issue is opened, it sends the title and body to OpenAI (GPT-4o-mini), which responds with a suggested set of labels and a priority level. The workflow then automatically applies those labels to the issue via the GitHub API.

Supported labels: `bug`, `enhancement`, `documentation`, `question`, `performance`, `security`, `good first issue`, `help wanted`, `duplicate`, `wontfix`.

## Trigger

HTTP POST webhook — register in GitHub under **Repository → Settings → Webhooks**, listening for `Issues` events.

## Required Credentials

| Credential | n8n Type | Where to get it |
|------------|----------|----------------|
| GitHub Personal Access Token | `HTTP Header Auth` | GitHub → Settings → Developer settings → Fine-grained token with `issues: write` on your repo |
| OpenAI API key | `OpenAI API` | [platform.openai.com/api-keys](https://platform.openai.com/api-keys) |

## Required Setup

1. Create a GitHub token with `issues: write` permission.
2. In n8n, create an **HTTP Header Auth** credential:
   - Header Name: `Authorization`
   - Header Value: `token YOUR_GITHUB_TOKEN`
3. Ensure the labels referenced (`bug`, `enhancement`, etc.) already exist in your GitHub repo — create them under **Issues → Labels** if missing.
4. Register the webhook in GitHub (**Repository → Settings → Webhooks → Add webhook**), event: `Issues`.

## How to Import

1. Open your n8n instance.
2. Click **Workflows** → **Add Workflow** → **Import from File**.
3. Select `github-issue-triage.json`.
4. Assign your **HTTP Header Auth** credential to **Apply Labels via GitHub API**.
5. Assign your **OpenAI API** credential to **Suggest Labels with OpenAI**.
6. Click **Activate** and register the webhook URL in GitHub.

## Nodes Overview

| Node | Type | Purpose |
|------|------|---------|
| GitHub Webhook | `webhook` | Receives GitHub issues events |
| Filter Issue Opened | `if` | Only processes events where `action == "opened"` |
| Suggest Labels with OpenAI | `openAi` | Classifies the issue and returns suggested labels + priority as JSON |
| Parse Triage Result | `set` | Parses the AI JSON response and extracts repo/issue metadata |
| Apply Labels via GitHub API | `httpRequest` | POSTs the suggested labels to the GitHub Issues API |

## Expected Output

When issue #42 "App crashes on login when password contains special characters" is opened:

- OpenAI suggests `["bug", "security"]` with priority `high`
- The labels `bug` and `security` are automatically applied to the issue on GitHub
