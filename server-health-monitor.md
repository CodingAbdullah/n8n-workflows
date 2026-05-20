# Server Health Monitor

## What it does

Runs on a 5-minute schedule and pings a configurable list of URLs (APIs, dashboards, microservices, CDNs). For each URL that returns a non-200 status code — or times out — it sends a Slack alert to `#engineering-alerts` with the service name, URL, status code, and timestamp.

Healthy URLs produce no output; only failures trigger alerts.

## Trigger

Schedule trigger — runs automatically every 5 minutes. No external webhook registration required.

## Required Credentials

| Credential | n8n Type | Where to get it |
|------------|----------|----------------|
| Slack Bot Token | `Slack API` | [api.slack.com/apps](https://api.slack.com/apps) → Bot Token (`xoxb-...`), scope: `chat:write` |

## Required Setup

1. Open the **Define Target URLs** node and replace the example URLs with your actual endpoints.
   - Each entry has a `name` (display label) and a `url` (the endpoint to ping).
   - Add or remove entries as needed — the workflow handles any number of targets.
2. Create a **Slack API** credential in n8n with your bot token.
3. Invite the bot to your alerts channel.
4. Change `channel` in **Alert Slack** from `engineering-alerts` to your actual channel.
5. Optionally change the schedule interval in **Schedule Trigger** (e.g. `minutesInterval: 1` for 1-minute checks).

## How to Import

1. Open your n8n instance.
2. Click **Workflows** → **Add Workflow** → **Import from File**.
3. Select `server-health-monitor.json`.
4. Open **Define Target URLs** and update the URL list.
5. Assign your **Slack API** credential to the **Alert Slack** node.
6. Click **Activate** — the workflow starts running on the next scheduled interval.

## Nodes Overview

| Node | Type | Purpose |
|------|------|---------|
| Schedule Trigger | `scheduleTrigger` | Fires every 5 minutes |
| Define Target URLs | `set` | Holds the list of service name + URL pairs to monitor |
| Split Targets | `splitOut` | Converts the URL array into individual items for parallel processing |
| Ping Each URL | `httpRequest` | Makes a GET request to each URL with a 10-second timeout, never throwing on error |
| Check for Failures | `if` | Routes items where `statusCode != 200` to the alert branch |
| Alert Slack | `slack` | Posts a Slack alert for each failing service |

## Expected Output

When `https://api.example.com/health` returns 503:

```
🚨 Server Health Alert
Service: Main API
URL: https://api.example.com/health
Status: 503
Time: Wed, 20 May 2026 14:35:00 GMT
```

Healthy services produce no Slack messages. If all services are healthy, the workflow completes silently.
