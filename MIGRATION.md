# Migration guide: v2 → v3

Version 3 adds **phase-level contribution provenance**. It does **not** change the meaning of `decided_by`, `user_review`, `familiarity`, or the existing case status fields.

## Rules

1. **Keep v2 cases readable.** Do not mass-upgrade them simply to make the version number uniform.
2. **Read the underlying evidence first.** Use the cited conversation, code, commits, and validation results. An old case summary is a lead, not sufficient proof of who originated an idea.
3. **Preserve identity and chronology.** Keep `id`, `work_date`, and `recorded_at`. Set `updated_at` to the actual edit date only when changed.
4. **Add only demonstrable phases.** Leave a phase out when irrelevant or untraceable. Do not convert a `user` final decision into independently generated problem framing.
5. **Use accurate uncertainty.** `not_observed` means the adequately available source shows no independent human judgment for that phase; `unknown` means the evidence is insufficient.
6. **Preserve user-authored narrative.** Annotate contradictions; do not silently overwrite human statements.
7. **Upgrade to `schema_version: 3` after validating the new mapping**, evidence references, status, and Markdown structure.
8. **Review privacy before any public export.** Do not expose company secrets, credentials, or raw sensitive data.

## Example

Before:

```yaml
schema_version: 2
id: "d362e6af-1945-4ec5-8bb3-50bb8b589d29"
decided_by: user
user_review: reviewed
```

After reading the underlying source, you may add:

```yaml
schema_version: 3
id: "d362e6af-1945-4ec5-8bb3-50bb8b589d29"
decided_by: user
user_review: reviewed
contributions:
  hypothesis_generation:
    lead: ai
    human_actions: []
    independent_judgment: not_observed
    evidence: [E2]
  result_interpretation:
    lead: joint
    human_actions: [challenged]
    independent_judgment: demonstrated
    evidence: [E3, E4]
  final_decision:
    lead: user
    human_actions: [selected, reviewed]
    independent_judgment: not_observed
    evidence: [E5]
```

This example assumes E3/E4 show a new, reasoned human objection rather than a repetition of AI's critique. If they do not, set `independent_judgment: not_observed` or `unknown` as appropriate.

**Never add those fields using only `decided_by: user`.** Decision authority and independent reasoning are different claims.

## Validation checklist

- [ ] The case still describes **one actual decision** rather than an invented general success story.
- [ ] Each recorded phase has an explicit `lead`, `human_actions`, `independent_judgment`, and an evidence reference.
- [ ] Every `[E#]` reference exists in the body and points to a genuinely available source.
- [ ] `decided_by` still refers to the final choice, not the person who wrote the code.
- [ ] `user_review: reviewed` is supported by explicit examination, not silence.
- [ ] `independent_judgment: demonstrated` is supported by specific novel human reasoning.
- [ ] Missing phases are **not** interpreted as absent human contributions or as AI-led work.
- [ ] Assumptions, retrospective explanations, unmeasured outcomes, and unverified ability are clearly labeled.
- [ ] The original stable ID, dates, case link, and human-edited content are preserved.
- [ ] No credentials, private business records, or identifying user data are disclosed.

## Sample-based rollout

Migrate three to five diverse cases first: one user-led, one AI-led, one genuinely joint, and ideally one with unknown review. Compare the classification with the original transcripts. Only then consider a larger migration. Report disagreement rates and unknowns rather than forcing classification.
