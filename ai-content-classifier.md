# AI Content Classifier

## What it does

Exposes a POST webhook endpoint that accepts a JSON body with a `text` field. OpenAI classifies the text into a topic category and sentiment, then the workflow responds synchronously with a structured JSON result. Designed to be called from any application, script, or other automation.

## Trigger

HTTP POST webhook at `/webhook/classify-content`. The workflow responds inline (synchronous — the HTTP call blocks until the classification is complete).

## Required Credentials

| Credential | n8n Type | Where to get it |
|------------|----------|----------------|
| OpenAI API key | `OpenAI API` | [platform.openai.com/api-keys](https://platform.openai.com/api-keys) |

## Required Setup

1. Create an **OpenAI API** credential in n8n.
2. After importing, activate the workflow — the webhook URL becomes live once active.
3. Copy the webhook URL from the **Webhook** node (format: `https://<your-n8n>/webhook/classify-content`).

## How to Import

1. Open your n8n instance.
2. Click **Workflows** → **Add Workflow** → **Import from File**.
3. Select `ai-content-classifier.json`.
4. Assign your OpenAI credential to the **Classify with OpenAI** node.
5. Click **Activate**.

## Request Format

```bash
curl -X POST https://<your-n8n>/webhook/classify-content \
  -H "Content-Type: application/json" \
  -d '{"text": "NASA successfully launched the Artemis mission to the Moon today."}'
```

## Nodes Overview

| Node | Type | Purpose |
|------|------|---------|
| Webhook | `webhook` | Receives POST requests with `{ "text": "..." }` in the body |
| Classify with OpenAI | `openAi` | Classifies topic, sentiment, confidence, and generates a summary |
| Parse Classification | `set` | Parses the OpenAI JSON string response and adds metadata |
| Respond to Webhook | `respondToWebhook` | Returns the classification result to the HTTP caller |

## Expected Output

```json
{
  "success": true,
  "data": {
    "classification": {
      "topic": "science",
      "sentiment": "positive",
      "confidence": 0.97,
      "summary": "NASA launched the Artemis mission to the Moon."
    },
    "inputText": "NASA successfully launched the Artemis mission to the Moon today.",
    "processedAt": "2026-05-20T14:32:01.000Z"
  }
}
```
