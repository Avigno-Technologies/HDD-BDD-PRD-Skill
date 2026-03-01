---
name: ai-agent-app-prd
description: Create build-spec PRDs for lightweight AI Agent web apps using hypothesis-driven design. Use when users want to spec AI agent apps, plan AI-powered web applications, design conversational AI tools, or create PRDs for apps that use Claude, RAG, tool-use, or other AI capabilities. Walks through Agent Hypothesis → Capability Hypotheses → User Journeys → Stack Selection (LLM model + testing frameworks) → Cost Modeling & Unit Economics → Validation Plan → Build Spec PRD. Outputs a machine-readable PRD structured for direct code generation by Claude Code plugins.
---

# AI Agent App PRD Generator

Generate build-spec PRDs for lightweight AI agent web apps — from hypothesis to executable spec.

## Workflow Overview

```
1. Agent Hypothesis → Define what you're building and why
2. Capability Hypotheses → Justify each AI capability
3. User Journey Mapping → Define core interactions (JTBD)
4. Stack Selection → Map capabilities to plugins, select LLM model, choose testing frameworks
5. Cost Modeling → Calculate per-user unit economics and backend costs
6. Validation Plan → Design experiments before building
7. Generate Build Spec PRD → Machine-readable output
```

## Step 0: Initialization

Ask the user:

```
Before we start building your agent PRD, a few setup questions:

1. How much time do you have?
   A. Quick (~10 min) — I have a clear idea, just need structure
   B. Thorough (~30 min) — I'm still forming ideas, walk me through everything

2. Where are you starting from?
   A. I have an existing concept — I can describe the agent
   B. Starting from scratch — I have a problem but no solution yet
   C. I have a partial spec or notes — I'll share them

3. Any constraints I should know about?
   A. Technical (specific stack, existing codebase)
   B. Timeline (launch deadline)
   C. Budget (API cost limits, hosting limits)
   D. No constraints yet
```

Set expectations: "We'll walk through 7 steps: define your agent hypothesis, justify each capability, map user journeys, select a stack (including LLM model and testing frameworks), model your per-user costs, plan validation experiments, and generate a machine-readable build spec PRD. The output is a structured document you can hand to Claude Code to scaffold and build the app."

Adapt depth based on mode. For Quick mode, present likely defaults for confirmation rather than open-ended discovery. For Thorough mode, explore each step with full probing questions.

## Step 1: Agent Hypothesis

Define the core hypothesis for the agent. Present the format:

> "If I build an agent that **[does X]** for **[audience]**, then **[measurable outcome]** because **[rationale]**."
>
> **Falsification:** This hypothesis is wrong if **[failure condition]** within **[timeframe]**.

Ask in two batches:

**Batch 1:**
```
Let's define what this agent does and who it's for:

1. What problem does this agent solve?
   A. Information retrieval — finding and synthesizing knowledge
   B. Task automation — doing repetitive work
   C. Decision support — helping people choose
   D. Creative assistance — helping people create
   E. Customer interaction — handling conversations
   F. Something else — I'll describe

2. Who has this problem most acutely?
   A. I have a specific audience in mind — I'll describe them
   B. I have a general category (developers, marketers, etc.)
   C. Not sure yet — help me narrow it down

3. What's the current alternative? How do people solve this today?
```

**Batch 2:**
```
Now let's make it testable:

1. What outcome would prove this agent is worth building?
   A. Time saved (I can quantify it)
   B. Cost reduced (I can quantify it)
   C. Quality improved (I can measure it)
   D. New capability (not possible before)
   E. I'm not sure how to measure it yet

2. What would prove you wrong? What failure would make you stop?
```

**Challenge protocol:**
- If the hypothesis uses "everyone" or "anyone" as the audience, push back: "Can you name 5 specific people who have this problem today?"
- If the outcome is not measurable, push back: "How would you know if this agent is working? What number changes?"
- If there is no falsification condition, push back: "What would make you abandon this idea? If you can't answer that, the hypothesis isn't testable."

See `references/hypothesis-patterns.md` for agent-type-specific templates (information retrieval, task automation, decision support, creative assistant, customer-facing).

Summarize: reflect back the formatted hypothesis and ask for confirmation before proceeding.

(Type 'skip' to use a placeholder hypothesis and refine later)

## Step 2: Capability Hypotheses

Identify which AI capabilities the agent needs and justify each one. Present the capability menu (see `assets/capability-menu.md` for full plugin details):

| # | Capability | Description | Simpler Alternative |
|---|-----------|-------------|---------------------|
| 1 | Conversational AI | Natural language chat interface | -- (core, always included) |
| 2 | RAG / Knowledge base | Retrieve and reason over documents | Long system prompt |
| 3 | Web scraping / research | Fetch and process web content | Manual curation |
| 4 | Database operations | Store, query, manage structured data | Local storage |
| 5 | Payment processing | Charge users, manage subscriptions | Gumroad link |
| 6 | External API calls | Integrate with third-party services | Hardcoded data |
| 7 | File processing | Upload, parse, transform files | Copy-paste text |
| 8 | Browser automation | Interact with web pages | Manual screenshots |

```
Let's identify which capabilities your agent needs:

1. Which of these does your agent need? (Select all that apply)
   A. RAG / Knowledge base — retrieve and reason over documents
   B. Web scraping — fetch and process live web content
   C. Database — store and query persistent data
   D. Payment processing — charge users
   E. External APIs — integrate with third-party services
   F. File processing — upload and parse files
   G. Browser automation — interact with web pages
   H. None of the above — conversational AI only

2. For your primary capability, how critical is it?
   A. Essential — the agent can't deliver value without it
   B. Important — significantly better with it
   C. Nice to have — adds polish but not core
   D. Not sure yet — let's explore

(Type 'skip' to use the default minimal stack)
```

**For Quick mode:** Present common stack combinations from `assets/capability-menu.md` and ask "Which is closest?"
- Minimal Chat Agent (conversational AI only)
- Knowledge Agent (conversational AI + RAG)
- Research Agent (conversational AI + RAG + web scraping)
- Full-Stack Agent (conversational AI + RAG + web scraping + database + payments)
- Internal Tool Agent (conversational AI + database + browser automation)

**For each selected capability, challenge:**
- "Why do users need [capability]? What happens if you leave it out?"
- "What evidence would show this capability is actually needed?"
- "Could you achieve this with [simpler alternative] instead?"

Capture each as:

> **CH[n]: [Capability Name]**
> Users need **[capability]** because **[rationale]**.
> **Falsification:** This capability is unnecessary if **[condition]**.

See `references/hypothesis-patterns.md` for capability-specific hypothesis templates.

**Challenge protocol:**
- If more than 3 capabilities are selected, push back: "You selected [N] capabilities. For a v1, can we cut to 1-2 beyond conversational AI? Fewer capabilities means faster time-to-validation."
- If the user selects a capability but cannot articulate why, suggest the simpler alternative.

Summarize all capability hypotheses before proceeding.

(Type 'skip' to move to the next section)

## Step 3: User Journey Mapping

Define 3-5 core user journeys using the Jobs-to-be-Done format. Each journey should trace back to a hypothesis.

Explain: "A user journey describes a specific situation where someone interacts with your agent. We'll capture the trigger, the agent's behavior, and the value delivered."

Guide through journeys one at a time:

```
Let's map your core user journeys:

1. What's the first thing a user does when they encounter your agent?
   (This is the onboarding journey — how they go from zero to first value)

2. What's the core "aha" moment? The interaction that delivers the agent's
   primary value?

3. What brings them back for a second session?
   A. New data or content to process
   B. Ongoing workflow they repeat
   C. They wouldn't come back — it's a one-time tool
   D. Not sure yet
```

For Thorough mode, also explore edge cases:

```
4. What happens when the agent doesn't know the answer?
   A. It should say "I don't know" and suggest alternatives
   B. It should escalate to a human
   C. It should ask clarifying questions
   D. Not sure — let's discuss

5. What happens when the agent is wrong?
   A. User corrects it and continues
   B. It could cause real harm — we need safeguards
   C. Minor inconvenience — user retries
   D. Not sure — let's discuss
```

Capture each journey as:

> **J[n]: [Journey Name]**
>
> **When** [situation/trigger]
> **I want the agent to** [action/behavior]
> **So I can** [outcome/value]
>
> **Validates:** H1, CH[n]
> **Edge case:** [what happens when the agent fails here]

**Challenge protocol:**
- If a journey does not trace to any hypothesis, flag it: "This journey doesn't connect to your core hypothesis or any capability hypothesis. Should we add a hypothesis for it, or cut the journey?"
- If no edge-case journeys are defined (Thorough mode), prompt: "What happens when the agent fails? Users will judge your agent by its worst moment, not its best."

Summarize all journeys before proceeding.

(Type 'skip' to move to the next section)

## Step 4: Stack Selection and Architecture

Map validated capabilities to specific technologies and plugins. Present the stack manifest template (see `assets/capability-menu.md` for plugin detail cards):

```
Frontend:     [next.js | react | other] -> deployed to [Vercel]
AI Backend:   [Claude API | Agent SDK (Python) | Agent SDK (TypeScript)] -> [API routes | standalone]
LLM Model:    [opus | sonnet | haiku] -> [single | tiered: list routing strategy]
Knowledge:    [Pinecone index | none] -> [multilingual-e5-large | llama-text-embed-v2 | none]
Data:         [Supabase | none] -> [tables: list | none]
Integrations: [Firecrawl | Stripe | Playwright | custom MCP | none]
Auth:         [Supabase Auth | none] -> [email/password | OAuth | magic link | none]
Testing:      [vitest | jest | pytest] -> unit + integration
E2E:          [playwright | cypress | none] -> user journey coverage
Regression:   [CI pipeline | prompt suite | manual | none] -> change protection
Deploy:       Vercel
```

**For Quick mode:** Auto-fill from the closest common stack combination identified in Step 2. Present for confirmation.

**For Thorough mode:** Walk through each layer:

```
Let's select your stack. For each layer, I'll recommend based on your
capabilities and you can confirm or override:

1. Frontend framework?
   A. Next.js (recommended — API routes + SSR in one framework)
   B. React (if you have a separate backend)
   C. Other — I'll specify

2. AI Backend?
   A. Agent SDK (TypeScript) — recommended with Next.js
   B. Agent SDK (Python) — if you prefer Python
   C. Claude API directly — simplest, no tool use
```

### LLM Model Selection

Guide the user to choose the right model for their agent. Present the model menu (see `assets/capability-menu.md` for detailed model cards):

```
Which LLM model should power your agent?

1. Primary model (for main agent interactions):
   A. Claude Opus — highest capability, best for complex reasoning, highest cost
   B. Claude Sonnet — strong balance of capability and cost (recommended for most agents)
   C. Claude Haiku — fastest, cheapest, best for simple/high-volume tasks
   D. Not sure — help me choose

2. Do you need different models for different tasks?
   A. Single model for everything (simplest)
   B. Tiered: fast model for simple tasks, powerful model for complex ones
   C. Not sure — let's discuss the trade-offs
```

**Challenge protocol:**
- If the user picks Opus for a simple FAQ bot, push back: "Opus is powerful but expensive. For straightforward Q&A, Sonnet or Haiku could deliver similar quality at a fraction of the cost. What makes this agent need Opus-level reasoning?"
- If the user picks Haiku for complex multi-step reasoning, warn: "Haiku is fast and cheap but may struggle with complex tool orchestration or nuanced analysis. Consider Sonnet as your primary model with Haiku for simple subtasks."

### Testing Framework Selection

Every agent app needs testing from day one. Guide the user through testing infrastructure:

```
Let's set up testing for your agent:

1. Unit testing framework:
   A. Vitest (recommended for Next.js/TypeScript)
   B. Jest (widely used, good ecosystem)
   C. Pytest (if Python backend)
   D. I have an existing preference — I'll specify

2. End-to-end testing:
   A. Playwright (recommended — also available as a plugin for browser automation)
   B. Cypress (popular alternative)
   C. Skip for now — I'll add e2e later
   D. Not sure — recommend something

3. What should the testing strategy cover?
   A. Core agent responses (does the agent answer correctly?)
   B. Tool integration (do API calls, DB queries, and scraping work?)
   C. User flows (can a user complete key journeys end-to-end?)
   D. Regression (do existing features still work after changes?)
   E. All of the above (recommended)
```

**For Quick mode:** Default to Vitest + Playwright + all coverage areas. Present for confirmation.

Capture testing decisions as part of the stack manifest:
```
Testing:      [vitest | jest | pytest] -> unit + integration
E2E:          [playwright | cypress | none] -> user journey coverage
Regression:   [CI pipeline | manual | none] -> change protection
```

**Challenge protocol:**
- If the user skips e2e testing, warn: "AI agents are especially prone to regression because model behavior can shift. E2e tests on your core journeys are the cheapest insurance against shipping a broken agent."
- If no regression strategy, push: "How will you know if a prompt change breaks an existing feature? Even a simple test suite that validates your top 3 journeys prevents embarrassing regressions."

### Component Map

For the selected stack, define:
- **Frontend components:** What the user sees and interacts with (chat interface, input forms, result displays)
- **API routes:** Backend endpoints that connect frontend to AI and tools
- **Data models:** Tables and schemas if database is included

### Data Flow

Define the request-response path:

```
[User input]
  -> [Frontend component]
    -> [API route]
      -> [AI model/SDK]
        -> [Tools/integrations]
      <- [AI response]
    <- [Processed result]
  <- [Rendered output]
```

**Challenge protocol:**
- If the stack includes more than 3 integrations, push back: "You have [N] integrations. Each one adds deployment complexity, API key management, and failure modes. Can we cut to fewer for v1?"
- If the user selects Python backend with Next.js frontend, note the trade-off: "This means two separate deployment targets and no shared types. Are you sure?"

Summarize the complete stack manifest before proceeding.

(Type 'skip' to move to the next section)

## Step 5: Cost Modeling and Unit Economics

Calculate the per-user cost of running the agent and reality-check the business model. **This is a viability gate** — if costs exceed what users will pay, the PRD must be revised before proceeding.

Present the cost framework:

```
Let's figure out what your agent costs to run per user per month.

We'll estimate costs for each component of your stack:
```

### Cost Components

Walk through each selected stack component and estimate monthly per-user costs:

```
For each component, I'll estimate based on your stack selection:

1. LLM API costs (usually the largest expense):
   - Model: [selected model]
   - Estimated tokens per interaction: [input tokens + output tokens]
   - Estimated interactions per user per month: [number]
   - Cost per 1M tokens: [input price / output price]
   → Estimated LLM cost per user/month: $[X]

2. Embedding / RAG costs (if applicable):
   - Embedding model: [selected model]
   - Queries per user per month: [number]
   - Index storage (Pinecone): [tier/plan]
   → Estimated RAG cost per user/month: $[X]

3. Infrastructure costs:
   - Vercel hosting: [plan/tier]
   - Supabase (if applicable): [plan/tier]
   - Firecrawl (if applicable): [plan/tier]
   → Estimated infra cost per user/month: $[X]

4. Third-party API costs (if applicable):
   - [Service]: [per-call or per-month cost]
   → Estimated API cost per user/month: $[X]
```

### Unit Economics Summary

Present a clear summary table:

```
UNIT ECONOMICS PER USER/MONTH:

| Cost Component | Monthly Cost |
|----------------|-------------|
| LLM API calls | $[X] |
| Embeddings / RAG | $[X] |
| Infrastructure | $[X] |
| Third-party APIs | $[X] |
| TOTAL COST PER USER | $[X] |

Now let's check viability:

1. What would you charge per user per month?
   A. Free (ad-supported or lead gen)
   B. $0-10/month (consumer price point)
   C. $10-50/month (prosumer/SMB)
   D. $50-200/month (professional/enterprise)
   E. Usage-based pricing (per query, per document, etc.)
   F. Not sure yet — help me think about this

2. Based on these costs, your gross margin would be approximately:
   Revenue: $[price] - Cost: $[total] = Margin: $[margin] ([X]%)
```

### Viability Gate

**This is a hard checkpoint.** If costs exceed projected revenue, the skill MUST flag it:

**Viable (margin > 50%):** "Your unit economics look healthy. A $[price] price point with $[cost] costs gives you [X]% gross margin. Proceed to validation."

**Marginal (margin 20-50%):** "Your margins are thin at [X]%. Consider: (a) switching to a cheaper LLM model for routine tasks, (b) reducing the number of interactions per session, (c) caching frequent responses, or (d) raising your price point. Which approach would you like to explore?"

**Unviable (margin < 20% or negative):** "**Warning: Your agent costs $[cost]/user/month but you'd charge $[price]/user/month. This is not a sustainable business.** Before we proceed, you must either: (a) cut capabilities to reduce costs, (b) switch to a cheaper model, (c) increase your price point, or (d) find a different monetization strategy. Which would you like to explore?"

```
Your agent costs $[X]/user/month to run.
You plan to charge $[Y]/user/month.
Gross margin: [Z]%.

Is this viable?
   A. Yes — margins are acceptable, proceed
   B. No — let's cut costs (revisit stack/model selection)
   C. No — let's increase price (revisit value proposition)
   D. No — let's change the monetization model
```

**For Quick mode:** Auto-estimate costs from typical usage patterns for the selected stack combination. Present the summary for confirmation.

**For Thorough mode:** Walk through each cost component with detailed assumptions. Ask about expected usage patterns (interactions per session, sessions per month, document volume for RAG).

### Cost Reduction Levers

If costs need to come down, present options:

| Lever | Savings | Trade-off |
|-------|---------|-----------|
| Downgrade primary model (e.g., Opus → Sonnet) | 50-80% on LLM costs | Reduced reasoning quality |
| Use tiered models (Haiku for simple, Sonnet for complex) | 30-60% on LLM costs | Added routing complexity |
| Cache frequent responses | 20-50% on LLM costs | Stale answers for cached queries |
| Reduce RAG queries per interaction | 20-40% on RAG costs | Potentially less relevant context |
| Limit interactions per user/month | Variable | User experience trade-off |
| Move from managed services to self-hosted | 30-70% on infra | Operational complexity |

**Challenge protocol:**
- If the user insists on proceeding with negative margins, record it but flag: "Noted. The PRD will include a cost warning. This business model requires either future cost reduction or price increases to be sustainable."
- If the user's price point is unrealistically high for their target audience, push back: "Your target audience is [segment]. Do similar tools in this space charge $[price]? What's the evidence they'd pay this?"
- Always ask: "Would you use this agent if you had to pay $[price]/month for it?"

Summarize the cost model before proceeding.

(Type 'skip' to use rough estimates and move on)

## Step 6: Validation Plan

Design experiments to test hypotheses before building the full agent. Reference `references/validation-methods.md` for detailed method descriptions and the sequencing guide.

Present the validation methods:

| Method | Effort | Fidelity | When to Use |
|--------|--------|----------|-------------|
| Smoke test landing page | Low | Low | Validate demand before building |
| Wizard of Oz | Medium | High | Test complex agent behavior with humans |
| Hardcoded prototype | Medium | Medium | Test core interaction patterns |
| Concierge (manual agent) | Low | High | Test value proposition manually |
| A/B test | High | High | Optimize after launch |
| Prompt-only MVP | Low | Medium | Test if AI adds value at all |
| Fake door test | Low | Low | Test feature demand within agent |

For each hypothesis (agent hypothesis + capability hypotheses), ask:

```
For your core hypothesis (H1), how would you validate it before building?

1. Which validation method fits best?
   A. Smoke test landing page — measure demand with a signup page
   B. Wizard of Oz — you manually play the agent role
   C. Prompt-only MVP — test with just a system prompt, no tools
   D. Concierge — manually deliver what the agent would
   E. Hardcoded prototype — scripted responses for top scenarios
   F. Not sure — recommend something

2. What's your success threshold?
   A. I have a specific number in mind
   B. Help me define one
```

**For Quick mode:** Auto-suggest "prompt-only MVP" as the default validation method for the agent hypothesis, and "fake door test" for capability hypotheses. Present for confirmation.

**Validation sequence:** Order experiments cheapest-first. Recommend:
1. Demand validation (smoke test or fake door) — Week 1
2. Value validation (concierge or Wizard of Oz) — Week 2-3
3. AI capability validation (prompt-only MVP) — Week 3-4
4. Full product optimization (A/B test) — Post-launch

**Challenge protocol:**
- If the user jumps straight to A/B testing, push back: "A/B tests require traffic and a working product. Can you validate demand with a cheaper method first?"
- If the user cannot define a success metric, help: "What number would make you confident enough to spend a month building this?"
- "Can you validate [hypothesis] with a simpler method? The goal is to disprove bad ideas fast, not prove good ideas slowly."

Summarize the validation plan before proceeding.

(Type 'skip' to move to the next section)

## Step 7: Generate Build Spec PRD

Announce: "Generating your build spec PRD. This is a machine-readable document structured for direct use with Claude Code plugins."

Use the template from `assets/build-spec-template.md`. Fill in all sections from the preceding steps:

1. **YAML metadata** — agent name, version, date, domain, depth mode, capabilities list, stack summary, model selection, testing stack, hypothesis count, status
2. **Agent hypothesis** — formatted hypothesis from Step 1
3. **Capability hypotheses** — all CH statements from Step 2
4. **Capability manifest** — table mapping capabilities to plugins and config from Steps 2 and 4
5. **User journeys** — all JTBD journeys from Step 3 with hypothesis traceability
6. **Architecture** — tech stack table (including LLM model selection and testing infrastructure), component map, API routes, data models, data flow from Step 4
7. **Cost model** — per-user unit economics, margin analysis, and viability assessment from Step 5
8. **Build sequence** — auto-generated from the stack, ordered by dependency:
   - Phase 1: Foundation (scaffold project, configure AI backend, set up testing)
   - Phase 2: Core Agent (implement primary capability, build chat UI)
   - Phase 3: Integration (add secondary capabilities, connect tools)
   - Phase 4: Deploy and Validate (deploy to Vercel, run validation experiments)
9. **Validation plan** — experiment designs from Step 6
10. **Traceability matrix** — auto-generated table connecting hypotheses to capabilities, journeys, components, build steps, and validation methods
11. **Open questions** — collected throughout the conversation
12. **Non-goals** — collected throughout the conversation

**Output location:** Ask the user for a preferred directory. Default to the current working directory with filename `prd-[agent-name].md`.

After output: "This PRD is structured as a build spec. You can use it with Claude Code to scaffold and build the app. The build sequence in Section 8 tells you (or Claude Code) what to build in what order. The cost model in Section 7 flags any unit economics concerns."

## Output

- **Format:** Markdown
- **Location:** User-specified or current working directory
- **Filename:** `prd-[agent-name].md`

## Conversation Guidelines

1. **Ask questions in batches of 2-3** with lettered options for easy response
2. **Allow skipping** any section with "skip" or "s"
3. **Summarize after each step** — reflect back decisions for confirmation before moving to the next step
4. **Adapt depth** based on mode: Quick mode presents defaults for confirmation, Thorough mode explores with open-ended questions
5. **Don't write code** — PRD generation is the deliverable, not implementation
6. **Challenge weak hypotheses** — if a hypothesis is vague or unfalsifiable, push back with specific objections
7. **Prioritize ruthlessly** — push for 1-2 capabilities max beyond conversational AI in v1
8. **Challenge "I need AI for this"** — sometimes a form, a spreadsheet, or a static page is the right answer
9. **Never block** — if the user insists after a challenge, record their choice and move on
10. **Reference supporting files** — point users to `references/hypothesis-patterns.md` for hypothesis templates and `references/validation-methods.md` for experiment design guidance

## Example Question Format

```
Let's explore your Value Proposition:

1. What core problem does this agent solve for your target user?
   A. They waste time on a repetitive task the agent could automate
   B. They lack expertise the agent could provide on demand
   C. They need real-time information the agent could fetch and synthesize
   D. Something else — I'll describe

2. Why would someone use an AI agent instead of their current approach?
   A. It's significantly faster
   B. It's available 24/7 (no waiting for a human)
   C. It does something not possible without AI
   D. The current solution is painful enough to try anything
   E. Multiple reasons — I'll explain

(Type 'skip' to move to the next section if you have clarity here)
```
