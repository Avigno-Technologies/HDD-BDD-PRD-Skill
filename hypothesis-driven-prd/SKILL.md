---
name: hypothesis-driven-prd
description: Create Product Requirements Documents using Hypothesis-Driven Development (HDD) and Jobs-to-be-Done (JTBD) frameworks. Use when users want to write PRDs, define product requirements, spec features, plan MVPs, or translate business assumptions into testable features. Guides users through Business Model Canvas assumptions → Customer/LTV hypotheses → Behavioral hypotheses → Features → Requirements with full traceability. Supports single features, feature clusters, or MVP scope with critical path prioritization.
---

# Hypothesis-Driven PRD Generator

Generate PRDs that trace from business assumptions to testable requirements.

## Workflow Overview

```
1. Scope Discovery → Determine: single feature | feature cluster | MVP
2. Business Model Canvas → Surface assumptions (skippable per block)
3. Acquisition & LTV Hypotheses → Testable predictions with metrics
4. Behavioral Hypotheses → JTBD format behaviors
5. Feature Derivation → Features enabling target behaviors
6. Requirements + Validation → Experiments + acceptance criteria
7. Output → Single PRD with traceability matrix
```

## Step 1: Scope Discovery

Ask the user:

```
Before we begin, I need to understand the scope:

1. What are we speccing?
   A. Single feature (one focused capability)
   B. Feature cluster (related features validating one hypothesis)
   C. MVP (minimum features to validate a business assumption)
   D. Not sure yet—help me figure it out

2. How much discovery do you need?
   A. Thorough—walk me through everything, I'm still forming ideas
   B. Quick—I have clarity, just need structure
   C. Somewhere in between

3. Do you have an existing Business Model Canvas or assumptions doc?
   A. Yes, I'll share it
   B. No, let's build from scratch
   C. Partial—I have some thinking but it's incomplete
```

Adapt depth based on responses. For "Quick" mode, present likely assumptions for confirmation rather than open-ended discovery.

## Step 2: Business Model Canvas Iteration

Iterate through relevant BMC blocks to surface assumptions. For each block:
- Present 2-3 probing questions
- Allow user to skip with "skip" or "s"
- Capture assumptions as explicit statements

**Block sequence** (adapt based on scope):

### Value Proposition
- What problem does this solve? What pain does it eliminate?
- What gain does it create? Why would someone pay/switch for this?
- How is this different from alternatives?

### Customer Segments  
- Who specifically has this problem most acutely?
- What characterizes your ideal early adopter?
- Who is explicitly NOT the target (for now)?

### Channels
- How will users discover this?
- What's the primary acquisition path?
- Where do target users already spend attention?

### Customer Relationships
- What relationship does the user expect (self-service, assisted, automated)?
- What drives retention vs. churn for this segment?
- How do users currently solve this problem?

### Revenue Streams
- What would users pay for? How much?
- What's the pricing model assumption (subscription, transaction, freemium)?
- What's the LTV assumption?

### Key Activities & Resources (if relevant)
- What must you do exceptionally well?
- What's the critical capability or asset?

After each block, summarize: "So your assumption is: [A1: statement]"

**Format captured assumptions as:**
```
A1: [Value Prop assumption]
A2: [Customer Segment assumption]
A3: [Channel assumption]
...
```

## Step 3: Customer Acquisition & LTV Hypotheses

Transform assumptions into testable hypotheses using this format:

```
**H[n]: [Hypothesis Name]**

If [condition/we build X], 
then [prediction/users will do Y], 
because [rationale/underlying belief].

We'll know we're right when: [specific evidence/metric threshold]
We'll know we're wrong when: [falsification criteria]

Validates assumptions: A1, A3
```

**Guide the user:**
- Which assumptions are highest risk (most uncertain, most critical)?
- What would change your strategy if proven false?
- Prioritize 2-4 hypotheses for the PRD scope

**For MVP scope:** Identify the single most critical hypothesis to validate first.

## Step 4: Behavioral Hypotheses (JTBD Format)

For each hypothesis, define target behaviors using Jobs-to-be-Done format:

```
**B[n]: [Behavior Name]**

When [situation/trigger context],
I want to [motivation/action],
so I can [outcome/benefit].

Frequency: [how often this job arises]
Current solution: [how users solve this today]
Validates hypothesis: H[n]
```

**Probing questions:**
- What triggers the user to seek a solution?
- What does "done" look like for the user?
- What's the emotional state before/after?
- How do they measure success?

## Step 5: Feature Derivation

Map behaviors to features:

```
**F[n]: [Feature Name]**

Enables behavior: B[n]
Core capability: [what the feature does]
User value: [why it matters to user]
```

**For MVP/cluster scope, include:**

### Prioritization Matrix
| Feature | Impact (1-5) | Confidence (1-5) | Effort (1-5) | Priority |
|---------|--------------|------------------|--------------|----------|
| F1      |              |                  |              |          |

### Critical Path
Identify dependencies:
```
F1 (foundational) → F2 (depends on F1) → F3 (depends on F2)
                  → F4 (parallel track)
```

Ask: "Which features are prerequisites for others? What's the minimum path to validate H1?"

## Step 6: Requirements + Validation

### 6a. Hypothesis-Level Experiments

For each hypothesis:

```
**Experiment for H[n]:**

Experiment type: [A/B test | Usability test | Smoke test | Wizard of Oz | etc.]
Sample: [who, how many]
Duration: [timeframe]
Success metric: [specific threshold]
Failure metric: [when to abandon/pivot]
Minimum viable test: [smallest version that could validate]
```

### 6b. Feature-Level Requirements & Acceptance Criteria

For each feature, write numbered requirements with JTBD-based acceptance criteria:

```
**R[n].1: [Requirement statement]**

Acceptance criteria:
- When [situation], user can [action], resulting in [outcome]
- When [edge case], system [handles gracefully]

Validates: F[n] → B[n] → H[n] → A[n]
```

## Step 7: Generate PRD Document

Compile into a single document with this structure:

```markdown
# PRD: [Feature/MVP Name]

## Overview
[2-3 sentence summary: what this is, what problem it solves, primary hypothesis]

## Business Context

### Assumptions
| ID | Assumption | Confidence | Risk if Wrong |
|----|------------|------------|---------------|
| A1 | [statement] | High/Med/Low | [impact] |

### Hypotheses
[Full hypothesis statements from Step 3]

## Target Behaviors
[JTBD statements from Step 4]

## Features

### Feature Overview
| ID | Feature | Enables Behavior | Priority | Dependencies |
|----|---------|------------------|----------|--------------|
| F1 | [name]  | B1               | P0       | None         |

### Critical Path
[Dependency diagram if MVP/cluster scope]

### Feature Details
[Detailed feature descriptions]

## Requirements

### [F1: Feature Name]
[Numbered requirements with acceptance criteria]

## Validation Strategy

### Hypothesis Experiments
[Experiment designs from Step 6a]

### Success Metrics
| Metric | Target | Measurement Method | Timeline |
|--------|--------|-------------------|----------|
| [metric] | [threshold] | [how measured] | [when] |

## Traceability Matrix

| Requirement | Feature | Behavior | Hypothesis | Assumption |
|-------------|---------|----------|------------|------------|
| R1.1        | F1      | B1       | H1         | A1, A2     |
| R1.2        | F1      | B1       | H1         | A1         |

## Open Questions
[Unresolved items requiring further discovery]

## Non-Goals
[Explicit scope boundaries]
```

## Output

- **Format:** Markdown
- **Location:** `/tasks/` (or user-specified)
- **Filename:** `prd-[feature-name].md`

## Conversation Guidelines

1. **Ask questions in batches of 3-5 max** with lettered options for easy response
2. **Allow skipping** any discovery section with "skip" or "s"
3. **Summarize frequently** — reflect back assumptions/hypotheses for confirmation
4. **Adapt depth** based on user's stated preference (thorough vs. quick)
5. **Don't implement** — PRD generation is the deliverable, not code
6. **Challenge weak hypotheses** — if a hypothesis isn't falsifiable, push back
7. **Prioritize ruthlessly** — for MVP scope, constantly ask "what's the minimum?"

## Example Question Format

```
Let's explore your Value Proposition assumptions:

1. What core problem does this solve for your target user?
   A. [Inferred option based on context]
   B. [Alternative framing]
   C. [Different problem angle]
   D. Something else—I'll describe

2. Why would someone switch from their current solution?
   A. It's significantly faster
   B. It's significantly cheaper  
   C. It does something impossible before
   D. The current solution is painful enough to change
   E. Multiple/other—I'll explain

(Type 'skip' to move to the next section if you have clarity here)
```
