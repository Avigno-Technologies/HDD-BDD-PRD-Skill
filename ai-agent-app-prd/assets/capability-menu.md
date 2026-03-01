# Capability Menu and Plugin Mapping

Canonical reference for Step 2 (Capability Hypotheses) and Step 4 (Stack Selection) of the AI Agent App PRD skill. Use this to map capabilities to plugins, choose your stack, and identify simpler alternatives that may be sufficient for your MVP.

---

## Capability Menu

The master list of capabilities an AI agent app can include. Every capability is optional except Conversational AI (which is the agent itself). For each capability, write a hypothesis justifying its inclusion before adding it to your stack. See `references/hypothesis-patterns.md` for templates.

| # | Capability | Description | Plugins | Simpler Alternative |
|---|-----------|-------------|---------|---------------------|
| 1 | Conversational AI | Natural language chat interface | Claude API, `agent-sdk-dev:new-sdk-app` | -- (core, always included) |
| 2 | RAG / Knowledge base | Retrieve and reason over documents | `pinecone:assistant-create`, `pinecone:query`, embeddings | Long system prompt with examples |
| 3 | Web scraping / research | Fetch and process web content | `firecrawl:firecrawl-cli` (scrape, crawl, search) | Manual curation, static data |
| 4 | Database operations | Store, query, manage structured data | `supabase` MCP (SQL, auth, storage, realtime) | Local storage, JSON files |
| 5 | Payment processing | Charge users, manage subscriptions | `stripe:stripe-best-practices`, Stripe API | Gumroad link, manual invoicing |
| 6 | External API calls | Integrate with third-party services | Custom MCP tools, Agent SDK tool definitions | Hardcoded data, manual process |
| 7 | File processing | Upload, parse, transform files | Agent SDK tools, Supabase storage | Copy-paste text input |
| 8 | Browser automation | Interact with web pages programmatically | `playwright` MCP (screenshots, forms, clicks) | Manual screenshots, user-provided data |

### How to use this table

1. Start with Conversational AI. It is always included.
2. For each additional capability, write a capability hypothesis (Step 2) that justifies its presence.
3. If you cannot write a convincing hypothesis, use the simpler alternative instead.
4. Fewer capabilities means faster time-to-validation. Add complexity only when the hypothesis demands it.

---

## Plugin Detail Cards

Configuration and usage details for each plugin available to the skill. Use these cards during Step 4 (Stack Selection) to understand what each plugin provides and when to include it.

---

### 1. `agent-sdk-dev` -- Agent SDK Scaffolding

**What it does:** Scaffolds a new AI agent application using the Anthropic Agent SDK in Python or TypeScript. Generates project structure, dependencies, and boilerplate so you start with a working agent rather than an empty directory.

**Key skills/commands:**
- `new-sdk-app` -- Generate a new Agent SDK project (Python or TypeScript)
- Configures Claude API integration, tool definitions, and conversation loop

**Configuration required:**
- Anthropic API key (`ANTHROPIC_API_KEY`)
- Language choice (Python or TypeScript)
- Project name and directory

**When to include:** Always. This is the foundation of every agent app built with this skill.

**When to skip:** Never. If you are building an agent app, you need the SDK scaffolding.

---

### 2. `pinecone` -- Vector DB + RAG Assistants

**What it does:** Provides vector database infrastructure for Retrieval-Augmented Generation. Creates indexes, embeds documents, and performs semantic search so your agent can reason over large or frequently changing knowledge bases.

**Key skills/commands:**
- `assistant-create` -- Create a new Pinecone assistant with an index
- `query` / `search-records` -- Semantic search over indexed documents
- `upsert-records` -- Add or update documents in an index
- `rerank-documents` -- Re-rank search results for relevance

**Configuration required:**
- Pinecone API key (`PINECONE_API_KEY`)
- Embedding model choice (`multilingual-e5-large` or `llama-text-embed-v2`)
- Index name and cloud region

**When to include:** The source material exceeds 50,000 tokens, changes more than once per month, or users need answers grounded in specific documents.

**When to skip:** The knowledge fits in a 4,000-token system prompt and is stable quarter-over-quarter. Start with a long system prompt and graduate to RAG when you hit the limits.

---

### 3. `firecrawl` -- Web Scraping and Crawling

**What it does:** Fetches, scrapes, and crawls web content so your agent can access information from websites that do not offer APIs. Converts web pages to clean markdown and supports multi-page crawling with depth control.

**Key skills/commands:**
- `firecrawl-cli scrape` -- Scrape a single URL to markdown
- `firecrawl-cli crawl` -- Crawl an entire site with configurable depth
- `firecrawl-cli search` -- Search the web and return structured results
- Browser session support for JavaScript-rendered pages

**Configuration required:**
- Firecrawl API key (`FIRECRAWL_API_KEY`)
- Rate limiting configuration (to avoid being blocked)

**When to include:** Users need data that lives on third-party websites with no API and changes frequently. More than 50% of queries require external web data.

**When to skip:** The data is available via a stable API or RSS feed, or you can manually curate a static dataset that is updated on a schedule. Prefer APIs over scraping whenever possible.

---

### 4. `supabase` -- Database + Auth + Storage

**What it does:** Provides a full backend-as-a-service built on PostgreSQL. Handles structured data storage, user authentication, file storage, and real-time subscriptions in a single service, eliminating the need to stitch together multiple providers.

**Key skills/commands:**
- SQL queries via MCP (create tables, insert, select, update, delete)
- Auth management (signup, login, OAuth, magic links)
- Storage (upload, download, signed URLs)
- Realtime subscriptions (listen for changes)

**Configuration required:**
- Supabase project URL and anon/service key
- Database schema design (tables, relationships, RLS policies)
- Auth provider configuration (if using OAuth)

**When to include:** The agent must persist user-specific data across sessions, manage user accounts, or store files. More than 60% of users return for multiple sessions.

**When to skip:** The agent is stateless, single-session, or all data can be stored client-side. Use local storage or JSON files for prototyping and migrate to Supabase when persistence becomes critical.

---

### 5. `stripe` -- Payment Processing

**What it does:** Integrates Stripe for charging users, managing subscriptions, and handling payment-related webhooks. Provides best practices for CheckoutSessions, billing portals, and test-mode development so you do not have to learn Stripe from scratch.

**Key skills/commands:**
- `stripe-best-practices` -- Reference for Checkout, webhooks, and test cards
- CheckoutSession creation and management
- Webhook handling (payment success, subscription changes, failures)
- Test card numbers for development

**Configuration required:**
- Stripe API keys (publishable + secret, test mode first)
- Webhook endpoint URL and signing secret
- Product and price configuration in Stripe Dashboard

**When to include:** The agent's core value loop includes a purchase or subscription step, and your smoke test shows more than 10% of users attempting to pay.

**When to skip:** Fewer than 5% of users would pay for the service, or payment can be handled outside the agent flow (Gumroad link, manual invoicing, "contact us" form). Payment adds compliance, fraud, and refund complexity. Add it only when demand is validated.

---

### 6. `playwright` -- Browser Automation

**What it does:** Gives the agent the ability to interact with web pages programmatically: take screenshots, fill forms, click buttons, and extract data from JavaScript-rendered pages. Useful for automating workflows on sites without APIs.

**Key skills/commands:**
- `browser_snapshot` / `browser_take_screenshot` -- Capture page state
- `browser_click` / `browser_type` -- Interact with page elements
- `browser_fill_form` -- Fill multiple form fields
- `browser_navigate` -- Navigate to URLs
- `browser_evaluate` -- Execute JavaScript on page

**Configuration required:**
- Playwright browser installation (`browser_install`)
- Headless/headed mode configuration
- Timeout and retry settings for unreliable pages

**When to include:** More than 20% of workflow steps require interacting with a website that offers no programmatic access (no API, no webhook, no export).

**When to skip:** Every required action can be performed via an API, or simple web scraping (Firecrawl) is sufficient. Browser automation is fragile and slow. It should be a last resort, not a first choice.

---

### 7. `vercel` -- Deployment

**What it does:** Deploys your agent's frontend and API routes to Vercel's edge network. Handles build, deploy, and preview environments so you can ship updates in minutes rather than managing infrastructure.

**Key skills/commands:**
- `deploy` -- Deploy the current project to Vercel
- `logs` -- View deployment and runtime logs
- `setup` -- Initialize Vercel project configuration
- Preview deployments for pull requests

**Configuration required:**
- Vercel account and project link
- Environment variables (API keys, database URLs)
- Build settings (framework preset, output directory)

**When to include:** Always, if your agent has a web-based frontend. Vercel is the default deployment target for this skill.

**When to skip:** The agent is a CLI tool, a backend-only service, or you have an existing deployment pipeline on another platform.

---

### 8. `frontend-design` -- UI Generation

**What it does:** Generates production-grade frontend interfaces for your agent. Creates responsive layouts, component hierarchies, and styled UI elements so you spend time on agent behavior rather than CSS debugging.

**Key skills/commands:**
- Generate complete page layouts from descriptions
- Create component hierarchies (nav, sidebar, chat, forms)
- Produce styled, responsive markup (Tailwind, CSS modules)
- Build interactive elements (modals, dropdowns, accordions)

**Configuration required:**
- Frontend framework choice (Next.js, React)
- Styling system (Tailwind CSS recommended)
- Design tokens or brand guidelines (optional)

**When to include:** The agent has a user-facing web interface and you want to move fast on UI without hand-coding every component.

**When to skip:** The agent is API-only, CLI-based, or you have a dedicated frontend developer who prefers to build UI manually.

---

### 9. `Notion` -- Project Tracking

**What it does:** Creates and manages Notion pages, databases, and task boards for tracking your agent project. Useful for maintaining PRD documents, sprint boards, and decision logs in a shared workspace.

**Key skills/commands:**
- Create pages and databases with structured schemas
- Query and update database entries
- Search across workspace content
- Manage task status and assignments

**Configuration required:**
- Notion integration token
- Workspace access permissions
- Database schema design (properties, views)

**When to include:** You are working in a team and need shared project tracking, or you want to maintain a living PRD document that updates as hypotheses are validated or falsified.

**When to skip:** You are a solo builder, or your team already has a project tracking tool. Do not add project management overhead for a one-person MVP.

---

### 10. `superpowers` -- Development Workflow

**What it does:** Provides structured development workflows including writing plans, executing plans, test-driven development, and systematic debugging. Turns vague implementation tasks into step-by-step execution sequences.

**Key skills/commands:**
- `writing-plans` -- Break a feature into an implementation plan
- `executing-plans` -- Execute a plan step-by-step with checkpoints
- TDD workflow (write test, implement, refactor)
- Systematic debugging (reproduce, isolate, fix, verify)

**Configuration required:**
- No external configuration required
- Works with any project structure

**When to include:** Always recommended. Structured development workflows reduce errors and make progress visible, especially when building complex multi-capability agents.

**When to skip:** You have an established development workflow that already works well for your team. This plugin adds process, not functionality.

---

## Stack Manifest Template

Fill in this template during Step 4 (Stack Selection) to document your chosen stack. Every bracket is a decision point. Cross-reference your capability hypotheses from Step 2 to justify each choice.

```
Frontend:     [next.js | react | other] -> deployed to [Vercel]
AI Backend:   [Claude API | Agent SDK (Python) | Agent SDK (TypeScript)] -> [API routes | standalone]
Knowledge:    [Pinecone index | none] -> [multilingual-e5-large | llama-text-embed-v2 | none]
Data:         [Supabase | none] -> [tables: list | none]
Integrations: [Firecrawl | Stripe | Playwright | custom MCP | none]
Auth:         [Supabase Auth | none] -> [email/password | OAuth | magic link | none]
Deploy:       Vercel
```

### How to fill in the template

**Frontend:** Choose Next.js unless you have a strong reason not to. It provides API routes (for the AI backend) and server-side rendering (for SEO and performance) in one framework.

**AI Backend:** Use Agent SDK (TypeScript) if your frontend is Next.js (same language, shared types). Use Agent SDK (Python) if you prefer Python or need Python-specific libraries. Use Claude API directly only for the simplest agents that do not need tool use.

**Knowledge:** Choose `none` unless your Step 2 hypothesis specifically justifies RAG. If included, prefer `multilingual-e5-large` for general-purpose use and `llama-text-embed-v2` for long documents and structured content.

**Data:** Choose `none` unless your agent needs persistent, user-specific data. If included, list the tables you expect to need (e.g., `users, conversations, documents`).

**Integrations:** List only the integrations justified by capability hypotheses. An empty list is a valid and often correct answer for an MVP.

**Auth:** Choose `none` unless the agent serves multiple users who need private data. If included, prefer `magic link` for the lowest-friction onboarding.

**Deploy:** Vercel is the default. Override only if you have existing infrastructure requirements.

---

## Common Stack Combinations

Pre-validated combinations for quick selection. Each combination has been tested to work together without configuration conflicts. Use these as starting points and adjust based on your capability hypotheses.

---

### Minimal Chat Agent

**Capabilities included:** Conversational AI

**Plugins used:** `agent-sdk-dev`, `frontend-design`, `vercel`

**Stack manifest:**
```
Frontend:     next.js -> deployed to Vercel
AI Backend:   Agent SDK (TypeScript) -> API routes
Knowledge:    none
Data:         none
Integrations: none
Auth:         none
Deploy:       Vercel
```

**Typical use case:** A single-purpose conversational agent with a well-defined domain that fits entirely in a system prompt (e.g., a customer FAQ bot, a writing assistant, a code explainer).

---

### Knowledge Agent

**Capabilities included:** Conversational AI + RAG / Knowledge base

**Plugins used:** `agent-sdk-dev`, `pinecone`, `frontend-design`, `vercel`

**Stack manifest:**
```
Frontend:     next.js -> deployed to Vercel
AI Backend:   Agent SDK (TypeScript) -> API routes
Knowledge:    Pinecone index -> multilingual-e5-large
Data:         none
Integrations: none
Auth:         none
Deploy:       Vercel
```

**Typical use case:** An agent that answers questions grounded in a large or frequently updated knowledge base (e.g., internal documentation assistant, regulatory compliance advisor, product support agent).

---

### Research Agent

**Capabilities included:** Conversational AI + RAG / Knowledge base + Web scraping / research

**Plugins used:** `agent-sdk-dev`, `pinecone`, `firecrawl`, `frontend-design`, `vercel`

**Stack manifest:**
```
Frontend:     next.js -> deployed to Vercel
AI Backend:   Agent SDK (TypeScript) -> API routes
Knowledge:    Pinecone index -> multilingual-e5-large
Data:         none
Integrations: Firecrawl
Auth:         none
Deploy:       Vercel
```

**Typical use case:** An agent that combines a knowledge base with real-time web research to answer questions that require both stored knowledge and fresh information (e.g., competitive intelligence agent, market research assistant, academic literature reviewer).

---

### Full-Stack Agent

**Capabilities included:** Conversational AI + RAG / Knowledge base + Web scraping / research + Database operations + Payment processing

**Plugins used:** `agent-sdk-dev`, `pinecone`, `firecrawl`, `supabase`, `stripe`, `frontend-design`, `vercel`

**Stack manifest:**
```
Frontend:     next.js -> deployed to Vercel
AI Backend:   Agent SDK (TypeScript) -> API routes
Knowledge:    Pinecone index -> multilingual-e5-large
Data:         Supabase -> tables: users, conversations, documents, subscriptions
Integrations: Firecrawl, Stripe
Auth:         Supabase Auth -> magic link
Deploy:       Vercel
```

**Typical use case:** A commercial agent product with user accounts, persistent data, a knowledge base, web research, and a subscription model (e.g., a paid research-as-a-service agent, an AI-powered SaaS tool, a professional advisory service).

---

### Internal Tool Agent

**Capabilities included:** Conversational AI + Database operations + Browser automation

**Plugins used:** `agent-sdk-dev`, `supabase`, `playwright`, `frontend-design`, `vercel`

**Stack manifest:**
```
Frontend:     next.js -> deployed to Vercel
AI Backend:   Agent SDK (TypeScript) -> API routes
Knowledge:    none
Data:         Supabase -> tables: users, tasks, logs
Integrations: Playwright
Auth:         Supabase Auth -> email/password
Deploy:       Vercel
```

**Typical use case:** An internal agent that automates workflows involving web apps without APIs, stores results in a database, and serves a known set of team members (e.g., data entry automation agent, internal reporting tool, QA testing assistant).
