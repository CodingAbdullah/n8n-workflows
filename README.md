# n8n-workflows

A collection of custom-designed [n8n](https://n8n.io) workflow automations across two domains: **AI / LLM Automation** and **DevOps & Engineering**.

Each workflow is a standalone `.json` file importable into any n8n instance. A companion `.md` file documents credentials, setup, and expected behavior.

---

## What These Workflows Cover

### AI / LLM Automation

These workflows integrate OpenAI (GPT-4o / GPT-4o-mini) into everyday tools to automate tasks that would otherwise require manual reading, writing, or decision-making.

- **Email summarization** — instead of reading every incoming email in full, the workflow reads it for you and delivers a 3-bullet summary back to your inbox.
- **Slack Q&A assistant** — any team member can `@mention` your bot in Slack and get an AI-generated answer in the same thread, without leaving the app.
- **Text classification API** — a single POST endpoint that accepts any text and returns a structured JSON object with topic category, sentiment, confidence score, and a one-sentence summary. Drop it into any pipeline or app.
- **Automated code review** — when a pull request is opened on GitHub, the workflow fetches the diff, sends it to GPT-4o for analysis, and posts a structured review comment covering bugs, security issues, and style suggestions — before a human even opens the PR.

### DevOps & Engineering

These workflows connect GitHub and your infrastructure to Slack, keeping your team informed in real time without anyone having to watch dashboards or check emails.

- **PR notifications** — the moment a pull request is opened or merged, a formatted Slack message lands in your engineering channel with the author, repo, and lines changed.
- **CI failure alerts** — when a GitHub Actions run fails, an alert fires immediately to Slack with the branch, the commit message, who triggered the run, and a direct link to the failed run.
- **Issue auto-labeling** — new GitHub issues are automatically read by OpenAI, which suggests the most relevant labels (`bug`, `security`, `enhancement`, etc.) and applies them via the GitHub API — saving triage time on every issue.
- **Server health monitoring** — every 5 minutes, the workflow pings a configurable list of URLs (APIs, dashboards, microservices). Any endpoint that returns a non-200 status or times out triggers an immediate Slack alert with the service name and status code.

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
