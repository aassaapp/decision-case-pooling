---
name: decision-case-pooling
description: Extract evidence-backed engineering decision cases from AI-assisted development conversations, code changes, and validation results. Distinguish decision ownership, phase-level contributions, and observed evidence of independent human judgment. Use only when asked to capture, update, review, or analyze decision cases; not for routine implementation or generic summaries.
---

# Decision Case Pooling

Capture what happened in AI-assisted engineering, **who contributed what**, and **what the available evidence can actually establish**. The goal is not to manufacture portfolio success stories or to assign credit by intuition. The goal is to accumulate traceable decision cases that support technical reflection, learning plans, and honest career narratives.

Default output language: English. Read [the case template](references/case-template.md) before creating or updating a case.

## 1. Scope and operating modes

Use only the current accessible conversation, user-specified work records, relevant code and commits, and actual validation results. Do not imply access to earlier conversations or tools that are not available. Do not collect from unrelated projects or accounts.

- **Discover:** Identify up to three high-value candidate decisions from the specified work. Show fewer if evidence is thin; do not invent cases.
- **Capture:** Write one Markdown file per material decision when the user requests saving. A draft request produces a draft only.
- **Enrich:** Add new evidence or test results to an existing case without overwriting user-authored judgments.
- **Analyze:** Aggregate cases to identify *observed* patterns of human judgment, AI delegation, review, and missing evidence. Do not convert observed participation into a claim about general competence.

This skill does not run automatically, monitor work in the background, require an MCP server, or require Notion. Publishing, pushing, or uploading is separate from locally saving a case and requires appropriate user authorization.

## 2. Case selection

Prefer a decision for which there is a consequential choice, a meaningful uncertainty, and supporting evidence. Typical examples:

- A human challenges an AI-generated assumption, diagnosis, or metric.
- A human independently frames the problem or supplies a domain constraint that changes the solution.
- AI proposes a method, a human selects it after examining trade-offs, and the choice is validated.
- AI independently diagnoses, implements, tests, or corrects an issue, with or without later human review.
- A test, deployment, or user observation causes an earlier decision to be reversed.
- The team deliberately delegates a task with measurable acceptance criteria.
- A proposed optimization is rejected because it damages a more important property.

Neither human intervention nor a positive result is required. Record informative AI-led decisions, unreviewed delegation, failed experiments, reversals, and unresolved questions. If an action is merely routine execution with no substantive decision, do not promote it into a case.

## 3. Evidence discipline

Distinguish these categories in every case:

1. **Observation:** What the available logs, code, tools, tests, or user statements directly establish.
2. **Contemporaneous rationale:** The reason explicitly stated *at the time* of the choice.
3. **Retrospective interpretation:** A later explanation, hypothesis, or AI-generated lesson.
4. **Unknown:** A fact that the sources do not establish.

Never manufacture alternatives, motivations, verification, impact, dates, or human participation. Final code does not show who thought of an idea; a merged commit does not show who reviewed it. Silence is not review. Approval is not proof of independently derived reasoning. A passing test does not establish production correctness.

Attach `[E1]`, `[E2]`, ... markers to material claims, and list the corresponding accessible sources in **Evidence**. Prefer specific quotations (brief), file paths, commits, test names, and reproducible command/output references over vague summaries. Record unavailable evidence as unavailable rather than filling the gap with inference.

**Sensitive information:** Do not copy API keys, access tokens, credentials, personal data, confidential business records, or internal-only code into public examples or exported case pools. Redact evidence excerpts when necessary, without pretending redacted data was inspected. Do not publish any case without a separate authorization and an appropriate review.

## 4. Keep ownership, contribution, and competence separate

These are **three different questions**. Do not infer one from another.

### A. Decision ownership (`decided_by`)

Who ultimately chose the adopted option?

- `user`: The human made the final choice. This includes choosing an option originally proposed by AI.
- `ai`: AI made and executed the final choice without a recorded human selection.
- `joint`: The final choice was substantively co-developed, and neither party solely chose it.
- `unknown`: Evidence does not establish final ownership.

`decided_by: user` **does not imply** independent origination or technical mastery. Preserve this field for compatibility with existing v2 case pools.

### B. Review (`user_review`)

- `reviewed`: The sources show that the human examined the particular decision or its supporting evidence.
- `not-reviewed`: The human explicitly says the decision was not reviewed.
- `unknown`: Neither is established; lack of objection is **not** review.

Review of an outcome and independent reproduction of its reasoning are separate. Keep `user_review` for compatibility with v2.

### C. Phase-level contribution (`contributions`)

Describe the meaningful phases of the decision. Record only phases material to the case; there is no requirement to fill every phase. Use these canonical phase names:

1. `problem_framing`: Identifying the actual problem, constraints, and scope.
2. `hypothesis_generation`: Proposing explanations or candidate approaches.
3. `experiment_design`: Choosing test design, metrics, data, controls, or acceptance criteria.
4. `implementation`: Writing code, configuring systems, or executing the change.
5. `result_interpretation`: Checking results, challenging conclusions, and diagnosing failures.
6. `final_decision`: Choosing, rejecting, rolling back, or deliberately deferring the approach.

Each recorded phase has:

- `lead`: `user | ai | joint | unknown` — who principally performed this phase, **not** who owns the whole case.
- `human_actions`: an array drawn from `originated | challenged | constrained | selected | reviewed | delegated`. Use `[]` if no human action is observable; do not read it as a statement that no action occurred. These are evidence-backed actions, not personality traits.
- `independent_judgment`: `demonstrated | not_observed | unknown` — whether the sources **demonstrate independent human judgment in this phase**. This is a property of the evidence, never a global assessment of the person's ability.
- `evidence`: one or more references such as `[E1]`. If no adequate source exists, omit the phase rather than inventing a role. Use `unknown` where a phase is material but the source does not establish who led it.

### What qualifies as `independent_judgment: demonstrated`?

The human must have contributed a **specific, substantiated judgment that is not merely copied or selected from an already-present AI proposal**. Examples include:

- The human identifies an overlooked constraint or a faulty assumption before AI introduces it.
- The human produces an alternative hypothesis with a reason or concrete observation.
- The human independently challenges an AI-generated conclusion with a falsifiable counterexample or an explicitly argued criterion.
- The human proposes a test or acceptance criterion before AI suggests that particular test.

An original challenge *to* an AI proposal can count as independent judgment, **but only for the particular novel reasoning or constraint visible in the challenge**, not for the entire AI-generated approach.

Use `not_observed` when the relevant source is adequate and shows only delegation, acceptance, routine execution, or adoption of AI-proposed reasoning. Use `unknown` when provenance is incomplete or ambiguous. **Neither value means the human lacks the ability.** Do not infer independence from a final `user` decision, a commit author, a strong outcome, seniority, or an expression of agreement.

Where a phase contains multiple material sub-decisions with different leads, either explain them precisely in the case narrative or split into separate cases if the decisions are genuinely distinct. Avoid arbitrary attribution by percentage.

### Compact example

```yaml
contributions:
  problem_framing:
    lead: user
    human_actions: [originated, challenged]
    independent_judgment: demonstrated
    evidence: [E1, E2]
  hypothesis_generation:
    lead: ai
    human_actions: []
    independent_judgment: not_observed
    evidence: [E3]
  experiment_design:
    lead: ai
    human_actions: [constrained]
    independent_judgment: demonstrated
    evidence: [E4]
  implementation:
    lead: ai
    human_actions: [delegated]
    independent_judgment: not_observed
    evidence: [E5]
  final_decision:
    lead: user
    human_actions: [selected, reviewed]
    independent_judgment: not_observed
    evidence: [E6]
```

The `experiment_design` example is `demonstrated` **only if E4 contains a genuinely original, reasoned constraint supplied by the user**. If E4 merely selects an AI-proposed constraint, use `not_observed`.

## 5. Case frontmatter and status

Use `schema_version: 3` for newly captured cases. Preserve the v2 keys and their meanings:

- `id`: UUID v4. Stable across updates, file moves, and renames.
- `title`, `project`, `work_date`, `recorded_at`, `updated_at`.
- `status`: `candidate | needs-context | confirmed | written`.
- `familiarity.codebase` and `familiarity.stack`: `unfamiliar | familiar | unknown`.
- `decided_by`, `user_review`, `tags`.
- New: **sparse** `contributions` mapping defined above.

Dates use directly supported `YYYY-MM-DD` strings; use `null` for an unknown date or project. `written` means processed into an article/case study, **not** publicly released. A new case begins as `candidate` or `needs-context`. `confirmed` requires explicit human confirmation of the core decision and stated reasons. Do not treat a thumbs-up to a generated implementation as confirmation of every claim in a retrospective case.

When a confirmed case gains speculative or unverified substantive claims, mark those parts and move it back to `needs-context`; adding a source citation alone need not change the status.

## 6. Case body

Use the template headings, with specific evidence:

1. **Situation:** Goal, constraints, knowns, unknowns.
2. **Decision trigger:** Why a choice was necessary; what evidence or proposal raised the question.
3. **Options and decision:** Actually considered alternatives, criteria, the adopted choice, and accepted trade-offs.
4. **Who contributed what:** A chronological account of phase-level human and AI contributions. Explain exactly which reasoning, if any, was independently human-originated. Explicitly distinguish delegation from review.
5. **Validation and limits:** What was measured, observed results, and what the test did *not* establish.
6. **Transferable lessons and remaining knowledge gaps:** Separate a human's contemporaneous reflection from an AI retrospective suggestion. Identify independently explainable work versus work requiring further learning only when supported by direct assessment.
7. **Evidence:** `[E#]` mapping to accessible records.
8. **Open questions:** Only material unknowns; `None` if there are none.

An AI-authored case is an **attribution hypothesis backed by cited records**, not a notarized proof of human ability. When asked for an assessment of independence, explain which observed acts meet the rule and which are uncertain.

## 7. Saving and updating cases

Maintain one central case pool so patterns can be compared across projects:

```text
<central-case-pool>/
├── index.md
└── <project>/
    └── YYYY-MM-DD-short-topic.md
```

1. Prefer a folder explicitly chosen by the user. Otherwise read `.casepool` from the current work repository if it exists; its contents are the target path (ignore comment lines beginning with `#`). If absent, use a known central pool. Ask for the destination only if saving is requested and the path cannot be resolved.
2. Do not create `.casepool` in a work repository without consent. Discuss whether the pointer should be Git-tracked.
3. Before saving, check for an existing case involving the same decision and evidence; enrich it rather than duplicating it. Similar topics are not necessarily the same decision.
4. Filename: `YYYY-MM-DD-short-topic.md`; use the record date if the work date is unknown, but preserve `work_date: null`. Avoid overwriting collisions.
5. Maintain a single central `index.md` with title, work date, status, tags, and relative links. Preserve any user-written sections and never duplicate complete case bodies into the index.
6. After saving, check frontmatter validity, source references, internal links, and whether sensitive material was included. Briefly report the path and unresolved questions.

## 8. Pattern analysis

Analyze only cases in the selected scope. Explicitly state case count, date range, selection mechanism, and missing-data counts. The pool is a **purposive sample of interesting decisions**, not a census of working hours or a representative sample of all engineering activity.

Required outputs, when supported:

- Distribution of `decided_by` and `user_review`, including `unknown` counts and denominators.
- For each phase: counts of `lead`, `human_actions`, and `independent_judgment`, including only cases that contain that phase; show that denominator explicitly.
- Distinguish **observed independent human judgments**, **AI-supported human decisions**, and **AI-led delegated execution**; do not treat these categories as mutually exclusive at the whole-case level.
- Identify recurring strengths, apparent learning opportunities, and unverified competencies with source-backed examples and counterexamples.
- Check whether multiple cases come from the same task, conversation, or day. Mark those cases as **correlated** and do not treat their number as independent replications.
- Report selection bias, uncertain attribution, unobserved review, missing code/experiment provenance, and the limits of AI-authored retrospection.

**Never calculate a single 'human contribution percentage', 'AI dependency score', or 'engineering skill score' from these labels.** Phase counts do not measure effort, impact, difficulty, or job-market readiness. If comparing projects, account for different task types and visibility before interpreting differences.

For a learning-oriented follow-up, separately ask the human to explain or reproduce a small number of AI-led decisions without assistance. Label the resulting evidence as a **new assessment**, not as a retroactive rewrite of historical independence.

## 9. Version compatibility and migration

**Do not rewrite an existing v2 case merely because this skill now uses v3.** It remains readable and valid; absent `contributions` fields mean **not yet classified**, not AI-led and not unreviewed.

When the user requests an upgrade:

1. Read the source conversation, code changes, and tests referenced by that specific case.
2. Keep its `id`, `work_date`, `recorded_at`, and existing factual content. Update `updated_at` only when actually editing.
3. Add `contributions` for evidenced phases only. Never infer a phase from `decided_by`, `user_review`, `familiarity`, or narrative tone alone.
4. When evidence is inaccessible, leave the phase absent or use `unknown` with an explicit limitation; do not silently manufacture provenance.
5. Set `schema_version: 3` only after adding a valid phase mapping and checking `[E#]` references. Preserve unmodified cases as v2.
6. Update the central index without breaking old links. Do not overwrite human-edited case text.
7. After migration, compare the original and updated case for factual drift and over-attribution.

For older v1 cases, migrate existing fields to v2 semantics before adding v3 contributions; unknown ownership/review remains `unknown`.

## 10. Usage examples

- "Find decision-case candidates from this session."
- "Capture the architecture decision and record each participant's role."
- "Update this case with today's regression-test results."
- "Reclassify these cases using v3 contribution phases, without guessing."
- "Analyze where I independently challenged AI versus delegated its judgment."
- "Which AI-led design choices should I learn to explain without assistance?"
