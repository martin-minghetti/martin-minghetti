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
| [**GTM Briefing Copilot**](https://github.com/martin-minghetti/gtm-briefing-copilot) | Paste a company URL → verified GTM brief in 30s. Sequential pipeline: scrape → extract facts → analyze → messaging → verify. Evidence grounding with fact IDs, CRM-ready output. [Live demo](https://gtm-briefing-copilot.vercel.app). | 4 sequential (extract → analyze → messaging → verify) | Next.js 16, Vercel AI SDK, cheerio, shadcn/ui |
| [**RivalSight**](https://github.com/martin-minghetti/rivalsight) | Competitive intelligence monitor — snapshots pages with Playwright, extracts structured data via Claude, diffs against previous snapshots, and scores threats deterministically. Webhooks for medium+ alerts. | Playwright + Claude extraction + scoring rules | Next.js 16, Playwright, Drizzle/SQLite |

### Monitoring & Automation

| Project | What it does | Stack |
|---------|-------------|-------|
| [**modelsentry**](https://github.com/martin-minghetti/modelsentry) | AI early warning system — scrapes 8 RSS feeds and diffs 5 provider pages daily, classifies with Gemini, serves a static dashboard with weekly timeline, provider activity trends, and RSS feed. 159 tests, Lighthouse 100/94/96/100. Zero cost. [Live dashboard](https://martin-minghetti.github.io/modelsentry/). | TypeScript, Gemini API, GitHub Actions, GitHub Pages |
| [**WhatsApp AI Receptionist**](https://github.com/martin-minghetti/whatsapp-ai-receptionist) | Conversational appointment booking via WhatsApp. Checks real-time Google Calendar availability, handles cancellations, transcribes voice messages via Whisper. Config-driven — new clients onboarded via YAML, no code changes. | FastAPI, Claude, Redis, Google Calendar, Mercado Pago |
| [**Conversation-to-Action**](https://github.com/martin-minghetti/conversation-to-action) | Watches Slack, Discord, and WhatsApp threads — extracts bugs, features, tasks, and decisions with Claude. Deduplicates against your Linear/Notion backlog. Team approves or discards via in-channel buttons. ~$0.03-0.05 per thread. | Next.js 15, Supabase Realtime, Railway |

### Developer Tools

| Project | What it does | Stack |
|---------|-------------|-------|
| [**SkillCam**](https://github.com/martin-minghetti/skillcam) | Turn successful AI agent runs into reusable markdown skills. Reads native session logs from Claude Code and Codex CLI, distills them into SKILL.md files. Works with any LLM or without one (`--no-llm` mode). | TypeScript, npm package — [`npx skillcam distill --latest`](https://www.npmjs.com/package/skillcam) |

### Commercial Demos — Portfolio AR Kit

Production-grade fullstack apps shipped fast. Built as showcase pieces for Argentine SMB clients (alternative to Tiendanube / Booking-style SaaS). Each one ships with a `PAYMENT_MODE=simulated|production` flag so anyone can test the full Mercado Pago flow without a card. Honest wall-clock time tracked in `BUILD_LOG.md` per repo.

| Project | What it does | Differentiator | Stack |
|---------|-------------|----------------|-------|
| [**Cumbre**](https://github.com/martin-minghetti/cumbre) | Craft brewery (Patagonia) with ERP-lite operations. Catalog, cart, MP Checkout Pro, full purchase flow end-to-end. Atomic order pipeline (TX with `SELECT FOR UPDATE` + FIFO stock allocator + idempotency by `mp_payment_id` UNIQUE), simulated payment flow, admin login. 55 unit + 5 Playwright E2E. CSP / HSTS hardening. [Live](https://cumbre-three.vercel.app). | Atomic stock-aware order pipeline (`applyOrderPaid`) + FIFO batch allocator + DB-enforced idempotency on `mp_payment_id` | Next 16, Drizzle, Neon Postgres, MP Checkout Pro, Resend |
| [**Norhaven Lodge**](https://github.com/martin-minghetti/norhaven-lodge) | Boutique cabin booking site with date-overlap validation, Server Actions, transactional email, and AI-powered semantic search ("something for a couple, with lake view" → ranked cabins). [Live](https://norhaven-lodge.vercel.app). | MP Checkout Pro one-shot + Vercel AI SDK / Gemini 2.5 Flash semantic search | Next 16, Drizzle, Supabase, MP Checkout Pro, Resend |
| [**Cohere**](https://github.com/martin-minghetti/cohere) | Membership platform for professionals (yoga / pilates / coaching) with monthly recurring billing. Customer portal with real cancel / pause / resume, pro dashboard with active subs + MRR, multi-tenant simulated. [Live](https://cohere-six.vercel.app). | MP **Subscriptions** API (`preapproval_plan` + `preapproval`) + recurring webhook events | Next 16, Drizzle, Neon Postgres, MP Subscriptions, Resend |
| [**Bosque**](https://github.com/martin-minghetti/bosque) | E-commerce demo for a Patagonian chocolatier — multi-item cart, shipping calc (3 zones × 3 carriers, derived from postal code), admin panel with order stats, transactional email post-payment. [Live](https://bosque-three.vercel.app). | Server-side cart with HMAC-signed `session_id` cookie + AR shipping engine (Andreani / Correo Argentino / OCA) | Next 16, Drizzle, Neon Postgres, MP Checkout Pro, Resend |
| [**Sur41**](https://github.com/martin-minghetti/sur41) | Travel agency for Bariloche — 14 real excursions, native i18n (ES / EN / PT-BR), 96 SSG pages, AI-generated FLUX 1.1 Pro hero images. Booking flow with HMAC tokens, rate limiting, CSP/HSTS hardening. [Live](https://sur41.vercel.app). | App Router native i18n + Replicate FLUX image generation + production security headers | Next 16, Drizzle, Neon Postgres, MP Checkout Pro, Resend, Replicate |

All five ship with Vitest unit tests and Playwright E2E specs. Webhooks validate HMAC SHA256, enforce timestamp freshness, and idempotency.

### Vertical SaaS

| Project | What it does | Stack |
|---------|-------------|-------|
| [**QuestBoard**](https://questboard-lake.vercel.app) | DDQ (Due Diligence Questionnaire) automation for SaaS mid-market. Upload security questionnaire (EDRM, CAIQ, SIG Lite), AI completes answers grounded in your knowledge base, flags GREEN / YELLOW / RED for review. Pipeline parallelized 5x. Tested on 75q EDRM and 52q CAIQ corpora. Source private. | Next.js 16, Vercel AI SDK, Neon pgvector, Claude API, Clerk, Stripe |

---

## BYOK — Bring Your Own Keys

Every project that calls an LLM uses your own API keys, stored encrypted in your instance. No hosted inference, no usage limits you don't control, no vendor lock-in.

Typical cost per run:

| Project | Cost | Why |
|---------|------|-----|
| SDR Swarm | ~$0.08-0.15 | Sonnet for research/analysis/writing, Haiku for scoring |
| Code Review Orchestrator | ~$0.10-0.20 | Sonnet for security/impact, Haiku for tests/docs |
| Conversation-to-Action | ~$0.03-0.05 | Sonnet for extraction per 10-20 message thread |
| GTM Briefing Copilot | ~$0.06-0.12 | Sonnet x3 for extraction/analysis/messaging, Haiku for verification |
| InvoiceAI | ~$0.05-0.10 | Vision API per invoice |
| modelsentry | $0.00 | Gemini Flash-Lite free tier, GitHub Actions free for public repos |

---

## Tech Stack

```
AI           → Anthropic Claude (direct SDK), Claude Vision, Vercel AI SDK, Gemini API
                Replicate (FLUX 1.1 Pro image gen)
Backend      → FastAPI (Python) · Next.js API Routes + Server Actions (TypeScript)
Frontend     → Next.js 16 App Router · Tailwind CSS v4 · shadcn/ui · native i18n
Data         → Supabase · Neon Postgres · Drizzle ORM · SQLite · Redis
Validation   → Zod
Infra        → Vercel · Railway · GitHub Actions/Pages
Testing      → Vitest · Playwright · Pytest
Scraping     → Playwright · BeautifulSoup · cheerio
Payments     → Mercado Pago Checkout Pro · MP Subscriptions (preapproval) · HMAC webhooks
Integrations → WhatsApp Business API · Google Calendar · Resend (transactional email)
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
