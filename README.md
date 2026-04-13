# Martin Minghetti

**I build multi-agent AI systems that do real work** — not chatbot wrappers.

Sales research pipelines, invoice processing, competitive intelligence, appointment booking, code review. Each project ships a working product where AI agents collaborate on structured tasks with deterministic outputs.

Every project is open source, runs on your infrastructure, and uses your own API keys.

---

## The Approach

Most AI projects wrap a single LLM call in a UI. These don't.

Every project here follows three principles:

| Principle | What it means |
|-----------|---------------|
| **Direct SDK, no frameworks** | No LangChain, no CrewAI, no abstractions that hide what's happening. Every agent call is explicit, debuggable, and cheap to run. |
| **Multi-agent orchestration** | Complex tasks split into specialized agents. A security reviewer and a test gap detector don't need the same context — so they don't get it. Parallel where possible, sequential where needed. |
| **Human-in-the-loop by default** | AI proposes, humans approve. Flagged invoices go to a review queue. Outreach drafts get a quality score. Alerts fire only above a threat threshold. Nothing ships without a checkpoint. |

---

## Projects

### Agent Pipelines

| Project | What it does | Agents | Stack |
|---------|-------------|--------|-------|
| [**SDR Swarm**](https://github.com/martin-minghetti/sdr-swarm) | B2B sales research — from company URL to scored outreach draft in 15-30s. Researcher fetches from Tavily, homepage scraper, and Apollo in parallel. | 4 sequential (researcher → analyst → writer → scorer) | FastAPI, Next.js 16, Supabase, SSE streaming |
| [**Code Review Orchestrator**](https://github.com/martin-minghetti/code-review-orchestrator) | Paste a GitHub PR URL — 4 agents review in parallel. Every finding pinned to file + line with evidence and concrete fix. [Live demo](https://code-review-orchestrator.vercel.app). | 4 parallel (security, impact, test gaps, docs) | Next.js 16, Octokit, Vercel AI SDK, shadcn/ui |
| [**InvoiceAI**](https://github.com/martin-minghetti/invoice-processor) | 5-stage pipeline: Vision extraction → Zod validation → fuzzy PO matching → anomaly detection → human review queue. 6 anomaly rules, ships with sample invoices. | Vision extraction + rule engine | Next.js 16, Claude Vision, Drizzle/SQLite |
| [**RivalSight**](https://github.com/martin-minghetti/rivalsight) | Competitive intelligence monitor — snapshots pages with Playwright, extracts structured data via Claude, diffs against previous snapshots, and scores threats deterministically. Webhooks for medium+ alerts. | Playwright + Claude extraction + scoring rules | Next.js 16, Playwright, Drizzle/SQLite |

### Workflow Automation

| Project | What it does | Stack |
|---------|-------------|-------|
| [**WhatsApp AI Receptionist**](https://github.com/martin-minghetti/whatsapp-ai-receptionist) | Conversational appointment booking via WhatsApp. Checks real-time Google Calendar availability, handles cancellations, transcribes voice messages via Whisper. Config-driven — new clients onboarded via YAML, no code changes. | FastAPI, Claude, Redis, Google Calendar, Mercado Pago |
| [**Conversation-to-Action**](https://github.com/martin-minghetti/conversation-to-action) | Watches Slack, Discord, and WhatsApp threads — extracts bugs, features, tasks, and decisions with Claude. Deduplicates against your Linear/Notion backlog. Team approves or discards via in-channel buttons. ~$0.03-0.05 per thread. | Next.js 15, Supabase Realtime, Railway |

### Developer Tools

| Project | What it does | Stack |
|---------|-------------|-------|
| [**SkillCam**](https://github.com/martin-minghetti/skillcam) | Turn successful AI agent runs into reusable markdown skills. Reads native session logs from Claude Code and Codex CLI, distills them into SKILL.md files. Works with any LLM or without one (`--no-llm` mode). | TypeScript, npm package — [`npx skillcam distill --latest`](https://www.npmjs.com/package/skillcam) |

---

## BYOK — Bring Your Own Keys

Every project that calls an LLM uses your own API keys, stored encrypted in your instance. No hosted inference, no usage limits you don't control, no vendor lock-in.

Typical cost per run:

| Project | Cost | Why |
|---------|------|-----|
| SDR Swarm | ~$0.08-0.15 | Sonnet for research/analysis/writing, Haiku for scoring |
| Code Review Orchestrator | ~$0.10-0.20 | Sonnet for security/impact, Haiku for tests/docs |
| Conversation-to-Action | ~$0.03-0.05 | Sonnet for extraction per 10-20 message thread |
| InvoiceAI | ~$0.05-0.10 | Vision API per invoice |

---

## Tech Stack

```
AI           → Anthropic Claude (direct SDK), Claude Vision, Vercel AI SDK
Backend      → FastAPI (Python) · Next.js API Routes (TypeScript)
Frontend     → Next.js App Router · Tailwind CSS v4 · shadcn/ui
Data         → Supabase · SQLite/Drizzle · Redis
Validation   → Zod
Infra        → Vercel · Railway
Testing      → Vitest · Pytest
Scraping     → Playwright · BeautifulSoup
Integrations → WhatsApp Business API · Google Calendar · Mercado Pago
               Slack · Discord · Linear · Notion · Apollo · Tavily
```

---

## Contributing

Issues and PRs welcome on any project. If you're using one of these in production, I'd like to hear about it.

Each repo has its own contributing guidelines, test suite, and `DECISIONS.md` documenting the key architectural trade-offs.

---

## Contact

- GitHub: [@martin-minghetti](https://github.com/martin-minghetti)
- Location: Bariloche, Argentina
