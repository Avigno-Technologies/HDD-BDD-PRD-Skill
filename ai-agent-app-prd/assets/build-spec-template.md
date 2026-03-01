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

# Build Spec PRD: [Agent Name]

**Domain:** [domain]
**Depth Mode:** [quick|thorough]
**Date:** [YYYY-MM-DD]
**Status:** Draft

---

## 1. Agent Hypothesis

> If I build an agent that **[does X]** for **[audience]**, then **[measurable outcome]** because **[rationale]**.
>
> **Falsification:** This hypothesis is wrong if **[failure condition]** within **[timeframe]**.

### Capability Hypotheses

**CH1: [Capability Hypothesis Name]**

> If the agent can **[capability]**, then **[expected user behavior/outcome]** because **[rationale]**.
>
> - **We'll know we're right when:** [specific evidence/metric threshold]
> - **We'll know we're wrong when:** [falsification criteria]
> - **Supports core hypothesis by:** [connection to H1]

**CH2: [Capability Hypothesis Name]**

> If the agent can **[capability]**, then **[expected user behavior/outcome]** because **[rationale]**.
>
> - **We'll know we're right when:** [specific evidence/metric threshold]
> - **We'll know we're wrong when:** [falsification criteria]
> - **Supports core hypothesis by:** [connection to H1]

---

## 2. Capability Manifest

| Capability | Hypothesis | Plugin | Config Required |
|---|---|---|---|
| [capability] | CH1: [statement] | [plugin name] | [key config items] |
| [capability] | CH2: [statement] | [plugin name] | [key config items] |
| [capability] | CH[n]: [statement] | [plugin name] | [key config items] |

---

## 3. User Journeys

**J1: [Journey Name]**

> **When** [situation/trigger]
> **I want the agent to** [action/behavior]
> **So I can** [outcome/value]

- **Validates:** H1, CH[n]
- **Edge case:** [what happens when agent fails here]

**J2: [Journey Name]**

> **When** [situation/trigger]
> **I want the agent to** [action/behavior]
> **So I can** [outcome/value]

- **Validates:** H1, CH[n]
- **Edge case:** [what happens when agent fails here]

**J3: [Journey Name]**

> **When** [situation/trigger]
> **I want the agent to** [action/behavior]
> **So I can** [outcome/value]

- **Validates:** H1, CH[n]
- **Edge case:** [what happens when agent fails here]

**J4: [Journey Name]**

> **When** [situation/trigger]
> **I want the agent to** [action/behavior]
> **So I can** [outcome/value]

- **Validates:** H1, CH[n]
- **Edge case:** [what happens when agent fails here]

**J5: [Journey Name]**

> **When** [situation/trigger]
> **I want the agent to** [action/behavior]
> **So I can** [outcome/value]

- **Validates:** H1, CH[n]
- **Edge case:** [what happens when agent fails here]

---

## 4. Architecture

### 4a. Tech Stack

| Layer | Technology | Purpose |
|-------|-----------|---------|
| Frontend | [framework] | [role in agent UX] |
| AI Backend | [model/sdk] | [role in agent logic] |
| Knowledge | [pinecone/none] | [role in retrieval/context] |
| Data | [supabase/none] | [role in persistence] |
| Integrations | [service list] | [role in external connectivity] |
| Auth | [auth provider] | [role in user management] |
| Deploy | vercel | [role in hosting/serving] |

### 4b. Component Map

**Frontend Components**

| Component | Description | Depends On |
|-----------|-------------|------------|
| [component name] | [what it renders/does] | [dependencies] |
| [component name] | [what it renders/does] | [dependencies] |
| [component name] | [what it renders/does] | [dependencies] |

**API Routes**

| Route | Method | Description | Auth Required |
|-------|--------|-------------|---------------|
| [/api/endpoint] | [GET/POST] | [what it does] | [yes/no] |
| [/api/endpoint] | [GET/POST] | [what it does] | [yes/no] |
| [/api/endpoint] | [GET/POST] | [what it does] | [yes/no] |

**Data Models**

| Model | Fields | Purpose |
|-------|--------|---------|
| [model name] | [key fields] | [what it stores] |
| [model name] | [key fields] | [what it stores] |

### 4c. Data Flow

```
[User input]
  → [Frontend component]
    → [API route]
      → [AI model/SDK]
        → [Tools/integrations]
      ← [AI response]
    ← [Processed result]
  ← [Rendered output]
```

---

## 5. Build Sequence

### Phase 1: Foundation (no dependencies)
1. [action] → `[plugin/skill]`
2. [action] → `[plugin/skill]`

### Phase 2: Core Agent (depends on Phase 1)
3. [action] → `[plugin/skill]`
4. [action] → `[plugin/skill]`

### Phase 3: Integration (depends on Phase 2)
5. [action] → `[plugin/skill]`
6. [action] → `[plugin/skill]`

### Phase 4: Deploy & Validate
7. [action] → `[plugin/skill]`
8. [action] → `[plugin/skill]`

---

## 6. Validation Plan

**Experiment for H1 (Core Hypothesis):**

| Attribute | Value |
|-----------|-------|
| Experiment type | [method] |
| What we're testing | [hypothesis element] |
| Sample | [who, how many] |
| Duration | [timeframe] |
| Success metric | [threshold] |
| Failure metric | [abandon/pivot criteria] |
| Minimum viable test | [smallest validation] |

**Experiment for CH1:**

| Attribute | Value |
|-----------|-------|
| Experiment type | [method] |
| What we're testing | [hypothesis element] |
| Sample | [who, how many] |
| Duration | [timeframe] |
| Success metric | [threshold] |
| Failure metric | [abandon/pivot criteria] |
| Minimum viable test | [smallest validation] |

**Experiment for CH2:**

| Attribute | Value |
|-----------|-------|
| Experiment type | [method] |
| What we're testing | [hypothesis element] |
| Sample | [who, how many] |
| Duration | [timeframe] |
| Success metric | [threshold] |
| Failure metric | [abandon/pivot criteria] |
| Minimum viable test | [smallest validation] |

---

## 7. Traceability Matrix

| Hypothesis | Capability | Journey | Component | Build Step | Validation |
|---|---|---|---|---|---|
| H1 | CH[n] | J[n] | [component] | Phase [n], Step [n] | [method] |
| H1 | CH[n] | J[n] | [component] | Phase [n], Step [n] | [method] |
| H1 | CH[n] | J[n] | [component] | Phase [n], Step [n] | [method] |

---

## 8. Open Questions & Non-Goals

### Open Questions

- [ ] [Question requiring further discovery]
- [ ] [Unresolved technical consideration]
- [ ] [Stakeholder input needed]
- [ ] [Dependency to confirm]

### Non-Goals

Explicitly out of scope for this build:

- [Thing we're NOT building]
- [User segment we're NOT targeting]
- [Use case we're NOT supporting]
- [Integration we're NOT doing yet]
- [Scale we're NOT optimizing for yet]
