# Hypothesis Patterns for AI Agent Apps

Reference for formulating testable hypotheses in Steps 1 (Agent Hypothesis) and 2 (Capability Hypotheses) of the AI Agent App PRD skill.

---

## Agent Hypothesis Patterns

Templates for the core agent hypothesis. Pick the pattern closest to the agent you are building and fill in every bracket.

### Information Retrieval Agent

> If I build an agent that finds and synthesizes **[domain]** information for **[audience]**,
> then **[outcome]**,
> because **[rationale]**.
> **Evidence:** [specific metric] exceeds [threshold] within [timeframe].
> **Falsification:** This agent is unnecessary if [audience] can already find and synthesize [domain] information in under [N] minutes using [existing tool].

**Example:**
> If I build an agent that finds and synthesizes medical-billing guideline information for independent physician offices,
> then coding error rates will drop by 30%,
> because staff currently spend 45+ minutes per claim cross-referencing CMS manuals.
> **Evidence:** Average lookup time < 5 minutes and coding accuracy > 95% in pilot.
> **Falsification:** This agent is unnecessary if offices already achieve > 95% accuracy using their existing EHR lookup tools.

---

### Task Automation Agent

> If I build an agent that automates **[workflow]** for **[audience]**,
> then **[outcome]**,
> because **[rationale]**.
> **Evidence:** [specific metric] exceeds [threshold] within [timeframe].
> **Falsification:** This agent is unnecessary if [audience] can complete [workflow] in under [N] minutes manually, or if an existing tool already automates it adequately.

**Example:**
> If I build an agent that automates weekly expense-report reconciliation for mid-market finance teams,
> then month-end close will shorten by 2 days,
> because reconciliation currently requires 6+ hours of manual copy-paste per cycle.
> **Evidence:** Reconciliation time < 30 minutes and error rate < 1% in a 4-week pilot.
> **Falsification:** This agent is unnecessary if teams can complete reconciliation in under 15 minutes using spreadsheet macros they already own.

---

### Decision Support Agent

> If I build an agent that helps **[audience]** decide **[decision type]**,
> then **[outcome]**,
> because **[rationale]**.
> **Evidence:** [specific metric] exceeds [threshold] within [timeframe].
> **Falsification:** This agent is unnecessary if [audience] already makes [decision type] with comparable accuracy/speed using [existing method].

**Example:**
> If I build an agent that helps e-commerce merchandisers decide which products to feature on the homepage,
> then featured-product click-through rate will increase by 20%,
> because merchandisers currently rely on gut instinct rather than real-time sales velocity data.
> **Evidence:** CTR on featured products > 8% (vs. 6.5% baseline) over a 30-day A/B test.
> **Falsification:** This agent is unnecessary if merchandisers' manual picks already outperform algorithmic suggestions in a blind test.

---

### Creative Assistant Agent

> If I build an agent that helps **[audience]** create **[output type]**,
> then **[outcome]**,
> because **[rationale]**.
> **Evidence:** [specific metric] exceeds [threshold] within [timeframe].
> **Falsification:** This agent is unnecessary if [audience] can produce [output type] of comparable quality in under [N] minutes without AI assistance.

**Example:**
> If I build an agent that helps SaaS marketing teams create ad-copy variations,
> then campaign launch time will drop from 5 days to 1 day,
> because copywriters spend 80% of their time on first-draft variations, not strategic refinement.
> **Evidence:** Time-to-launch < 8 hours and ad performance within 5% of human-only baseline in a 2-week test.
> **Falsification:** This agent is unnecessary if teams already generate variations in under 2 hours using existing template libraries.

---

### Customer-Facing Agent

> If I build an agent that handles **[interaction type]** for **[business]**,
> then **[outcome]**,
> because **[rationale]**.
> **Evidence:** [specific metric] exceeds [threshold] within [timeframe].
> **Falsification:** This agent is unnecessary if [business] can handle [interaction type] at current volume with existing staff and customers report satisfaction above [N].

**Example:**
> If I build an agent that handles first-tier product-return requests for a DTC apparel brand,
> then average resolution time will drop from 24 hours to under 5 minutes,
> because 70% of return requests follow a predictable, policy-based decision tree.
> **Evidence:** Resolution time < 5 minutes, CSAT > 4.2/5, and escalation rate < 15% over a 30-day pilot.
> **Falsification:** This agent is unnecessary if the brand's current support team already resolves returns within 30 minutes and CSAT is above 4.5.

---

## Capability Hypothesis Patterns

For each capability your agent might include, write a hypothesis that justifies its presence. If you cannot fill in the brackets convincingly, the capability may not be needed.

### 1. Conversational AI

> Users need **Conversational AI** because **[rationale]**.
> **Test:** We'll know this is needed when **[evidence]**.
> **Falsification:** This capability is unnecessary if **[condition]**.

**Example:**
> Users need **Conversational AI** because their queries are ambiguous and require multi-turn clarification before the agent can act.
> **Test:** We'll know this is needed when > 40% of user intents require at least one follow-up question to resolve.
> **Falsification:** This capability is unnecessary if > 90% of requests can be handled with structured forms or single-shot commands.

---

### 2. RAG / Knowledge Base

> Users need **RAG / Knowledge Base** because **[rationale]**.
> **Test:** We'll know this is needed when **[evidence]**.
> **Falsification:** This capability is unnecessary if **[condition]**.

**Example:**
> Users need **RAG / Knowledge Base** because the domain knowledge exceeds what fits in a single system prompt and changes frequently.
> **Test:** We'll know this is needed when the source material exceeds 50,000 tokens or is updated more than once per month.
> **Falsification:** This capability is unnecessary if the knowledge fits in a 4,000-token system prompt and is stable quarter-over-quarter.

---

### 3. Web Scraping / Research

> Users need **Web Scraping / Research** because **[rationale]**.
> **Test:** We'll know this is needed when **[evidence]**.
> **Falsification:** This capability is unnecessary if **[condition]**.

**Example:**
> Users need **Web Scraping / Research** because the information they need lives on third-party websites with no API and changes daily.
> **Test:** We'll know this is needed when > 50% of user queries require data not available in our own knowledge base or via existing APIs.
> **Falsification:** This capability is unnecessary if the data is available via a stable API or RSS feed that can be polled on a schedule.

---

### 4. Database Operations

> Users need **Database Operations** because **[rationale]**.
> **Test:** We'll know this is needed when **[evidence]**.
> **Falsification:** This capability is unnecessary if **[condition]**.

**Example:**
> Users need **Database Operations** because the agent must read and write user-specific records that persist across sessions.
> **Test:** We'll know this is needed when > 60% of users return for a second session and expect the agent to remember prior context.
> **Falsification:** This capability is unnecessary if users don't return more than once or if session data can be stored client-side.

---

### 5. Payment Processing

> Users need **Payment Processing** because **[rationale]**.
> **Test:** We'll know this is needed when **[evidence]**.
> **Falsification:** This capability is unnecessary if **[condition]**.

**Example:**
> Users need **Payment Processing** because the agent's core value loop includes a purchase or subscription step.
> **Test:** We'll know this is needed when > 10% of active users attempt to purchase within the agent experience in a smoke-test.
> **Falsification:** This capability is unnecessary if fewer than 5% of users would pay $1 for the service, or if payment can be handled outside the agent flow without friction.

---

### 6. External API Calls

> Users need **External API Calls** because **[rationale]**.
> **Test:** We'll know this is needed when **[evidence]**.
> **Falsification:** This capability is unnecessary if **[condition]**.

**Example:**
> Users need **External API Calls** because the agent must pull real-time data (e.g., weather, stock prices, shipping status) to complete its task.
> **Test:** We'll know this is needed when > 30% of agent responses require data that is not in the knowledge base and changes hourly or faster.
> **Falsification:** This capability is unnecessary if static or daily-snapshot data is sufficient for the use case.

---

### 7. File Processing

> Users need **File Processing** because **[rationale]**.
> **Test:** We'll know this is needed when **[evidence]**.
> **Falsification:** This capability is unnecessary if **[condition]**.

**Example:**
> Users need **File Processing** because they upload documents (PDFs, CSVs, images) that the agent must parse to deliver value.
> **Test:** We'll know this is needed when > 25% of users attempt to upload a file during their first session.
> **Falsification:** This capability is unnecessary if users can paste the relevant content as text, or if the data is already available via an API.

---

### 8. Browser Automation

> Users need **Browser Automation** because **[rationale]**.
> **Test:** We'll know this is needed when **[evidence]**.
> **Falsification:** This capability is unnecessary if **[condition]**.

**Example:**
> Users need **Browser Automation** because the agent must interact with third-party web apps that have no API (e.g., filling forms, clicking buttons, extracting page data).
> **Test:** We'll know this is needed when > 20% of the workflow steps require interacting with a website that offers no programmatic access.
> **Falsification:** This capability is unnecessary if every required action can be performed via an API, or if a simple web-scraping approach (no interaction) is sufficient.

---

## Falsification Patterns

Common falsification criteria for AI agent hypotheses. Use these as starting points and adjust the thresholds for your specific domain.

### Agent-Level Falsification

| Pattern | Template |
|---------|----------|
| **Manual-effort threshold** | This agent is unnecessary if users can accomplish [task] in under [N] minutes manually. |
| **Existing-tool threshold** | This agent is unnecessary if [existing tool] already solves the problem with [N]% satisfaction. |
| **Frequency threshold** | This agent is unnecessary if the average user encounters this need fewer than [N] times per [period]. |
| **Accuracy threshold** | This agent is unnecessary if it cannot match human accuracy within [N]% on [task]. |
| **Trust threshold** | This agent is unnecessary if fewer than [N]% of target users would trust an AI to handle [task]. |

### Capability-Level Falsification

| Capability | Falsification Template |
|------------|----------------------|
| **Conversational AI** | Conversational AI is unnecessary if > [N]% of requests are single-turn and unambiguous. |
| **RAG / Knowledge Base** | RAG is unnecessary if the knowledge fits in a [N]-token system prompt and changes less than [frequency]. |
| **Web Scraping / Research** | Web scraping is unnecessary if the data is available via [existing API/feed]. |
| **Database Operations** | A database is unnecessary if users don't return more than [N] times or if client-side storage suffices. |
| **Payment Processing** | Payment is unnecessary if fewer than [N]% of users would pay [$X] for the service. |
| **External API Calls** | External API calls are unnecessary if [static/daily data] is sufficient for the use case. |
| **File Processing** | File processing is unnecessary if users can paste content as text or data is already in an API. |
| **Browser Automation** | Browser automation is unnecessary if every required action has a programmatic API alternative. |

---

## Common Pitfalls

AI-agent-specific hypothesis mistakes to watch for during Steps 1 and 2.

### Vague or Unfalsifiable Hypotheses

- **"AI will make it better"** -- Better how? By what metric? This is not testable. Rewrite with a specific outcome and threshold.
- **"Users want an AI assistant"** -- Users want outcomes, not technology. Restate in terms of the job-to-be-done.
- **"This will save time"** -- How much time? Compared to what baseline? Add numbers or it cannot be falsified.

### Capability Overreach

- **Assuming RAG is needed for everything** -- If your knowledge base is small and stable, a well-crafted system prompt may be sufficient. Always check the token count first.
- **Adding payment processing "just in case"** -- Payment adds significant complexity (compliance, fraud, refunds). Only include it if your smoke test shows real willingness to pay.
- **Browser automation as a first resort** -- Browser automation is fragile and slow. Prefer APIs, then web scraping, then browser automation as a last resort.

### Audience Mistakes

- **Building for "anyone who uses the internet"** -- If you cannot name 10 specific people who have the problem, your segment is too broad.
- **Conflating "technically possible" with "users want this"** -- The question is never "can an AI do this?" but "will users choose an AI over their current approach?"
- **Overestimating willingness to interact with AI agents** -- Many users still prefer human support, static documentation, or manual workflows. Validate demand before building.

### Confirmation Bias

- **Only looking for success signals** -- Define what failure looks like before you run the experiment. If you cannot describe a failed outcome, the hypothesis is not falsifiable.
- **Ignoring negative evidence from early users** -- If 3 out of 5 pilot users abandon the agent mid-task, that is a signal, not an outlier.
- **Anchoring on the happy path** -- Test edge cases, error states, and "what if the LLM hallucinates?" scenarios. The happy path is the least informative test.
