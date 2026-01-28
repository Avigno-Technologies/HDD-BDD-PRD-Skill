# Hypothesis-Driven PRD Skill

A Claude skill for creating Product Requirements Documents using Hypothesis-Driven Development (HDD) and Jobs-to-be-Done (JTBD) frameworks.

## Overview

This skill guides product teams through a systematic process that traces from business assumptions to testable requirements:

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
Traceability matrix connecting all layers
```

## The Problem This Solves

Traditional PRDs suffer from:
- **Features without rationale** — Requirements exist, but no one remembers *why*
- **Untested assumptions** — Business beliefs treated as facts
- **No falsification criteria** — No definition of what "failure" looks like
- **Disconnected metrics** — Success metrics not traced to business assumptions
- **Translation gaps** — Business, product, engineering, and growth speak different languages

## Key Features

- **Hypothesis format with falsification:** Every hypothesis includes "we'll know we're wrong when..."
- **JTBD behavioral specifications:** "When [situation], I want to [action], so I can [outcome]"
- **Full traceability:** Every requirement traces back through Feature → Behavior → Hypothesis → Assumption
- **Flexible scope:** Supports single features, feature clusters, or full MVPs
- **Validation-first:** Experiment design is part of the PRD, not an afterthought
- **Skippable discovery:** Quick mode for teams with clarity, thorough mode for exploration

## Skill Structure

```
hypothesis-driven-prd/
├── SKILL.md                           # Main workflow (7 steps)
├── assets/
│   └── prd-template.md                # Ready-to-use PRD template
└── references/
    └── bmc-and-hypotheses.md          # BMC question bank + hypothesis patterns
```

## Installation

### For Claude.ai Projects
1. Download `hypothesis-driven-prd.skill`
2. Upload to your Claude project as a skill

### Manual Use
Copy the contents of `SKILL.md` into your Claude conversation or project instructions.

## Usage

Start a conversation with Claude and describe your product/feature idea. The skill will guide you through:

1. **Scope Discovery** — Single feature, cluster, or MVP?
2. **Business Model Canvas** — Surface assumptions (skippable per block)
3. **Hypotheses** — Transform assumptions into testable predictions
4. **Behaviors** — Define target user behaviors in JTBD format
5. **Features** — Map behaviors to enabling features
6. **Requirements** — Write acceptance criteria with validation
7. **Output** — Generate PRD with traceability matrix

## Hypothesis Format

```
If [condition/we build X], 
then [prediction/users will do Y], 
because [rationale/underlying belief].

We'll know we're right when: [specific metric threshold]
We'll know we're wrong when: [falsification criteria]
```

## JTBD Behavior Format

```
When [situation/trigger context],
I want to [motivation/action],
so I can [outcome/benefit].
```

## Who This Is For

| Stakeholder | Value |
|-------------|-------|
| **Business/Founders** | Surface and test implicit beliefs about the market |
| **Product Owners** | Defensible prioritization traced to business risk |
| **Engineers** | Clear acceptance criteria with context on *why* |
| **Growth/Acquisition** | Connect acquisition strategy to product capability |

## Documentation

- `hypothesis-driven-prd-overview.md` — Full STAR analysis of the framework design
- `references/bmc-and-hypotheses.md` — Deep-dive BMC questions and hypothesis patterns

## License

MIT

## Contributing

Issues and PRs welcome. This framework is designed to evolve based on real-world usage.

---

*Created January 2026*
