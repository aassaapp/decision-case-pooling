---
schema_version: 3
id: "REPLACE-WITH-UUID-V4"
title: "A specific, evidence-backed decision title"
project: null
work_date: null
recorded_at: "YYYY-MM-DD"
updated_at: "YYYY-MM-DD"
status: candidate
familiarity:
  codebase: unknown
  stack: unknown
decided_by: unknown
user_review: unknown
tags: []
# Include only material phases supported by source evidence.
# Allowed phases: problem_framing, hypothesis_generation, experiment_design,
# implementation, result_interpretation, final_decision.
# lead: user | ai | joint | unknown
# human_actions: originated | challenged | constrained | selected | reviewed | delegated
# independent_judgment: demonstrated | not_observed | unknown
# All evidence IDs must appear in the Evidence section below.
contributions:
  problem_framing:
    lead: unknown
    human_actions: []
    independent_judgment: unknown
    evidence: [E1]
---

# A specific, evidence-backed decision title

> Summarize the choice and its reasoning in one or two sentences. If the reasoning is not established, say so.

## 1. Situation

Describe the goal, operational constraints, relevant context, what was known, and what was uncertain at the time. Do not rewrite the entire project history.

## 2. Decision trigger

Explain which observation, failure, AI proposal, user concern, or change in constraints made a decision necessary. Cite evidence.

## 3. Options and decision

Only describe options actually considered. Explain the criteria, final choice, rejected alternatives, and the costs or risks knowingly accepted. Separate expected impact from measured outcomes.

## 4. Who contributed what

Reconstruct the consequential steps in chronological order. For each, identify:

- **Phase:** One of the canonical `contributions` phases.
- **AI:** Proposals, analysis, implementation, testing, or corrections actually made by AI.
- **Human:** Original reasoning, questions, constraints, challenges, delegated tasks, review, and decisions actually evidenced.
- **Independence:** Which *specific* human judgment was demonstrably independently supplied, if any. Selecting an AI proposal is not by itself independent origination.

Explain ambiguities and cite `[E#]`. Do not convert a user's lack of objection into review.

## 5. Validation, results, and limits

- **Question tested:**
- **Method, data, and baseline:**
- **Observed results (with units and conditions):**
- **Trade-offs or regressions:**
- **Not established:** What remains untested, confounded, subjective, or based on a small sample?

A completed implementation, a passing build, and a trustworthy user outcome are distinct claims.

## 6. Transferable lessons and learning gaps

Distinguish **the user's documented reflection** from **a retrospective interpretation suggested by AI**. Record the conditions under which a lesson could transfer. If independent mastery of any technique has not been assessed, state that explicitly. Do not present historical participation as proof of interview-ready competence.

## Evidence

- [E1] Source type; exact accessible path, commit, message excerpt, or test output; what it supports.

## Open questions

- Only consequential unanswered questions. Write `None` if there are none.
