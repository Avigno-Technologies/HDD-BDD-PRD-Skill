# AI Agent App PRD Skill — Implementation Plan

> **For Claude:** REQUIRED SUB-SKILL: Use superpowers:executing-plans to implement this plan task-by-task.

**Goal:** Create the `ai-agent-app-prd` skill — a 6-step hypothesis-driven workflow that produces machine-readable build spec PRDs for lightweight AI agent web apps.

**Architecture:** Five markdown files in a new `ai-agent-app-prd/` directory, sibling to `hypothesis-driven-prd/`. The main `SKILL.md` drives the conversational workflow. Supporting assets provide templates, reference data, and reusable patterns. No code — all markdown.

**Tech Stack:** Claude skill format (YAML frontmatter + markdown), referencing Claude Code plugins (Agent SDK, Pinecone, Firecrawl, Supabase, Stripe, Vercel, Playwright, Notion, frontend-design, superpowers).

**Design doc:** `docs/plans/2026-03-01-ai-agent-app-prd-skill-design.md`

---

### Task 1: Create directory structure

**Files:**
- Create: `ai-agent-app-prd/` directory
- Create: `ai-agent-app-prd/assets/` directory
- Create: `ai-agent-app-prd/references/` directory

**Step 1: Create the three directories**

```bash
mkdir -p ai-agent-app-prd/assets ai-agent-app-prd/references
```

**Step 2: Verify structure exists**

```bash
ls -R ai-agent-app-prd/
```

Expected: `assets/` and `references/` subdirectories listed.

**Step 3: Commit**

```bash
git add ai-agent-app-prd/
git commit -m "chore: scaffold ai-agent-app-prd directory structure"
```

Note: git won't commit empty directories. This commit may be skipped if git requires files. If so, proceed to Task 2 and include the directories in that commit.

---

### Task 2: Create `references/hypothesis-patterns.md`

**Files:**
- Create: `ai-agent-app-prd/references/hypothesis-patterns.md`

This is the reference file for Step 1 (Agent Hypothesis) and Step 2 (Capability Hypotheses). It provides reusable patterns for formulating testable hypotheses specific to AI agent apps.

**Step 1: Write the file**

Create `ai-agent-app-prd/references/hypothesis-patterns.md` with these sections:

1. **Agent Hypothesis Patterns** — Templates for the core "If I build an agent that [X] for [audience], then [outcome] because [rationale]" format. Include patterns for:
   - Information retrieval agents ("If I build an agent that finds and synthesizes [domain] information...")
   - Task automation agents ("If I build an agent that automates [workflow] for [audience]...")
   - Decision support agents ("If I build an agent that helps [audience] decide [decision type]...")
   - Creative assistant agents ("If I build an agent that helps [audience] create [output type]...")
   - Customer-facing agents ("If I build an agent that handles [interaction type] for [business]...")

2. **Capability Hypothesis Patterns** — Templates for justifying each capability from the capability menu:
   - Conversational AI: "Users need natural language interaction because [rationale]"
   - RAG / Knowledge base: "Users need retrieval over [data type] because [rationale]"
   - Web scraping: "Users need live web data because [rationale]"
   - Database operations: "Users need persistent storage because [rationale]"
   - Payment processing: "Users need to pay because [rationale]"
   - External APIs: "Users need [third-party service] because [rationale]"
   - File processing: "Users need to upload/process [file type] because [rationale]"
   - Browser automation: "Users need programmatic web interaction because [rationale]"

3. **Falsification Patterns** — Common falsification criteria for AI agent hypotheses:
   - "This agent is unnecessary if users can accomplish [task] in under [N] minutes manually"
   - "RAG is unnecessary if the knowledge fits in a [N]-token system prompt"
   - "Web scraping is unnecessary if the data is available via [existing API/feed]"
   - "A database is unnecessary if users don't return more than [N] times"
   - "Payment is unnecessary if [fewer than N%] of users would pay [$X]"

4. **Common Pitfalls** — AI-agent-specific hypothesis mistakes:
   - "AI will make it better" (unfalsifiable)
   - Assuming RAG is needed for everything (often a long prompt suffices)
   - Building for "anyone who uses the internet" (no segment)
   - Confusing "technically possible" with "users want this"
   - Overestimating willingness to interact with AI agents

Each pattern should follow the format:
```
> If [condition],
> then [prediction],
> because [rationale].
>
> Evidence: [specific metric/threshold]
> Falsification: [what would prove this wrong]
```

Reference the existing file `hypothesis-driven-prd/references/bmc-and-hypotheses.md` for tone and structure — adapt the AARRR (Acquisition, Activation, Retention, Revenue, Referral) patterns for AI agent contexts.

**Step 2: Review the file**

Read the file back. Verify:
- All 5 agent hypothesis patterns are present
- All 8 capability hypothesis patterns match the capability menu from the design doc
- Falsification patterns cover each capability
- Common pitfalls section exists
- Formatting is consistent (blockquotes for patterns, tables where appropriate)

**Step 3: Commit**

```bash
git add ai-agent-app-prd/references/hypothesis-patterns.md
git commit -m "feat: add hypothesis patterns reference for AI agent apps"
```

---

### Task 3: Create `references/validation-methods.md`

**Files:**
- Create: `ai-agent-app-prd/references/validation-methods.md`

This is the reference file for Step 5 (Validation Plan). It provides experiment designs tailored to AI agent app validation.

**Step 1: Write the file**

Create `ai-agent-app-prd/references/validation-methods.md` with these sections:

1. **Validation Methods Table** — Expanded version of the design doc table:

   | Method | Effort | Fidelity | Time | When to Use | AI Agent Example |
   |--------|--------|----------|------|-------------|------------------|
   | Smoke test landing page | Low | Low | 1-3 days | Demand validation | Landing page describing what the agent does, measure signups |
   | Wizard of Oz | Medium | High | 1-2 weeks | Complex agent behavior | Human manually responds as if AI, measure satisfaction |
   | Hardcoded prototype | Medium | Medium | 3-7 days | Core interaction testing | Agent with scripted responses for top 5 scenarios |
   | Concierge (manual agent) | Low | High | 1-2 weeks | Value proposition testing | Manually research and deliver what the agent would |
   | A/B test | High | High | 2-4 weeks | Optimization after launch | Compare two agent behaviors or UI approaches |
   | Prompt-only MVP | Low | Medium | 1-3 days | AI capability testing | Build agent with prompts only (no RAG, no tools) to test if AI adds value |
   | Fake door test | Low | Low | 1-3 days | Feature demand within agent | Add button for unbuilt capability, measure clicks |

2. **Validation Sequence Guide** — How to order experiments:
   - Phase 1: Validate demand (smoke test, fake door)
   - Phase 2: Validate value proposition (concierge, Wizard of Oz)
   - Phase 3: Validate AI capability (prompt-only MVP, hardcoded prototype)
   - Phase 4: Validate full product (A/B test after launch)

3. **AI-Agent-Specific Validation Considerations:**
   - How to test conversational quality without building the full agent
   - How to validate RAG usefulness before building the pipeline
   - How to test tool-use value without full integration
   - Measuring "AI quality" — user satisfaction vs. accuracy vs. speed
   - The "prompt-only MVP" pattern: test with just Claude + system prompt before adding infrastructure

4. **Experiment Design Template:**
   ```
   **Experiment for H[n]:**

   | Attribute | Value |
   |-----------|-------|
   | Experiment type | [method from table] |
   | What we're testing | [specific hypothesis element] |
   | Sample | [who, how many, how recruited] |
   | Duration | [timeframe] |
   | Success metric | [specific threshold] |
   | Failure metric | [when to abandon/pivot] |
   | Minimum viable test | [smallest version that validates] |
   | AI-specific consideration | [what's unique about testing this with AI] |
   ```

**Step 2: Review the file**

Read back. Verify:
- 7 validation methods in the table with AI agent examples
- Validation sequence has 4 phases
- AI-specific considerations cover conversational, RAG, and tool-use
- Experiment template includes AI-specific row
- Formatting matches existing reference files

**Step 3: Commit**

```bash
git add ai-agent-app-prd/references/validation-methods.md
git commit -m "feat: add validation methods reference for AI agent apps"
```

---

### Task 4: Create `assets/capability-menu.md`

**Files:**
- Create: `ai-agent-app-prd/assets/capability-menu.md`

This is the canonical reference for Step 2 (Capability Hypotheses) and Step 4 (Stack Selection). It maps every capability to its plugins, configuration, and simplicity alternatives.

**Step 1: Write the file**

Create `ai-agent-app-prd/assets/capability-menu.md` with these sections:

1. **Capability Menu** — The master table (must match design doc exactly):

   | # | Capability | Description | Plugins | Simpler Alternative |
   |---|-----------|-------------|---------|---------------------|
   | 1 | Conversational AI | Natural language chat interface | Claude API, `agent-sdk-dev:new-sdk-app` | — (core, always included) |
   | 2 | RAG / Knowledge base | Retrieve & reason over documents | `pinecone:assistant-create`, `pinecone:query`, embeddings | Long system prompt with examples |
   | 3 | Web scraping / research | Fetch and process web content | `firecrawl:firecrawl-cli` (scrape, crawl, search) | Manual curation, static data |
   | 4 | Database operations | Store, query, manage structured data | `supabase` MCP (SQL, auth, storage, realtime) | Local storage, JSON files |
   | 5 | Payment processing | Charge users, manage subscriptions | `stripe:stripe-best-practices`, Stripe API | Gumroad link, manual invoicing |
   | 6 | External API calls | Integrate with third-party services | Custom MCP tools, Agent SDK tool definitions | Hardcoded data, manual process |
   | 7 | File processing | Upload, parse, transform files | Agent SDK tools, Supabase storage | Copy-paste text input |
   | 8 | Browser automation | Interact with web pages programmatically | `playwright` MCP (screenshots, forms, clicks) | Manual screenshots, user-provided data |

2. **Plugin Detail Cards** — For each plugin, provide:
   - Plugin name and version
   - What it does (2-3 sentences)
   - Key skills/commands available
   - Configuration required (API keys, setup steps)
   - When to include vs. when to skip

   Cover these plugins:
   - `agent-sdk-dev` — Agent SDK scaffolding
   - `pinecone` — Vector DB + RAG assistants
   - `firecrawl` — Web scraping/crawling
   - `supabase` — Database + auth + storage
   - `stripe` — Payment processing
   - `playwright` — Browser automation
   - `vercel` — Deployment
   - `frontend-design` — UI generation
   - `Notion` — Project tracking
   - `superpowers` — Development workflow (writing-plans, executing-plans, TDD)

3. **Stack Manifest Template** — The fill-in-the-blank format from the design doc:
   ```
   Frontend:     [next.js | react | other] -> deployed to [Vercel]
   AI Backend:   [Claude API | Agent SDK (Python) | Agent SDK (TypeScript)] -> [API routes | standalone]
   Knowledge:    [Pinecone index | none] -> [multilingual-e5-large | llama-text-embed-v2 | none]
   Data:         [Supabase | none] -> [tables: list | none]
   Integrations: [Firecrawl | Stripe | Playwright | custom MCP | none]
   Auth:         [Supabase Auth | none] -> [email/password | OAuth | magic link | none]
   Deploy:       Vercel
   ```

4. **Common Stack Combinations** — Pre-validated combinations for quick mode:
   - **Minimal chat agent:** Conversational AI only → Claude API + Next.js + Vercel
   - **Knowledge agent:** Conversational AI + RAG → + Pinecone
   - **Research agent:** Conversational AI + RAG + Web scraping → + Pinecone + Firecrawl
   - **Full-stack agent:** All capabilities → + Supabase + Stripe + Pinecone + Firecrawl
   - **Internal tool agent:** Conversational AI + Database + Browser automation → + Supabase + Playwright

**Step 2: Review the file**

Read back. Verify:
- 8 capabilities in menu matching design doc
- "Simpler Alternative" column populated for each
- All 10 plugin detail cards present
- Stack manifest template matches design doc format
- 5 common stack combinations listed
- No code, just markdown

**Step 3: Commit**

```bash
git add ai-agent-app-prd/assets/capability-menu.md
git commit -m "feat: add capability menu and plugin mapping reference"
```

---

### Task 5: Create `assets/build-spec-template.md`

**Files:**
- Create: `ai-agent-app-prd/assets/build-spec-template.md`

This is the output template for Step 6 (Generate Build Spec PRD). The skill fills this in during the final step.

**Step 1: Write the file**

Create `ai-agent-app-prd/assets/build-spec-template.md` with the complete PRD structure from the design doc. The template must include:

1. **YAML Metadata Block** — Machine-parseable header:
   ```yaml
   ---
   agent_name: "[Name]"
   version: "v1"
   date: "[YYYY-MM-DD]"
   domain: "[domain]"
   depth_mode: "[quick|thorough]"
   capabilities:
     - [capability_1]
     - [capability_2]
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

2. **Section 1: Agent Hypothesis** — Template for core hypothesis with falsification.

3. **Section 2: Capability Manifest** — Table template:
   | Capability | Hypothesis | Plugin | Config Required |
   |---|---|---|---|
   | [capability] | CH[n]: [statement] | [plugin name] | [key config items] |

4. **Section 3: User Journeys** — JTBD template (3-5 journeys):
   ```
   **J[n]: [Journey Name]**

   > **When** [situation/trigger]
   > **I want the agent to** [action/behavior]
   > **So I can** [outcome/value]

   **Validates:** H[n], CH[n]
   **Edge case:** [what happens when agent fails here]
   ```

5. **Section 4: Architecture**
   - 4a: Tech Stack (stack manifest format)
   - 4b: Component Map (frontend components, API routes, data models)
   - 4c: Data Flow diagram (user → frontend → API → AI → tools → response)

6. **Section 5: Build Sequence** — Phased template:
   ```
   ### Phase 1: Foundation (no dependencies)
   1. [action] → `[plugin/skill]`
   2. [action] → `[plugin/skill]`

   ### Phase 2: Core Agent (depends on Phase 1)
   3. [action] → `[plugin/skill]`

   ### Phase 3: Integration (depends on Phase 2)
   4. [action] → `[plugin/skill]`

   ### Phase 4: Deploy & Validate
   5. [action] → `[plugin/skill]`
   ```

7. **Section 6: Validation Plan** — Experiment table template per hypothesis.

8. **Section 7: Traceability Matrix** — Full-chain template:
   | Hypothesis | Capability | Journey | Component | Build Step | Validation |
   |---|---|---|---|---|---|
   | H[n] | CH[n] | J[n] | [component] | Step [n] | [method] |

9. **Section 8: Open Questions & Non-Goals**

Model the structure after the existing `hypothesis-driven-prd/assets/prd-template.md` — use the same markdown formatting conventions (blockquotes for templates, tables for structured data, clear section headers).

**Step 2: Review the file**

Read back. Verify:
- YAML metadata block has all fields from design doc
- All 8 sections present
- Traceability matrix connects all 6 layers (hypothesis → capability → journey → component → build step → validation)
- Build sequence references plugin names with backtick formatting
- Template uses placeholder syntax `[bracketed]` consistently
- No actual content — pure template

**Step 3: Commit**

```bash
git add ai-agent-app-prd/assets/build-spec-template.md
git commit -m "feat: add build spec PRD output template"
```

---

### Task 6: Create `SKILL.md` — The Main Workflow (Part 1: Frontmatter + Steps 1-3)

**Files:**
- Create: `ai-agent-app-prd/SKILL.md`

This is the main skill file — the conversational workflow that Claude follows. Split across two tasks for manageability.

**Step 1: Write SKILL.md with frontmatter and Steps 1-3**

The file must start with YAML frontmatter:
```yaml
---
name: ai-agent-app-prd
description: Create build-spec PRDs for lightweight AI Agent web apps using hypothesis-driven design. Use when users want to spec AI agent apps, plan AI-powered web applications, design conversational AI tools, or create PRDs for apps that use Claude, RAG, tool-use, or other AI capabilities. Walks through Agent Hypothesis → Capability Hypotheses → User Journeys → Stack Selection → Validation Plan → Build Spec PRD. Outputs a machine-readable PRD structured for direct code generation by Claude Code plugins.
---
```

Then the main content:

**Header section:**
- Title: "AI Agent App PRD Generator"
- One-line description: "Generate build-spec PRDs for lightweight AI agent web apps — from hypothesis to executable spec."
- Workflow overview diagram (text-based, like the existing skill)

**Step 0: Initialization**
- Ask depth mode: Quick (~10 min) or Thorough (~30 min)
- Ask if they have an existing concept or starting from scratch
- Set expectations: "This skill will walk you through 6 steps. At the end, you'll have a build-spec PRD that Claude Code can use to scaffold your app."

**Step 1: Agent Hypothesis**
- Present the hypothesis format template
- Ask questions in batches of 2-3 (matching design doc):
  - What problem does this agent solve?
  - Who has this problem most acutely?
  - What's the current alternative?
- Then:
  - What outcome would prove this is worth building?
  - What would prove you wrong?
- Challenge protocol: push back on vague/unfalsifiable hypotheses
- Summarize: reflect back the formatted hypothesis for confirmation
- Reference: `See references/hypothesis-patterns.md for patterns`

**Step 2: Capability Hypotheses**
- Present the capability menu table (reference `assets/capability-menu.md`)
- Ask user to select capabilities (multiple choice, allow "other")
- For EACH selected capability:
  - Ask: "Why do users need [capability]?"
  - Ask: "What evidence would show this is needed?"
  - Challenge: "Could you achieve this with [simpler alternative from menu]?"
  - Capture as: `CH[n]: Users need [capability] because [rationale]. Falsification: [condition].`
- Summarize all capability hypotheses before moving on
- For Quick mode: present common stack combinations and ask "which is closest?"

**Step 3: User Journey Mapping**
- Explain JTBD format
- Ask for 3-5 journeys, guided:
  1. "What's the first thing a user does?" (onboarding)
  2. "What's the core 'aha' moment?" (value delivery)
  3. "What brings them back?" (retention)
  4. "What happens when the agent doesn't know the answer?" (edge case)
  5. "What happens when the agent is wrong?" (edge case)
- For each journey, capture in format and tag with hypothesis
- Challenge: flag any journey that doesn't trace to a hypothesis
- Summarize all journeys

Write the content following these conventions from the existing `hypothesis-driven-prd/SKILL.md`:
- Use code blocks for example question formats
- Use blockquotes for hypothesis/journey templates
- Include "Example Question Format" blocks showing lettered options
- Include skip instructions: `(Type 'skip' to move to the next section)`

**Step 2: Review Part 1 of the file**

Read back `SKILL.md`. Verify:
- YAML frontmatter has `name` and `description` fields
- Description mentions key trigger phrases (AI agent, PRD, build spec, conversational AI, RAG, tool-use)
- Workflow overview shows all 6 steps
- Steps 1-3 match the design doc
- Question batches are 2-3 per turn
- Challenge moments are present in each step
- Skip instructions included
- References to supporting files use relative paths

Do NOT commit yet — Task 7 completes the file.

---

### Task 7: Complete `SKILL.md` — Steps 4-6 + Conversation Guidelines

**Files:**
- Modify: `ai-agent-app-prd/SKILL.md` (append Steps 4-6 and guidelines)

**Step 1: Append Steps 4-6 and conversation guidelines**

**Step 4: Stack Selection & Architecture**
- Present the stack manifest template (reference `assets/capability-menu.md`)
- For each validated capability from Step 2, map to specific plugin:
  - Show plugin name, what it does, config needed
  - Ask: "Does this look right for [capability]?"
- Challenge: "You selected [N] integrations for v1. Can we cut to [fewer]?"
- For Quick mode: present closest common stack combination, ask for confirmation
- Output: filled-in stack manifest
- Define component map: frontend components needed, API routes needed, data models needed
- Define data flow: user → frontend → API → AI model → tools → response → frontend

**Step 5: Validation Plan**
- Reference `references/validation-methods.md`
- For each hypothesis (agent + capability), ask:
  - "How would you test this before building the full thing?"
  - Present validation method options with effort/fidelity ratings
- Define validation sequence: what to test first
- For Quick mode: auto-suggest "prompt-only MVP" as default validation for most hypotheses
- Challenge: "Can you validate [hypothesis] with just a [simpler method]?"
- Summarize the validation plan

**Step 6: Generate Build Spec PRD**
- Announce: "Generating your build spec PRD..."
- Use template from `assets/build-spec-template.md`
- Fill in all sections from Steps 1-5:
  - YAML metadata from selections
  - Agent hypothesis from Step 1
  - Capability manifest from Step 2
  - User journeys from Step 3
  - Architecture from Step 4
  - Build sequence (auto-generated from stack, ordered by dependency phases)
  - Validation plan from Step 5
  - Traceability matrix (auto-generated connecting all layers)
  - Open questions (collected throughout)
  - Non-goals (collected throughout)
- Output location: user-specified or default `prd-[agent-name].md`
- After output, state: "This PRD is structured as a build spec. You can use it with Claude Code to scaffold and build the app."

**Conversation Guidelines section** (at end of file):
1. Ask questions in batches of 2-3 with lettered options
2. Allow skipping with "skip" or "s"
3. Summarize after each step before moving to next
4. Adapt depth based on mode selection (Quick vs. Thorough)
5. Don't write code — PRD generation is the deliverable
6. Challenge weak hypotheses and unnecessary complexity
7. Prioritize ruthlessly — push for 1-2 capabilities max in v1
8. Challenge "I need AI for this" — sometimes a form is the answer
9. Never block — if user insists after challenge, record choice and move on
10. Reference supporting files for patterns and templates

**Output section:**
- Format: Markdown
- Location: user-specified or default
- Filename: `prd-[agent-name].md`

**Step 2: Review the complete file**

Read back full `SKILL.md`. Verify:
- All 6 steps present and complete
- Frontmatter description covers all trigger phrases
- Each step has: goal, questions, challenge moment, summary instruction
- Quick vs. Thorough mode differences noted in each step
- Conversation guidelines match design doc (10 guidelines)
- Output section specifies format, location, filename
- References to `assets/` and `references/` files are correct
- No code generation — the skill only produces PRD markdown
- Total file is comprehensive but not bloated (target: similar length to existing SKILL.md, ~280 lines)

**Step 3: Commit**

```bash
git add ai-agent-app-prd/SKILL.md
git commit -m "feat: add main SKILL.md workflow for AI Agent App PRD skill"
```

---

### Task 8: Update repository README

**Files:**
- Modify: `README.md` (root level)

**Step 1: Read the current README**

```bash
# Read README.md to understand current content
```

**Step 2: Update README to mention both skills**

The README currently describes only the Hypothesis-Driven PRD skill. Update it to:
- Describe the repository as housing multiple PRD skills
- Add a section for the AI Agent App PRD skill with:
  - Brief description (2-3 sentences)
  - Link to the skill directory
  - Key differences from the HDD PRD skill
- Keep existing HDD PRD skill description intact
- Update any "this skill" language to "these skills" where appropriate

**Step 3: Review the update**

Read back. Verify:
- Both skills are mentioned
- No content was removed from the existing description
- New section accurately describes the AI Agent App PRD skill

**Step 4: Commit**

```bash
git add README.md
git commit -m "docs: update README to cover both PRD skills"
```

---

### Task 9: Final review and validation

**Files:**
- Review: all files in `ai-agent-app-prd/`

**Step 1: Verify file structure matches design doc**

```bash
ls -R ai-agent-app-prd/
```

Expected:
```
ai-agent-app-prd/:
SKILL.md
assets/
references/

ai-agent-app-prd/assets:
build-spec-template.md
capability-menu.md

ai-agent-app-prd/references:
hypothesis-patterns.md
validation-methods.md
```

**Step 2: Cross-reference against design doc**

Read the design doc at `docs/plans/2026-03-01-ai-agent-app-prd-skill-design.md` and verify:

- [ ] SKILL.md has all 6 steps from design doc
- [ ] SKILL.md frontmatter triggers match design doc scope
- [ ] Capability menu has all 8 capabilities from design doc
- [ ] Build spec template has all 8 sections from design doc
- [ ] Hypothesis patterns cover all 5 agent types and 8 capabilities
- [ ] Validation methods include all 7 methods from design doc (including prompt-only MVP)
- [ ] Stack manifest template matches design doc format
- [ ] Plugin integration mapping covers all 10 plugins from design doc
- [ ] Traceability matrix connects all 6 layers
- [ ] Quick and Thorough modes are documented in SKILL.md
- [ ] Challenge protocol is present in each step
- [ ] Conversation guidelines match design doc (10 items)

**Step 3: Verify no cross-file inconsistencies**

Check that:
- Capability names in `capability-menu.md` exactly match names used in `SKILL.md`
- Plugin names in `capability-menu.md` exactly match names in `build-spec-template.md`
- Hypothesis format in `hypothesis-patterns.md` matches format used in `SKILL.md` Step 1
- Validation methods in `validation-methods.md` match methods referenced in `SKILL.md` Step 5
- JTBD journey format in `build-spec-template.md` matches format in `SKILL.md` Step 3

**Step 4: Fix any issues found**

If any inconsistencies or missing items are found, fix them and commit:

```bash
git add -A ai-agent-app-prd/
git commit -m "fix: resolve cross-file inconsistencies in AI Agent App PRD skill"
```

If no issues found, skip this step.

---

## Summary

| Task | File | Description |
|------|------|-------------|
| 1 | directories | Scaffold `ai-agent-app-prd/` structure |
| 2 | `references/hypothesis-patterns.md` | Agent + capability hypothesis patterns |
| 3 | `references/validation-methods.md` | AI-agent-specific validation experiments |
| 4 | `assets/capability-menu.md` | Capability → plugin mapping reference |
| 5 | `assets/build-spec-template.md` | PRD output template |
| 6 | `SKILL.md` (part 1) | Frontmatter + Steps 1-3 |
| 7 | `SKILL.md` (part 2) | Steps 4-6 + guidelines |
| 8 | `README.md` | Update repo README for both skills |
| 9 | all files | Final cross-reference validation |

**Total:** 9 tasks, ~8 commits, all markdown (no code).
