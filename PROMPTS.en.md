# PM Prompt Cheat Sheet

Copy-paste prompts. Replace `[...]` with your own data. Where a skill from this repo exists, the prompt invokes it; for the rest the model does fine without a skill, as long as it gets the right constraints.

## Rules that make the difference

1. **Give the data, not a description of it.** Paste the transcript, CSV, ticket, email. "We have a lot of churn" gets you generalities; 200 rows of an export gets you conclusions.
2. **Name the decision.** "Help me decide between X and Y by Friday" instead of "analyze X".
3. **Make it ask first.** "Before you start, ask me up to 3 questions about what you don't know." This cuts out the guessing.
4. **Make it mark assumptions.** "Anything my data doesn't tell you, mark as [ASSUMPTION]; don't state it as fact."
5. **Limit the length.** "One page max" / "5 bullets". Without a limit you get a report.
6. **Ask for criticism, not praise.** "What's the weakest part?" works better than "what do you think?".

---

## Discovery

**Synthesize multiple interviews**
```
Below are [N] interview transcripts with [segment]. Extract:
1. Problems mentioned by at least 2 people (with the number of people and a quote for each).
2. What they do today instead of our product, and what it costs them (time/money).
3. Things that will surprise me — that contradict this assumption: [your assumption].
Don't generalize from a single statement. If only one person said something, say so.
[transcripts]
```

**Interview script** → `interview-script` skill
```
/pm-skill:interview-script Interviews with [segment] about how they [activity] today. Decision they should inform: [decision]. 30 minutes.
```

**Cluster feedback / requests**
```
Below are [N] customer requests. Group them by PROBLEM, not by the proposed solution.
For each group: problem name, number of requests, 2 quotes, which segment raises it most often.
At the end: which feature requests are actually the same problem.
[requests]
```

**Assumption map**
```
Idea: [description]. List the assumptions that must be true for this to work
(customer value, usability, viability, feasibility).
For each: what happens if it's false, how confident we are based on [evidence], the cheapest test we can run in a week.
Sort so the ones that kill the idea and that we're least sure of come first.
```

**Survey** → `survey-design` skill
```
/pm-skill:survey-design Decision: [decision]. Audience: [group]. Channel: [email / in-app]. 5 questions max.
```

---

## Strategy and decisions

**Decision memo**
```
I need to decide: [decision] by [date]. Options: [A], [B], [C (do nothing)].
For each option: what we gain, what we lose, what must be true for it to be the best, the cost of reversing it.
Recommend one, and say what signal within [period] should make us change our mind.
One page max.
```

**Red-team a plan** → `strategy-red-team` skill
```
/pm-skill:strategy-red-team [paste the plan / strategy / roadmap]
```

**Pre-mortem** → `pre-mortem` skill
```
/pm-skill:pre-mortem [paste the PRD or launch plan]. Launch: [date].
```

**Steelman the opposing view**
```
I believe [position]. Write the strongest possible argument AGAINST it, the way a competent
[CFO / CTO / head of sales] would make it. Then: which of these arguments actually undermine my position, and which don't.
```

**Competitor analysis (with web search)**
```
Research [competitor] on the web: pricing, product changes in the last 6 months, what customers complain about (G2, Reddit, Capterra).
Every claim with a link to its source. At the end: 3 things where they genuinely beat us in [our segment], and 3 where we beat them.
```

---

## Specs and delivery

**PRD** → `create-prd` skill
```
/pm-skill:create-prd [problem, for whom, how we'll measure success, links/files]
```

**Review an existing PRD**
```
/pm-skill:create-prd Review this PRD: first the gaps that block a decision, then what can be cut. [PRD]
```

**Cut scope to an MVP**
```
Here's the feature list for the first version: [list]. Goal of the first version: test whether [hypothesis].
Which features are essential to test that hypothesis, and which aren't? For the ones cut, say what to replace them with for now
(manual work, a mock, an off-the-shelf tool).
```

**Acceptance criteria and edge cases**
```
User story: [story]. Write acceptance criteria in Given/When/Then format.
Then separately: 10 edge cases the team will most likely forget (empty data, permissions, concurrent actions,
time zones, limits, network errors, rolling back a change). Mark which need a product decision and which are purely technical.
```

**Break an epic into stories**
```
Epic: [description]. Break it into stories that can each be shipped and shown to users on their own (vertical slices, not technical layers).
Order: first the one that will show fastest whether the whole epic makes sense.
```

---

## Data and metrics

**Define a metric**
```
I want to measure [what]. Propose an exact definition: numerator, denominator, time window, who we exclude.
List 3 ways this metric can go up while the product gets worse, and a guardrail metric for each.
```

**Metric tree**
```
Our main metric: [metric]. Break it into components (e.g. traffic × conversion × frequency) down to the level
a single team can move in a quarter. For each branch: which team owns it and where the data comes from.
```

**SQL from a schema**
```
Schema: [CREATE TABLE ... / table description]. Dialect: [Postgres/BigQuery].
Write a query for: [business question]. Explain the assumptions (e.g. how you count an active user, time zone)
and add a sanity-check query I can use to confirm the result makes sense.
```

**A/B test analysis** → `ab-test-analysis` skill
```
/pm-skill:ab-test-analysis Hypothesis: [..]. Primary metric: [..]. Guardrails: [..]. Data: [CSV / numbers per variant].
```

**Investigate: a metric dropped**
```
[Metric] dropped by [X%] since [date]. Data: [export / chart / numbers per segment].
List possible causes in this order: data/tracking bug → traffic mix change → seasonality → product change → customer change.
For each: how to confirm or rule it out in 30 minutes.
```

---

## Communication

**Status update for leadership**
```
Write an update for [audience] in this format: the most important point in the first sentence, then: progress vs plan,
risks along with what we need from them, next steps. 150 words max. No adjectives like "great" or "significant".
Facts: [notes]
```

**Saying no to a stakeholder**
```
[Person/team] is asking for [request]. We won't do it now because [reason]. Write a reply that:
says "no" in the first sentence, explains the reason in terms of their goal, offers an alternative or a date to revisit.
Tone: [direct / warm]. 100 words max.
```

**Release notes**
```
Here's the list of changes from tickets: [list]. Rewrite it as a note for users: the benefit instead of the technical change,
no ticket numbers or internal names. Separately: changes that require users to take action.
```

**Meeting notes**
```
Transcript: [..]. Extract only: decisions (who decided), tasks (who, what, by when), open questions.
If a task has no owner or due date, mark it [NO OWNER] / [NO DUE DATE]. Don't summarize the discussion.
```

---

## Critiquing your own work

**Skeptical reviewer**
```
You are a skeptical [CTO / CFO / head of sales] with 5 minutes for this document. What 3 questions will you ask in the meeting
that the document doesn't answer? What will you find not credible?
[document]
```

**Document consistency**
```
Check whether this document is internally consistent: do the goals match the metrics, the solution the problem,
the scope the timeline? List only the inconsistencies, quoting both places.
[document]
```

**Shortening**
```
Cut this text in half without losing any decision, number or task. Show what you removed.
[text]
```
