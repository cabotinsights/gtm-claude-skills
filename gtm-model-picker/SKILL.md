---
name: gtm-model-picker
description: Recommends which Claude model (Fable, Opus, Sonnet or Haiku), effort level and subagent setup to use for a go-to-market task. Use when someone asks which model, effort level or setup to use for sales, marketing, RevOps or GTM work, such as account plans, outbound sequences, lead routing, CRM cleaning, ICP or scoring models, territory design, or enrichment runs.
---

# GTM Model Picker

Pick the cheapest Claude setup that does a GTM task well. Return a model, an effort level, a subagent yes/no, a short reason and a rough cost note.

## Source and honesty rule

Model names, prices, effort defaults, the effort-before-model order and the benchmark figures below come from Anthropic's guide "Which Claude model should you build with?" (claude.dev/blog/which-claude-model-should-you-build-with/).

The guide's examples are about coding and knowledge work. Every GTM example in this skill is a mapping of that guidance onto sales and marketing work, and is a **recommendation**, not something the guide states. When you give an answer, do not present a GTM mapping as an Anthropic claim. Do not quote a figure that is not in the price table or benchmark section below. If you estimate tokens or cost, say it is an estimate and state the assumption.

## Facts from the guide

### Prices (API list, per million tokens)

| Model | Input / Output | Cache reads | Default API effort |
|---|---|---|---|
| Haiku 5.5 | $0.10 / $0.50 up to 100K prompt tokens; $0.50 / $2.50 over 100K | $0.01 up to 100K; $0.05 over 100K | medium |
| Sonnet 5.5 | $2 / $10 | $0.10 | high (medium in Claude Code) |
| Opus 5.5 | $4 / $20 | $0.20 | medium |
| Fable 5.2 | $8 / $40 | $0.20 | high (medium in Claude Code) |

All four have a 1M-token context window and 128K max output. Haiku 5.5 is the only model whose price changes with prompt length, so keep Haiku prompts under 100K tokens.

Effort levels: low, medium, high, xhigh, max.

### Starting point

- Start most work on **Opus 5.5 at medium**.
- Move down to **Sonnet 5.5** for well-scoped work. A clear spec does not make a task Sonnet-appropriate on its own; hard work may still need Opus.
- Move down to **Haiku 5.5** for classification, routing, tagging, extraction, and subagents doing search, reading or summarising.
- Move up to **Fable 5.2** for the hardest work (the guide names coding, finance and science), architecture planning, leading many subagents, or long unattended runs.
- For replies a person is waiting on, start with **Sonnet 5.5 at low or medium**.

### Benchmark points

- Opus 5.5 on SWE-bench Pro: medium scored about 2.5 points below high at about 70% of the cost; low scored about 8 points below at about a third of the cost; xhigh scored about 1.4 points above high at 2.5 times the cost.
- Opus 5.5 at low with failures re-run at high passed about 97% of tasks at about $0.17 each. All tasks at high passed 95.3% at $0.29 each.
- FrontierCode 2.0: Fable 5.2 at high scored 45.3% against 45.5% for Opus 5.5 at max, at roughly half the cost per task. The guide says to test Fable 5.2 before raising Opus past high on a hard task.
- An Opus 5.5 lead splitting bulk work across parallel Sonnet 5.5 workers cost about a third less than Opus 5.5 alone at default effort, with about the same score (internal defect-search benchmark).
- Price per token does not always predict cost per task. In one 20-problem benchmark, two problems accounted for 43% of spend.

## Decision process

Work through these four questions in order. Ask the user only if the answer would change the recommendation and you cannot infer it.

1. **Task type.** Which bucket does it fall in?
   - Classify, tag, route, extract, dedupe, summarise one record: Haiku territory.
   - Write or rewrite to a clear brief (emails, copy, battlecards, follow-ups): Sonnet territory.
   - Judgement across messy sources for a few high-value accounts or decisions: Opus territory.
   - Open-ended analysis that sets strategy for the whole GTM motion (ICP, scoring, sizing, territories): Fable territory.
2. **Volume.** One-off, a handful, or hundreds to thousands of rows? High volume pushes the per-row work down a model and makes subagents worth it.
3. **Reasoning needed.** Is there one right answer a check can confirm (a persona tag, a valid email), or does it need weighing evidence and trade-offs? More judgement pushes effort up first, then model.
4. **Cost and speed sensitivity.** Is this a batch job where cost per row matters, or a live reply someone is waiting on? Batch favours cheap model plus re-run of failures. Live replies favour Sonnet at low or medium.

Then decide:

- **Model:** start from the bucket in step 1. Default to Opus 5.5 at medium if unsure.
- **Effort:** start at the model's default. Lower it for simple, checkable work. Raise it one level for higher stakes before switching model.
- **Subagents:** yes if there are many independent rows or sources to read; no for a single document or decision.

## Model guide

All GTM examples in this section are recommendations based on the guide's general rules.

### Opus 5.5: the default for judgement work

Use when the output needs weighing of evidence, and getting it wrong costs a deal or a quarter. The guide's starting point for most work.

- Account plans for your top accounts
- QBR and board pipeline prep
- Lost-deal analysis from call notes and emails
- Mapping an enterprise buying committee

Effort: medium to start. High for a board pack or a top-five account.

### Sonnet 5.5: well-scoped work you repeat

Use when the brief is clear and the job is producing good writing or structured output against it, many times over.

- Outbound sequences by persona
- Call notes into follow-up emails
- Battlecards from competitor pages
- Landing page and ad copy per segment

Effort: medium for drafts. Low for live replies. High for a C-level email to a strategic account, rather than moving to Opus.

### Haiku 5.5: high-volume classification and extraction

Use when each row has a narrow, checkable answer and you have thousands of them.

- Tagging job titles to personas
- Inbound lead routing
- Sorting email replies (interested, not now, unsubscribe, out of office)
- CRM field cleaning and dedupe

Effort: low or medium. Validate output with a rule or a script and re-run failures rather than raising effort on every row.

### Fable 5.2: the hardest, open-ended work

Use when the task is a multi-step analysis with no fixed answer, it shapes the whole GTM plan, or it needs to lead many subagents.

- Rebuilding ICP and scoring from closed-won data
- Sizing a new market
- Territory redesign

Effort: start at medium or the API default (high). Twice the token price of Opus 5.5, so justify it by the size of the decision.

## Effort rules

From the guide, when a model is not good enough, try in this order:

1. **Give it a way to check its own work.** A validation rule, a required-field check, a lookup against CRM, a scoring rubric.
2. **Raise effort one level.**
3. **Move up a model**, starting at a lower effort than you used on the smaller one.

Further rules from the guide:

- Effort is the finer dial. It adds latency and cannot add knowledge or skill the model lacks. If the model does not know the market or the product, give it context, not more effort.
- **Where moving up is cheaper:** past high, one more level costs a lot for a small gain (Opus xhigh was 2.5 times the cost of high for about 1.4 points). Fable 5.2 at high matched Opus 5.5 at max for roughly half the cost. So if Opus at high is not good enough, test Fable at high before pushing Opus to xhigh or max.
- **Lowering effort:** for checkable batch work, run low first and re-run only the failures at high. On the guide's benchmark that beat running everything at high on both pass rate and cost.

GTM applications (recommendation):

- Sonnet at medium for persona emails; Sonnet at high for a C-level email to a strategic account.
- Enrichment at low effort, then re-run at high only the rows that fail validation (missing domain, bad email format, title not mapped).
- Opus at medium for a standard account plan; Opus at high for the top five; test Fable before going past high.

## Subagent pattern

The guide: keep the expensive model's context lean, let cheaper subagents do the verbose reading, keep judgement calls on the main model. Set a subagent's model in its definition.

GTM version (recommendation): the big model plans and decides; small models do the per-row legwork.

**Example: signal-led weekly call list**

1. Opus 5.5 at medium writes the brief: which signals count (hiring for target roles, new funding, leadership change), how to record evidence, what "no signal" looks like.
2. Haiku 5.5 subagents work through 500 accounts in batches, pulling hiring and funding signals into a fixed structure, one account per row, with a source link.
3. A script checks each row (source present, date in range, fields filled). Failed rows are re-run.
4. Opus 5.5 reads the structured summary, not the raw pages, and picks the 10 accounts worth calling this week, with one line on why each.

Example subagent definition:

```markdown
---
name: signal-puller
description: Pulls hiring and funding signals for a batch of accounts and returns one structured row per account.
tools: WebSearch, WebFetch, Read
model: haiku
---

For each account, find hiring for the listed roles and any funding round in the last 180 days. Return: account, signal type, date, one-line evidence, source URL. If nothing is found, return "no signal". Do not score or rank accounts.
```

For bulk writing too large for one context (for example, first drafts for 2,000 contacts across 12 personas), an Opus lead splitting work across parallel Sonnet workers is the guide's own bulk pattern.

## Answer format

Return exactly this, no preamble:

```
Recommended model: <model> 5.x
Effort: <low | medium | high | xhigh | max>, plus when to change it
Subagents: <Yes | No>, plus the split if yes
Why: <one or two lines>
Rough cost note: <based on the price table; label any token count as an estimate>
```

If the task is ambiguous, give the recommendation for the most likely reading and name the one fact that would change it.

## Worked examples

All cost notes below are estimates built from the guide's prices with stated token assumptions.

**1. "I've got 10,000 job titles from a list export. I need each tagged to one of our six personas."**

```
Recommended model: Haiku 5.5
Effort: low; re-run titles that fail validation (no persona, or "unclear") at medium
Subagents: Yes, Haiku workers on batches of a few hundred titles, keeping each prompt under 100K tokens
Why: Narrow, checkable classification at high volume is the guide's Haiku use case.
Rough cost note: Estimate, assuming about 100 input and 20 output tokens per title: about 1M input and 0.2M output, so roughly $0.20 at $0.10 / $0.50 per million.
```

**2. "Write a first-draft five-step outbound sequence for each of our four personas, for a new product launch."**

```
Recommended model: Sonnet 5.5
Effort: medium
Subagents: No, four sequences fit in one session
Why: Well-scoped writing against a clear brief that you will repeat each launch.
Rough cost note: Small job at $2 / $10 per million tokens; the brief, messaging doc and examples will be most of the input.
```

**3. "Prep our Q4 board pipeline review from the CRM export and the last quarter of deal notes."**

```
Recommended model: Opus 5.5
Effort: medium for the draft, high for the final narrative and risk calls
Subagents: Yes if the deal notes run past a few hundred pages: Haiku summarises each deal to a fixed template, Opus writes the review
Why: Judgement across messy sources where a wrong read is costly; summaries keep Opus's context lean.
Rough cost note: Opus is $4 / $20 per million. Most spend is reading notes, which is why Haiku summaries ($0.10 input) do that part.
```

**4. "Rebuild our ICP and lead scoring model from two years of closed-won and closed-lost data."**

```
Recommended model: Fable 5.2
Effort: start at medium; if gaps remain, re-run the weakest segments at high before trying anything above high
Subagents: Yes, Haiku cleans and normalises fields (industry, size bands, titles) row by row; Fable does the analysis and the model design
Why: Open, quantitative analysis that sets targeting for the whole team; the guide puts the hardest analytical work on Fable.
Rough cost note: Fable is $8 / $40 per million, twice Opus. Send it cleaned, aggregated data, not raw exports.
```

**5. "Our SDRs want instant suggested replies when a prospect answers a cold email. Which model?"**

```
Recommended model: Sonnet 5.5
Effort: low; raise to medium for replies from target accounts
Subagents: No; optionally route first with Haiku 5.5 (interested, objection, not now, unsubscribe) and only draft for the first two
Why: A person is waiting on it, and the guide says to start live replies on Sonnet at low or medium.
Rough cost note: $2 / $10 per million for drafts; the Haiku routing step costs a fraction of that per reply.
```
