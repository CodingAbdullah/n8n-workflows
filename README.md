# n8n-workflows

A collection of custom-designed [n8n](https://n8n.io) workflow automations across two domains: **AI / LLM Automation** and **DevOps & Engineering**.

Each workflow is a standalone `.json` file importable into any n8n instance. A companion `.md` file documents credentials, setup, and expected behavior.

---

## Importing a Workflow

1. Open your n8n instance
2. Go to **Workflows → New → Import from File**
3. Select the `.json` file
4. Add your credentials under **Settings → Credentials**
5. Activate the workflow

---

## AI / LLM Automation

| Workflow | File | Description |
|----------|------|-------------|
| AI Email Summarizer | `ai-email-summarizer.json` | Summarizes incoming Gmail messages with OpenAI and sends the digest back to the sender |
| AI Slack Assistant | `ai-slack-assistant.json` | Listens for Slack app-mentions and replies in-thread using OpenAI |
| AI Content Classifier | `ai-content-classifier.json` | Webhook endpoint that classifies any text payload (topic + sentiment) via OpenAI |
| AI Code Review Assistant | `ai-code-review-assistant.json` | Triggers on GitHub PRs, reviews the diff with OpenAI, and posts a review comment |

## DevOps & Engineering

| Workflow | File | Description |
|----------|------|-------------|
| GitHub PR Notifier | `github-pr-notifier.json` | Posts a Slack message whenever a GitHub PR is opened or merged |
| GitHub CI Failure Alert | `github-ci-failure-alert.json` | Sends a Slack alert when a GitHub Actions workflow run fails |
| GitHub Issue Triage | `github-issue-triage.json` | Uses OpenAI to suggest labels for new GitHub issues and applies them automatically |
| Server Health Monitor | `server-health-monitor.json` | Pings a list of URLs every 5 minutes and alerts Slack if any return a non-200 response |

---

## License

MIT — see [LICENSE](./LICENSE)
