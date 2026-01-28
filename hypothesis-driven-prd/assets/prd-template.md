# PRD: [Feature/MVP Name]

**Version:** 1.0  
**Date:** [Date]  
**Author:** [Author]  
**Status:** Draft | Review | Approved

---

## Overview

[2-3 sentence summary: what this is, what problem it solves, primary hypothesis being tested]

---

## Business Context

### Assumptions

| ID | Assumption | Confidence | Risk if Wrong |
|----|------------|------------|---------------|
| A1 | [statement] | High/Med/Low | [impact description] |
| A2 | [statement] | High/Med/Low | [impact description] |
| A3 | [statement] | High/Med/Low | [impact description] |

### Hypotheses

**H1: [Hypothesis Name]**

If [condition/we build X],  
then [prediction/users will do Y],  
because [rationale/underlying belief].

- **We'll know we're right when:** [specific evidence/metric threshold]
- **We'll know we're wrong when:** [falsification criteria]
- **Validates assumptions:** A1, A2

**H2: [Hypothesis Name]**

If [condition/we build X],  
then [prediction/users will do Y],  
because [rationale/underlying belief].

- **We'll know we're right when:** [specific evidence/metric threshold]
- **We'll know we're wrong when:** [falsification criteria]
- **Validates assumptions:** A3

---

## Target Behaviors

**B1: [Behavior Name]**

> When [situation/trigger context],  
> I want to [motivation/action],  
> so I can [outcome/benefit].

- **Frequency:** [how often this job arises]
- **Current solution:** [how users solve this today]
- **Validates hypothesis:** H1

**B2: [Behavior Name]**

> When [situation/trigger context],  
> I want to [motivation/action],  
> so I can [outcome/benefit].

- **Frequency:** [how often this job arises]
- **Current solution:** [how users solve this today]
- **Validates hypothesis:** H1, H2

---

## Features

### Feature Overview

| ID | Feature | Enables Behavior | Priority | Dependencies |
|----|---------|------------------|----------|--------------|
| F1 | [Feature name] | B1 | P0 | None |
| F2 | [Feature name] | B1, B2 | P0 | F1 |
| F3 | [Feature name] | B2 | P1 | F1 |

### Critical Path

```
F1 (foundational)
    ├── F2 (depends on F1)
    │   └── F4 (depends on F2)
    └── F3 (depends on F1, parallel to F2)
```

### Feature Details

#### F1: [Feature Name]

**Enables behavior:** B1  
**Core capability:** [what the feature does]  
**User value:** [why it matters to user]

#### F2: [Feature Name]

**Enables behavior:** B1, B2  
**Core capability:** [what the feature does]  
**User value:** [why it matters to user]

---

## Requirements

### F1: [Feature Name]

**R1.1: [Requirement statement]**

Acceptance criteria:
- When [situation], user can [action], resulting in [outcome]
- When [edge case], system [handles gracefully]

**R1.2: [Requirement statement]**

Acceptance criteria:
- When [situation], user can [action], resulting in [outcome]

### F2: [Feature Name]

**R2.1: [Requirement statement]**

Acceptance criteria:
- When [situation], user can [action], resulting in [outcome]
- When [edge case], system [handles gracefully]

---

## Validation Strategy

### Hypothesis Experiments

**Experiment for H1:**

| Attribute | Value |
|-----------|-------|
| Experiment type | [A/B test / Usability test / Smoke test / Wizard of Oz / etc.] |
| Sample | [who, how many] |
| Duration | [timeframe] |
| Success metric | [specific threshold] |
| Failure metric | [when to abandon/pivot] |
| Minimum viable test | [smallest version that could validate] |

**Experiment for H2:**

| Attribute | Value |
|-----------|-------|
| Experiment type | [type] |
| Sample | [who, how many] |
| Duration | [timeframe] |
| Success metric | [specific threshold] |
| Failure metric | [when to abandon/pivot] |
| Minimum viable test | [smallest version that could validate] |

### Success Metrics

| Metric | Target | Measurement Method | Timeline |
|--------|--------|-------------------|----------|
| [Primary metric] | [threshold] | [how measured] | [when to evaluate] |
| [Secondary metric] | [threshold] | [how measured] | [when to evaluate] |
| [Leading indicator] | [threshold] | [how measured] | [when to evaluate] |

---

## Traceability Matrix

| Requirement | Feature | Behavior | Hypothesis | Assumption |
|-------------|---------|----------|------------|------------|
| R1.1 | F1 | B1 | H1 | A1, A2 |
| R1.2 | F1 | B1 | H1 | A1 |
| R2.1 | F2 | B1, B2 | H1, H2 | A1, A3 |

---

## Open Questions

- [ ] [Question requiring further discovery]
- [ ] [Unresolved technical consideration]
- [ ] [Stakeholder input needed]

---

## Non-Goals

Explicitly out of scope for this PRD:

- [Thing we're NOT building]
- [User segment we're NOT targeting]
- [Use case we're NOT supporting]
- [Integration we're NOT doing yet]

---

## Appendix

### Glossary

| Term | Definition |
|------|------------|
| [Term] | [Definition] |

### References

- [Link to related research]
- [Link to design mockups]
- [Link to technical specs]
