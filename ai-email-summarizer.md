# AI Email Summarizer

## What it does

Polls your Gmail inbox every minute for new messages. When a new email arrives, it sends the full body to OpenAI (GPT-4o-mini) and asks for a 3-bullet-point summary focused on action items and key information. The summary is then sent back to the original sender as a reply email.

## Trigger

Gmail polling trigger — checks for new messages every minute.

## Required Credentials

| Credential | n8n Type | Where to get it |
|------------|----------|----------------|
| Gmail account | `Gmail OAuth2` | Google Cloud Console → OAuth 2.0 Client |
| OpenAI API key | `OpenAI API` | [platform.openai.com/api-keys](https://platform.openai.com/api-keys) |

## Required Setup

1. Create a **Gmail OAuth2** credential in n8n (Settings → Credentials → New → Gmail OAuth2).
2. Create an **OpenAI API** credential in n8n with your secret key.
3. In the **Send Summary Email** node, optionally change `sendTo` to a fixed address if you want summaries delivered to yourself instead of the sender.
4. Optionally change the `modelId` in **Summarize with OpenAI** to `gpt-4o` for higher quality summaries.

## How to Import

1. Open your n8n instance.
2. Click **Workflows** in the left sidebar → **Add Workflow** → **Import from File**.
3. Select `ai-email-summarizer.json`.
4. Open each node with a credential placeholder and assign your saved credentials.
5. Click **Activate** (toggle in the top-right corner).

## Nodes Overview

| Node | Type | Purpose |
|------|------|---------|
| Gmail Trigger | `gmailTrigger` | Polls Gmail inbox for new messages every minute |
| Summarize with OpenAI | `openAi` | Sends email body to GPT-4o-mini and requests a bullet-point summary |
| Send Summary Email | `gmail` | Emails the AI-generated summary back to the original sender |

## Expected Output

Each new email triggers one execution. The original sender receives a follow-up email with subject `AI Summary: <original subject>` containing three bullet points summarizing the email content.
