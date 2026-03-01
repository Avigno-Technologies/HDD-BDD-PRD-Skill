# AI Agent App PRD Skill -- Design Document

**Date:** 2026-03-01
**Status:** Approved
**Location:** `ai-agent-app-prd/` (sibling to `hypothesis-driven-prd/`)

---

## Overview

A Claude skill that walks a solo builder through a hypothesis-driven design process for lightweight AI Agent web apps. Each run produces a **build spec PRD** -- structured so Claude Code + installed plugins can immediately scaffold and build the app.

### Target User
Solo builder / indie hacker who handles idea, design, build, and deploy. The skill is a thought partner that covers all angles.

### App Profile
- **Format:** Web app with AI backend (frontend + Vercel + API routes calling AI models)
- **Lifecycle:** Template-based clones -- repeatable pattern, customized per use case
- **AI Capabilities:** Conversational AI + RAG/Knowledge base + Tool-using agents
- **Variability:** Both domain knowledge AND tool/integration palette change per app

### Philosophy
- **Every capability must earn its place.** No RAG, no web scraping, no database unless there's a validated hypothesis demanding it.
- **Falsify before you build.** Each hypothesis includes what would prove it wrong.
- **The PRD is the build contract.** A machine-actionable spec with tech stack, component architecture, and plugin mappings.
- **Template-friendly.** Consistent output structure so clones are comparable and repeatable.

### Scope
The skill handles concept-through-PRD. It does NOT generate code, scaffold projects, or deploy. Its terminal output is a PRD document structured for code generation by other skills/plugins.

---

## 6-Step Workflow

### Step 1: Agent Hypothesis
**Goal:** Define what you're building and why -- as a testable hypothesis.

**Format:**
> "If I build an agent that **[does X]** for **[audience]**, then **[measurable outcome]** because **[rationale]**."
>
> **Falsification:** This hypothesis is wrong if **[failure condition]** within **[timeframe]**.

**Questions asked:**
- What problem does this agent solve? (open-ended)
- Who has this problem most acutely? (guided)
- What's the current alternative? (if they don't use your agent, what do they do?)
- What outcome would prove this is worth building?
- What would prove you wrong?

**Challenge moment:** Pushes back if the hypothesis is too vague, unfalsifiable, or the audience is "everyone."

### Step 2: Capability Hypotheses
**Goal:** For each AI capability, formulate a hypothesis justifying its inclusion.

**Capability menu:**

| Capability | Description | Relevant Plugins |
|---|---|---|
| Conversational AI | Natural language chat interface | Claude API, Agent SDK |
| RAG / Knowledge base | Retrieve & reason over documents | Pinecone, embeddings |
| Web scraping / research | Fetch and process web content | Firecrawl |
| Database operations | Store, query, manage structured data | Supabase |
| Payment processing | Charge users, manage subscriptions | Stripe |
| External API calls | Integrate with third-party services | Custom MCP tools |
| File processing | Upload, parse, transform files | Agent SDK tools |
| Browser automation | Interact with web pages programmatically | Playwright |

**For each selected capability:**
> "Users need **[capability]** because **[rationale]**."
> **Test:** We'll know this is needed when **[evidence]**.
> **Falsification:** This capability is unnecessary if **[condition]**.

**Challenge moment:** "Could you achieve this with a simpler approach?" for every capability.

### Step 3: User Journey Mapping
**Goal:** Define 3-5 core user interactions using JTBD-inspired format.

**Format per journey:**
> **When** [situation/trigger]
> **I want the agent to** [action/behavior]
> **So I can** [outcome/value]
> **Validates:** [which hypothesis]

**Must include:**
- The first thing a user does (onboarding moment)
- The core "aha" interaction (where value is delivered)
- The repeat interaction (what brings them back)
- Edge cases: agent doesn't know, agent is wrong, user goes off-script

**Challenge moment:** Journeys that don't trace back to a hypothesis get flagged.

### Step 4: Stack Selection & Architecture
**Goal:** Map validated capabilities to specific plugins and define component architecture.

**Stack manifest format:**
```
Frontend:     [framework] -> deployed to [Vercel]
AI Backend:   [Claude API / Agent SDK] -> [API routes]
Knowledge:    [Pinecone index] / [none] -> [embedding model]
Data:         [Supabase] / [none] -> [tables needed]
Integrations: [Firecrawl] / [Stripe] / [other] -> [specific tools]
Auth:         [Supabase Auth] / [none] -> [auth flows]
```

**Per component:** Which hypothesis it serves, which plugin implements it, configuration needed.

**Challenge moment:** "You selected N integrations for an MVP. Can we cut to fewer for v1?"

### Step 5: Validation Plan
**Goal:** Define how to test each hypothesis before investing fully.

**Validation methods:**

| Method | Effort | Fidelity | When to Use |
|---|---|---|---|
| Smoke test landing page | Low | Low | Demand validation |
| Wizard of Oz | Medium | High | Complex agent behavior |
| Hardcoded prototype | Medium | Medium | Core interaction testing |
| A/B test | High | High | Optimization after launch |
| Concierge (manual agent) | Low | High | Value proposition testing |

**Output:** Prioritized validation sequence -- what to test first, what can wait.

### Step 6: Generate Build Spec PRD
**Goal:** Output a single markdown document structured for machine consumption.

**PRD structure:**
1. Agent Hypothesis (core hypothesis with falsification)
2. Capability Manifest (table: capability | hypothesis | plugin | config)
3. User Journeys (JTBD journeys with hypothesis traceability)
4. Architecture (tech stack manifest, component map, data flow)
5. Build Sequence (ordered phases referencing specific plugins)
6. Validation Plan (prioritized hypothesis tests)
7. Traceability Matrix (hypothesis -> capability -> journey -> component -> build step)
8. Open Questions & Non-Goals

---

## Conversation Design

### Question Style
- Batches of 2-3 questions per turn
- Multiple choice preferred with "other" escape hatch
- Progressive disclosure: start broad, drill down where user shows depth
- Allow "skip" or "s" for non-critical questions
- Summarize after each step before moving to next

### Challenge Protocol
Acts as a **critical friend:**
- Pushes back on vague hypotheses
- Questions unnecessary complexity
- Suggests simpler alternatives before accepting complex ones
- Never blocks -- if user insists, records the choice and moves on

### Depth Modes

| Mode | Steps | Time | Best For |
|---|---|---|---|
| **Quick** | Compressed -- 1-2 questions per step, minimal challenge | ~10 min | Repeat builders who know what they want |
| **Thorough** | Full question bank, deep challenges, validation planning | ~30 min | New concepts, complex agents, higher stakes |

### Template Memory
- Consistent capability menu across all runs
- Identical PRD section structure for comparability
- Metadata tagging: date, domain, capabilities, stack, status

### Guardrails
- Don't write code
- Don't recommend specific UI designs (defer to frontend-design)
- Prioritize ruthlessly -- push for 1-2 capabilities max in v1
- Challenge "I need AI for this" -- sometimes a simple form is the answer

---

## Build Spec Output Format

### PRD Metadata Block
```yaml
---
agent_name: "[Name]"
version: "v1"
date: "[YYYY-MM-DD]"
domain: "[domain]"
depth_mode: "[quick|thorough]"
capabilities:
  - [list of selected capabilities]
stack:
  frontend: "[framework]"
  ai: "[model/sdk]"
  knowledge: "[pinecone|none]"
  data: "[supabase|none]"
  integrations: ["[list]"]
  deploy: "vercel"
hypotheses_count: [N]
validated: false
status: "draft"
---
```

### Plugin Integration Mapping

| PRD Section | Feeds Into | Plugin/Skill |
|---|---|---|
| Architecture -> Frontend | Scaffold UI | `frontend-design` |
| Architecture -> AI Backend | Create Agent SDK app | `agent-sdk-dev:new-sdk-app` |
| Capability Manifest -> RAG | Create Pinecone index + upload data | `pinecone:quickstart`, `pinecone:assistant-create` |
| Capability Manifest -> Data | Set up Supabase tables & auth | `supabase` (MCP) |
| Capability Manifest -> Payments | Configure Stripe integration | `stripe:stripe-best-practices` |
| Capability Manifest -> Web Scraping | Configure Firecrawl | `firecrawl:firecrawl-cli` |
| Build Sequence | Step-by-step execution plan | `superpowers:writing-plans` -> `superpowers:executing-plans` |
| Validation Plan | Test strategy | `superpowers:test-driven-development` |
| Deployment | Push to production | `vercel:deploy` |
| Project tracking | Create task board | `Notion:\tasks:setup` |

### Build Sequence Format
Ordered phases with dependencies:
- **Phase 1: Foundation** (no dependencies) -- scaffold, set up services
- **Phase 2: Core Agent** (depends on Phase 1) -- implement AI backend, chat API, RAG
- **Phase 3: Integration** (depends on Phase 2) -- connect tools, auth, extras
- **Phase 4: Deploy & Validate** -- push to Vercel, run validation experiments

### Traceability Matrix Format
Connects every layer: Hypothesis -> Capability -> Journey -> Component -> Build Step -> Validation

---

## Skill File Structure

```
ai-agent-app-prd/
├── SKILL.md                      # Main workflow (the 6 steps)
├── assets/
│   ├── build-spec-template.md    # PRD output template
│   └── capability-menu.md        # Reference: all capabilities + plugin mappings
└── references/
    ├── hypothesis-patterns.md    # Patterns for agent hypotheses
    └── validation-methods.md     # Experiment types for AI agents
```

Lives as a sibling to `hypothesis-driven-prd/` in the same repository.

---

## Decisions Log

| Decision | Choice | Rationale |
|---|---|---|
| Approach | Hypothesis-First Agent Design | Rigorous validation prevents wasted builds across many clones |
| Framework | Fresh start, inspired by HDD | AI agents need different discovery than traditional products |
| Delivery | Web app + AI backend | Target use case: deployed web apps with Claude-powered backends |
| Output | Build spec PRD (machine-readable) | Enables direct handoff to Claude Code for code generation |
| Target user | Solo builder / indie hacker | Skill = thought partner that covers all angles |
| Depth modes | Quick (~10 min) and Thorough (~30 min) | Flexibility for repeat builders vs. new concepts |
| Repo location | Same repo, sibling directory | Shared lineage, cross-reference potential |
