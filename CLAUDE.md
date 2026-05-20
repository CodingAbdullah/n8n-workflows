# CLAUDE.md

## Project Purpose

This repo is a curated collection of importable n8n workflow automations across two domains:
- **AI / LLM Automation** — single-pass OpenAI integrations (email, Slack, webhooks, GitHub)
- **DevOps & Engineering** — GitHub event handling and infrastructure monitoring via Slack

All workflows are self-contained `.json` files importable into any n8n instance.

---

## File Structure

All files live flat at the repo root — no subdirectories.

Each workflow is a **pair** of files:
```
<kebab-name>.json   ← n8n workflow definition
<kebab-name>.md     ← companion documentation
```

---

## Naming Conventions

| Prefix | Domain |
|--------|--------|
| `ai-` | AI / LLM automation (single-pass) |
| `agent-` | Agentic workflows (multi-step, tools, memory) |
| `github-` | GitHub event-driven DevOps |
| `server-` | Infrastructure monitoring |

Names are lowercase kebab-case. Example: `ai-email-summarizer.json`.

---

## Workflow JSON Rules

Every `.json` file must include these top-level fields:

```json
{
  "name": "Human-Readable Workflow Name",
  "nodes": [...],
  "connections": {...},
  "active": false,
  "settings": { "executionOrder": "v1" },
  "meta": { "templateCredsSetupCompleted": true }
}
```

- `active` must always be `false` — users activate after importing
- Node IDs use the format `xxxxxxxx-xxxx-4000-8000-xxxxxxxxxxxx`
- Node positions use 240px horizontal spacing starting at x=240
- Credentials are referenced by name only — never embed keys or tokens

---

## Node Package Namespaces

| Node type | Package prefix |
|-----------|---------------|
| Standard nodes (webhook, HTTP, Slack, Gmail, etc.) | `n8n-nodes-base.` |
| AI / LangChain nodes (agent, LLM, memory, tools, vector stores) | `@n8n/n8n-nodes-langchain.` |

---

## Credentials Policy

Credentials must be referenced by display name only:

```json
"credentials": {
  "openAiApi": { "id": "2", "name": "OpenAI Account" }
}
```

Never include actual API keys, tokens, passwords, or secrets anywhere in `.json` files.

---

## Companion README Requirements

Every `.md` file must contain these sections in order:

1. **What it does** — one paragraph describing the workflow's purpose
2. **Trigger** — how the workflow starts (webhook path, schedule, event type)
3. **Required Credentials** — table of credential name, n8n type, and where to obtain it
4. **Required Setup** — numbered steps before the workflow will function
5. **How to Import** — numbered steps to import into n8n
6. **Nodes Overview** — table with columns: Node | Type | Purpose
7. **Expected Output** — what a successful execution produces (include a sample if applicable)

---

## What to Avoid

- Do not set `"active": true` in any workflow JSON
- Do not create subdirectories — all files stay at the repo root
- Do not add credentials, `.env` files, or secrets of any kind
- Do not create workflow files without a companion `.md`
- Do not add comments inside JSON files
