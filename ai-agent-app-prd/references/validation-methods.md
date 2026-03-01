# Validation Methods for AI Agent Apps

Reference for designing experiments that validate AI agent app hypotheses. Use when building the Validation Plan (Step 5).

---

## Validation Methods Table

| Method | Effort | Fidelity | Time | When to Use | AI Agent Example |
|--------|--------|----------|------|-------------|------------------|
| **Smoke test landing page** | Low | Low | 1-3 days | Demand validation | Landing page describing what the agent does, measure signups |
| **Wizard of Oz** | Medium | High | 1-2 weeks | Complex agent behavior | Human manually responds as if AI, measure satisfaction |
| **Hardcoded prototype** | Medium | Medium | 3-7 days | Core interaction testing | Agent with scripted responses for top 5 scenarios |
| **Concierge (manual agent)** | Low | High | 1-2 weeks | Value proposition testing | Manually research and deliver what the agent would |
| **A/B test** | High | High | 2-4 weeks | Optimization after launch | Compare two agent behaviors or UI approaches |
| **Prompt-only MVP** | Low | Medium | 1-3 days | AI capability testing | Build agent with prompts only (no RAG, no tools) to test if AI adds value |
| **Fake door test** | Low | Low | 1-3 days | Feature demand within agent | Add button for unbuilt capability, measure clicks |

**How to read this table:**
- **Effort** reflects engineering and design investment, not elapsed time
- **Fidelity** measures how closely the test resembles the real product experience
- **Time** is wall-clock time from decision to first results
- Higher fidelity is not always better. Match fidelity to the risk level of the hypothesis

---

## Validation Sequence Guide

Order experiments to maximize learning per dollar spent. Cheap, fast tests eliminate bad ideas before you invest in expensive ones.

### Phase 1: Validate Demand (Week 1-2)

**Goal:** Confirm that people actually want what the agent would do.

**Methods:** Smoke test landing page, Fake door test

- Describe the agent's value proposition in plain language
- Measure inbound interest: signups, waitlist joins, click-throughs
- Do not build any AI yet. If nobody wants the outcome, the technology is irrelevant
- Test multiple positioning angles if the first does not convert

**Key question answered:** Would people seek out a solution to this problem?

### Phase 2: Validate Value Proposition (Week 2-4)

**Goal:** Confirm that the delivered result is actually valuable, even if delivered manually.

**Methods:** Concierge (manual agent), Wizard of Oz

- Deliver the agent's intended output by hand (you are the agent)
- Measure satisfaction, willingness to pay, and repeat usage
- This tests the value of the outcome independent of the AI delivery mechanism
- Pay close attention to what users actually do with the output, not what they say

**Key question answered:** If we could deliver this perfectly, would people value it?

### Phase 3: Validate AI Capability (Week 3-6)

**Goal:** Confirm that AI can deliver the value at acceptable quality.

**Methods:** Prompt-only MVP, Hardcoded prototype

- Build the simplest possible AI version (system prompt + model, nothing else)
- Compare AI output quality to the manual baseline from Phase 2
- Identify where AI falls short and whether those gaps are dealbreakers
- Test edge cases, failure modes, and recovery paths

**Key question answered:** Can AI deliver this at the quality bar users require?

### Phase 4: Validate Full Product (Post-launch)

**Goal:** Optimize the live product experience.

**Methods:** A/B test

- Compare agent behaviors, UI treatments, prompt strategies, or model choices
- Requires sufficient traffic for statistical significance
- Only invest here after Phases 1-3 confirm the core value proposition
- Focus A/B tests on the highest-leverage variables first

**Key question answered:** Which specific implementation performs best?

### Sequencing principles

- Never skip Phase 1. Building AI before validating demand is the most expensive mistake
- Phases 2 and 3 can partially overlap if the team can run parallel experiments
- Each phase should produce a clear go/no-go/pivot decision before the next phase begins
- Budget at least one pivot between each phase. Plans rarely survive first contact with users

---

## AI-Agent-Specific Validation Considerations

AI agent apps introduce testing challenges that do not exist in traditional software. Address these explicitly in your validation plan.

### Testing conversational quality without building the full agent

- Use the Wizard of Oz method: a human responds via chat interface pretending to be AI
- This establishes the satisfaction ceiling. If users are not satisfied with perfect responses, they will not be satisfied with AI responses
- Record all conversations. They become your first evaluation dataset
- Track where the human "agent" struggles. These are the same places AI will struggle, often worse

### Validating RAG usefulness before building the pipeline

- Manually retrieve relevant documents and paste them into prompts
- Test whether grounding responses in source material actually improves user satisfaction
- Measure how often the user needs information from the knowledge base vs. general knowledge
- If 80% of queries can be answered without RAG, the pipeline may not be worth the investment yet

### Testing tool-use value without full integration

- Have a human perform the tool actions (API calls, data lookups, calculations) in real time
- Feed results back into the conversation manually
- Measure whether tool results change the user's assessment of the interaction
- Prioritize tool integrations by how frequently they were needed during manual testing

### Measuring AI quality

AI quality is not a single metric. Measure these dimensions independently:

**User satisfaction**
- Post-interaction rating (1-5 or thumbs up/down)
- Would-recommend score
- Repeat usage rate (strongest signal)

**Accuracy**
- Factual correctness of responses (requires human evaluation)
- Hallucination rate on questions with known answers
- Appropriate uncertainty expression (does the agent say "I don't know" when it should?)

**Speed**
- Time to first response
- Total interaction time vs. manual alternative
- Perceived speed (users tolerate longer waits for higher-quality output)

**Important:** User satisfaction and accuracy do not always correlate. A confident, wrong answer often scores higher on satisfaction than an accurate, hedged answer. Measure both.

### The prompt-only MVP pattern

The fastest way to test whether AI adds value to your use case:

1. Write a system prompt that describes your agent's role, constraints, and behavior
2. Deploy it with a basic chat interface (or just test via the API console)
3. Run real user scenarios through it
4. Evaluate output quality against your Phase 2 manual baseline

**What the prompt-only MVP tests:**
- Whether the base model is capable enough for your use case
- Which scenarios work well vs. which need additional infrastructure
- What the failure modes look like (this shapes your engineering roadmap)

**What the prompt-only MVP does NOT test:**
- Performance with your specific data (that requires RAG)
- Complex multi-step workflows (that requires tool use)
- Production-grade reliability (that requires engineering)

Start here. If the prompt-only MVP cannot handle the core scenario at all, adding RAG and tools will not save it. If it handles the core scenario well, you know exactly where to invest engineering effort.

---

## Experiment Design Template

Use this template for each hypothesis you plan to validate. One experiment per hypothesis keeps results clean and actionable.

> **Experiment for H[n]:**
>
> | Attribute | Value |
> |-----------|-------|
> | Experiment type | [method from table above] |
> | What we're testing | [specific hypothesis element] |
> | Sample | [who, how many, how recruited] |
> | Duration | [timeframe] |
> | Success metric | [specific threshold] |
> | Failure metric | [when to abandon/pivot] |
> | Minimum viable test | [smallest version that validates] |
> | AI-specific consideration | [what is unique about testing this with AI] |

### Filling out the template

**Experiment type:** Pick the method that matches your current phase and the hypothesis risk level. Default to the cheapest method that can disprove the hypothesis.

**What we're testing:** Be precise. Not "users will like the agent" but "users who receive AI-generated weekly reports will open them at a higher rate than the current email digest."

**Sample:** Define the target user segment, minimum sample size for confidence, and recruitment channel. For qualitative tests (Wizard of Oz, concierge), 5-8 users is often sufficient. For quantitative tests (A/B, smoke test), calculate required sample size based on expected effect size.

**Duration:** Set a fixed timebox. Open-ended experiments drift and never conclude.

**Success metric:** A specific number that, if reached, means the hypothesis is supported. Example: "40% of waitlist signups convert to active users within 7 days."

**Failure metric:** A specific number that, if reached, means the hypothesis is falsified and you should pivot or abandon. This is not just "below success metric." Define what outcome is bad enough to warrant a direction change.

**Minimum viable test:** The absolute smallest version of the experiment that still produces a valid signal. If the full experiment takes 2 weeks, what could you learn in 2 days?

**AI-specific consideration:** What makes this experiment different because AI is involved? Common entries:
- "AI output quality may vary between runs; test the same scenario multiple times"
- "Users may not trust AI for this task; measure trust separately from satisfaction"
- "Model costs at scale would be $X/month; validate unit economics alongside quality"
- "Latency for this task is Y seconds; test whether users will wait"

### Example: Completed template

> **Experiment for H1: Contract review agents save legal teams time**
>
> | Attribute | Value |
> |-----------|-------|
> | Experiment type | Wizard of Oz |
> | What we're testing | Legal professionals find AI-flagged contract issues valuable enough to change their review workflow |
> | Sample | 6 in-house counsel from mid-market companies, recruited via LinkedIn outreach |
> | Duration | 2 weeks (3 contracts per participant) |
> | Success metric | 5 of 6 participants say they would use this weekly; average review time drops 30% |
> | Failure metric | Fewer than 3 of 6 find the flagged issues relevant; participants ignore AI suggestions |
> | Minimum viable test | 2 participants, 1 contract each, 3-day turnaround |
> | AI-specific consideration | Human "agent" should deliberately include 1-2 borderline flags per contract to test whether users trust and act on uncertain findings |
