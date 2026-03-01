---
name: ai-agent-app-prd
description: Create build-spec PRDs for lightweight AI Agent web apps using hypothesis-driven design. Use when users want to spec AI agent apps, plan AI-powered web applications, design conversational AI tools, or create PRDs for apps that use Claude, RAG, tool-use, or other AI capabilities. Walks through Agent Hypothesis → Capability Hypotheses → User Journeys → Stack Selection → Validation Plan → Build Spec PRD. Outputs a machine-readable PRD structured for direct code generation by Claude Code plugins.
---

# AI Agent App PRD Generator

Generate build-spec PRDs for lightweight AI agent web apps — from hypothesis to executable spec.

## Workflow Overview

```
1. Agent Hypothesis → Define what you're building and why
2. Capability Hypotheses → Justify each AI capability
3. User Journey Mapping → Define core interactions (JTBD)
4. Stack Selection → Map capabilities to plugins
5. Validation Plan → Design experiments before building
6. Generate Build Spec PRD → Machine-readable output
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

Set expectations: "We'll walk through 6 steps: define your agent hypothesis, justify each capability, map user journeys, select a stack, plan validation experiments, and generate a machine-readable build spec PRD. The output is a structured document you can hand to Claude Code to scaffold and build the app."

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
Knowledge:    [Pinecone index | none] -> [multilingual-e5-large | llama-text-embed-v2 | none]
Data:         [Supabase | none] -> [tables: list | none]
Integrations: [Firecrawl | Stripe | Playwright | custom MCP | none]
Auth:         [Supabase Auth | none] -> [email/password | OAuth | magic link | none]
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

## Step 5: Validation Plan

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

## Step 6: Generate Build Spec PRD

Announce: "Generating your build spec PRD. This is a machine-readable document structured for direct use with Claude Code plugins."

Use the template from `assets/build-spec-template.md`. Fill in all sections from the preceding steps:

1. **YAML metadata** — agent name, version, date, domain, depth mode, capabilities list, stack summary, hypothesis count, status
2. **Agent hypothesis** — formatted hypothesis from Step 1
3. **Capability hypotheses** — all CH statements from Step 2
4. **Capability manifest** — table mapping capabilities to plugins and config from Steps 2 and 4
5. **User journeys** — all JTBD journeys from Step 3 with hypothesis traceability
6. **Architecture** — tech stack table, component map, API routes, data models, data flow from Step 4
7. **Build sequence** — auto-generated from the stack, ordered by dependency:
   - Phase 1: Foundation (scaffold project, configure AI backend)
   - Phase 2: Core Agent (implement primary capability, build chat UI)
   - Phase 3: Integration (add secondary capabilities, connect tools)
   - Phase 4: Deploy and Validate (deploy to Vercel, run validation experiments)
8. **Validation plan** — experiment designs from Step 5
9. **Traceability matrix** — auto-generated table connecting hypotheses to capabilities, journeys, components, build steps, and validation methods
10. **Open questions** — collected throughout the conversation
11. **Non-goals** — collected throughout the conversation

**Output location:** Ask the user for a preferred directory. Default to the current working directory with filename `prd-[agent-name].md`.

After output: "This PRD is structured as a build spec. You can use it with Claude Code to scaffold and build the app. The build sequence in Section 5 tells you (or Claude Code) what to build in what order."

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
