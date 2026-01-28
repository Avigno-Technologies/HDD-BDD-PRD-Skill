# Hypothesis-Driven PRD Skill: STAR Overview

## Situation

### The Problem We Observed

Product development suffers from a persistent translation gap. Different stakeholders speak different languages:

| Stakeholder | Speaks In | Cares About |
|-------------|-----------|-------------|
| **Business/Founders** | Revenue, market share, competitive position | Will this make money? Will customers pay? |
| **Product Owners** | User stories, backlogs, roadmaps | What should we build? In what order? |
| **Engineers** | Requirements, acceptance criteria, technical specs | How do I know when I'm done? What exactly should this do? |
| **Growth/Acquisition** | CAC, conversion funnels, channel effectiveness | How do we get users? What message resonates? |

Traditional PRDs attempt to bridge these gaps but typically fail in predictable ways:

1. **Features without rationale** — Requirements exist, but no one remembers *why* they matter
2. **Untested assumptions** — Business model beliefs are treated as facts rather than hypotheses
3. **No falsification criteria** — Teams build features without defining what "failure" looks like
4. **Disconnected metrics** — Success metrics aren't traced to the business assumptions they're supposed to validate
5. **Scope creep without traceability** — New requirements appear without connection to validated needs

### The Organizational Cost

When these gaps persist:
- Engineers build features that don't move business metrics
- Product teams can't explain *why* they prioritized one feature over another
- Growth teams optimize for acquisition without understanding what makes users stick
- Leadership makes resource decisions based on opinions rather than evidence
- Failed products consume runway without generating learning

---

## Task

### What We Set Out to Build

A systematic framework that:

1. **Traces requirements backward** to the business assumptions they validate
2. **Makes assumptions explicit** and testable rather than implicit and assumed
3. **Creates a common language** across business, product, engineering, and growth
4. **Embeds validation design** into the PRD itself—not as an afterthought
5. **Supports multiple scopes** — from single features to full MVPs
6. **Balances rigor with speed** — thorough discovery when needed, quick structure when clear

### The Core Insight

We recognized that **two complementary frameworks** needed to be married:

**Hypothesis-Driven Development (HDD)** addresses the *whether* question:
- Should we build this at all?
- What assumption are we testing?
- How will we know if we're wrong?

**Jobs-to-Be-Done (JTBD)** addresses the *what* and *why* questions:
- What is the user trying to accomplish?
- In what context does this need arise?
- What does success look like from the user's perspective?

Neither framework alone is sufficient. HDD without JTBD produces hypotheses disconnected from user reality. JTBD without HDD produces user stories without business validation.

---

## Action

### The Framework We Designed

We created a **seven-step cascade** that flows from business model to testable requirements:

```
Business Model Canvas Assumptions
        ↓
Customer Acquisition & LTV Hypotheses  
        ↓
Behavioral Hypotheses (JTBD format)
        ↓
Features that enable target behaviors
        ↓
Requirements with acceptance criteria
        ↓
Validation experiments + success metrics
        ↓
Traceability matrix connecting all layers
```

### Key Design Decisions

#### Decision 1: Start with Business Model Canvas

**Why:** The BMC forces explicit articulation of *assumptions* about value proposition, customer segments, channels, relationships, and revenue. Most product failures stem from untested assumptions in these areas—not from poor engineering.

**How:** We iterate through relevant BMC blocks with probing questions, allowing users to skip sections where they have clarity. Each block produces numbered assumptions (A1, A2, A3...) that become the foundation for everything downstream.

**Concern addressed:** "We're building features but don't know if the underlying business model is sound."

#### Decision 2: Hypotheses with Falsification Criteria

**Why:** A hypothesis without falsification criteria is just an opinion. Teams need to define—*in advance*—what evidence would cause them to abandon or pivot.

**Format chosen:**
```
If [condition/we build X], 
then [prediction/users will do Y], 
because [rationale/underlying belief].

We'll know we're right when: [specific evidence/metric threshold]
We'll know we're wrong when: [falsification criteria]
```

**Concern addressed:** "We shipped the feature, metrics are ambiguous, and now we're arguing about whether it 'worked.'"

#### Decision 3: JTBD for Behavioral Hypotheses

**Why:** User stories ("As a user, I want X so that Y") often become feature descriptions in disguise. JTBD forces attention to the *situation* that triggers the need—the context that makes the job arise.

**Format chosen:**
```
When [situation/trigger context],
I want to [motivation/action],
so I can [outcome/benefit].
```

**Concern addressed:** "We built what users said they wanted, but they don't use it because we misunderstood the context."

#### Decision 4: Explicit Traceability Matrix

**Why:** As products evolve, the connection between requirements and business assumptions gets lost. A traceability matrix makes these connections explicit and queryable.

**Format:**
| Requirement | Feature | Behavior | Hypothesis | Assumption |
|-------------|---------|----------|------------|------------|
| R1.1        | F1      | B1       | H1         | A1, A2     |

**Concern addressed:** "We have 47 requirements and no one can explain which business outcomes they serve."

#### Decision 5: Validation Strategy as First-Class Citizen

**Why:** Most PRDs treat validation as an afterthought ("we'll figure out how to measure it later"). By requiring experiment design *within* the PRD, we force teams to think about testability before committing resources.

**Included elements:**
- Hypothesis-level experiments (A/B tests, smoke tests, Wizard of Oz, etc.)
- Feature-level acceptance criteria
- Success *and* failure metrics with thresholds
- Minimum viable test definition

**Concern addressed:** "We spent six months building, and now we realize we can't actually measure whether it worked."

#### Decision 6: Flexible Scope (Feature → Cluster → MVP)

**Why:** Not every PRD is an MVP. Sometimes you're speccing a single feature within an existing product. The framework needed to scale up and down.

**How:** The skill asks about scope upfront and adapts:
- **Single feature:** Focused hypothesis, minimal BMC exploration
- **Feature cluster:** Multiple features validating one hypothesis, includes prioritization
- **MVP:** Full BMC exploration, critical path mapping, ruthless scoping to minimum

**Concern addressed:** "This framework is too heavy for a simple feature add" or "This framework doesn't help us think about MVP scope."

#### Decision 7: Skippable Discovery with Depth Options

**Why:** Product people have varying levels of clarity. Some need guided discovery; others have done the thinking and just need structure.

**How:** 
- User declares "thorough" vs. "quick" mode upfront
- Every BMC block can be skipped with "skip" or "s"
- In quick mode, present likely assumptions for confirmation rather than open-ended questions

**Concern addressed:** "I already know what I want to build; don't make me answer 50 questions."

---

## Result

### How This Framework Creates Shared Language

#### For Business/Founders

The framework surfaces and tests their implicit beliefs:

- **Before:** "We should build X because I think customers want it"
- **After:** "We're testing assumption A3 (customers will pay $Y for pain relief Z) via hypothesis H2, which we'll validate by measuring [specific metric]"

**Value:** Business intuition becomes testable. Failures generate learning, not blame.

#### For Product Owners

The framework provides defensible prioritization:

- **Before:** "We prioritized this feature because stakeholders asked for it"
- **After:** "F2 is P0 because it's on the critical path to validate H1, which tests our highest-risk assumption A1"

**Value:** Prioritization decisions are traceable to business risk, not opinion.

#### For Engineers

The framework provides clear acceptance criteria and scope boundaries:

- **Before:** "Build a login feature" (followed by 47 clarifying questions)
- **After:** "R1.1: When [situation], user can [action], resulting in [outcome]. This validates F1 → B1 → H1 → A1."

**Value:** Engineers know *exactly* what "done" means and *why* the feature matters.

#### For Growth/Acquisition

The framework connects acquisition strategy to product capability:

- **Before:** "Run ads and see what converts"
- **After:** "H1 predicts that [channel X] will reach [segment Y] with CAC < $Z. F3 is the activation feature that converts acquired users. We'll measure [specific conversion metric]."

**Value:** Growth experiments are connected to product hypotheses, not run in isolation.

### The Meta-Benefit: Structured Disagreement

When stakeholders disagree, the framework provides a resolution mechanism:

1. **Identify which assumption is contested** — "You're questioning A2"
2. **Examine the hypothesis that tests it** — "H1 is designed to validate A2"
3. **Review the evidence threshold** — "We agreed we'd know we're wrong when [metric] < [threshold]"
4. **Make a decision** — Either gather more evidence or accept the uncertainty and move forward

This converts "I think/you think" arguments into "what does the evidence say?" discussions.

---

## Assumptions Underlying This Framework

We should be explicit about our own assumptions:

| Assumption | Risk if Wrong |
|------------|---------------|
| Teams *want* rigor in product planning | Framework feels bureaucratic, gets abandoned |
| Business model assumptions are identifiable | Teams can't articulate what they're really betting on |
| Hypotheses can be meaningfully falsified | Metrics are too noisy or long-term to provide clear signal |
| JTBD format captures user motivation better than alternatives | Format feels awkward, teams revert to feature-based stories |
| Traceability adds value proportional to maintenance cost | Matrix becomes stale, no one updates it |
| Upfront validation design improves outcomes | Teams design experiments they never run |

### Mitigation Built Into the Skill

- **Bureaucracy concern:** Skippable sections, quick mode, scope-appropriate depth
- **Articulation concern:** Probing questions with multiple-choice options to scaffold thinking
- **Falsification concern:** Require both success AND failure criteria; include "minimum viable test"
- **Format awkwardness:** Examples embedded throughout; template provided
- **Maintenance concern:** Single document (not multiple), clear structure, generated at decision point
- **Unused experiments:** Experiments designed before build commitment, reviewed as part of PRD approval

---

## Open Questions for Future Iteration

1. **Integration with task generation:** How should this PRD format feed into ai-dev-tasks style implementation planning?

2. **Living document vs. snapshot:** Should the PRD update as hypotheses are validated/invalidated, or remain a historical artifact?

3. **Team size calibration:** Is this framework too heavy for solo founders? Too light for enterprise product teams?

4. **Validation cadence:** How do we prompt teams to actually *run* the experiments they designed?

5. **Multi-hypothesis products:** How do we handle products testing multiple independent hypotheses simultaneously?

---

## Summary

The Hypothesis-Driven PRD Skill exists because **building the wrong thing faster is not progress**. 

By forcing explicit articulation of assumptions, testable hypotheses, user behaviors, and validation criteria—all in a single traceable document—we give cross-functional teams a shared language for making product decisions.

The framework doesn't guarantee success. It guarantees that failure generates learning, and that success can be attributed to specific validated assumptions rather than luck.

**The core promise:** Every requirement in your PRD can answer the question "What business assumption does this test, and how will we know if we're wrong?"

---

*Framework Version: 1.0*  
*Created: January 2026*
