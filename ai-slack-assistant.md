# AI Slack Assistant

## What it does

Listens for Slack `app_mention` events — any time a user `@mentions` your Slack bot. Strips the mention tag from the message, sends the remaining text to OpenAI (GPT-4o-mini), and posts the AI response back in the same Slack thread.

## Trigger

Slack webhook trigger on the `app_mention` event type. Requires your n8n instance to be publicly reachable so Slack can deliver events.

## Required Credentials

| Credential | n8n Type | Where to get it |
|------------|----------|----------------|
| Slack Bot Token | `Slack API` | [api.slack.com/apps](https://api.slack.com/apps) → OAuth & Permissions → Bot Token (`xoxb-...`) |
| OpenAI API key | `OpenAI API` | [platform.openai.com/api-keys](https://platform.openai.com/api-keys) |

## Required Setup

1. Create a Slack App at [api.slack.com/apps](https://api.slack.com/apps).
2. Under **OAuth & Permissions**, add bot scopes: `app_mentions:read`, `chat:write`.
3. Under **Event Subscriptions**, enable events and set the request URL to your n8n webhook URL (shown in the Slack Trigger node after import).
4. Subscribe to the `app_mention` bot event.
5. Install the app to your workspace and invite the bot to the desired channel (`/invite @YourBotName`).
6. Create a **Slack API** credential in n8n using the Bot User OAuth Token.

## How to Import

1. Open your n8n instance.
2. Click **Workflows** → **Add Workflow** → **Import from File**.
3. Select `ai-slack-assistant.json`.
4. Open the **Slack Trigger** node — copy the webhook URL and paste it into your Slack App's Event Subscriptions page.
5. Assign your Slack and OpenAI credentials to the relevant nodes.
6. Click **Activate**.

## Nodes Overview

| Node | Type | Purpose |
|------|------|---------|
| Slack Trigger | `slackTrigger` | Receives `app_mention` events from Slack via webhook |
| Build Prompt | `set` | Cleans the mention tag from the message and extracts channel/thread IDs |
| Ask OpenAI | `openAi` | Sends the cleaned user message to GPT-4o-mini with a Slack-aware system prompt |
| Reply in Thread | `slack` | Posts the AI response in the same Slack thread as the original mention |

## Expected Output

When a user types `@YourBot what is the capital of France?` in Slack, the bot replies in-thread within a few seconds: `The capital of France is Paris.`
