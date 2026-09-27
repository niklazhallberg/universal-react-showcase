# Iterations and Learning

The instruction architecture is treated as a living system. This log uses only what is recorded in the repository history and the published tests. Where the reasoning behind a change hasn't been written down, it says so rather than reconstructing it after the fact.

---

## Timeline

| Date | Version | Change | Tested? |
|---|---|---|---|
| 2026-02-04 | v1.0 | Universal template: principle-based rules, project overrides, three generation modes, order of precedence, anti-pattern guardrails | — |
| 2026-02-09 | v1.0 | Two baseline vs. guided comparisons published (UserProfileCard, NotificationCenter) | **Yes: all documented results** |
| 2026-02-13 | v1.0 → v1.1 | Build order and read-only self-audit protocol added (committed under the v1.0 header; not listed in the file's own version history) | No |
| 2026-02-13 | v1.1 | Color Setup Flow: three input modes, contrast checks, approval gate | No |

**Documented results in this repository relate to v1.0.** The Color Setup Flow was introduced in v1.1 after the evaluation and hasn't yet been tested under the same conditions.

---

## Iteration 1: making the observed structure explicit

**Observation**
In both tests, the guided output separated validation, data access, state and presentation into layers (schema → API → hook → component). The v1.0 core rules didn't state this sequence explicitly.

**Change**
A `BUILD ORDER` section and a `REVIEW PROTOCOL` section were added to the core rules. The review protocol has the agent audit new or modified files against the anti-pattern set, report pass/fail per rule, and change nothing until the report is done.

**Why**
*[Author to confirm: the reasoning for promoting these from the guides into the core rules.]*

**Outcome**
Not yet evaluated. The v1.0 tests predate this change.

---

## Iteration 2: separating structural consistency from art direction

**Observation**
In v1.0, the token file shipped with placeholder colours, and in Template mode every component used tokens exclusively. Nothing in the rules asked for creative input on the palette, so placeholder values could flow straight into generated UI.
*[Author to add: what was actually observed in use that prompted this change.]*

**Change**
v1.1 added a palette setup flow that runs on first generation:
- Three input modes: full palette, seed colours or an image, or a mood description
- Contrast checks in both light and dark themes, with adjustments reported
- Explicit human approval before any value is written
- Only colour values change; token names and structure stay fixed

**Why**
The structural rules (tokens, composition, accessibility) should stay stable, while the visual identity is decided per project. The flow makes that boundary operational: creative input is asked for rather than assumed.

**Outcome**
**Untested.** The published comparisons ran against v1.0, before this flow existed. Its effect on visual variety and on the correctness of the contrast checks is a design intention until it's re-tested.

---

## Known trade-offs and failure modes

- **Consistency vs. sameness.** Strict token use in Template mode produces uniform output by design. Without a filled-in project overrides section or a provided design, projects risk looking alike. Balanced and Design-First modes exist to counter this, but haven't been compared side by side.
- **Instructions vs. independent judgement.** Part of the guided "improvement" comes from specifying the stack. That shows consistency, not better unprompted decisions.
- **A contrast check done by the model is still a claim.** The setup flow tells the agent to check contrast. Without an external contrast tool in the loop, the result needs human verification.
- **Interactive flows vs. one-shot tests.** The setup flow needs a human in the loop, which the one-prompt test protocol doesn't allow. Future tests need to run the setup first and log it as part of the conditions.
- **Public vs. private.** The core rules file was removed from the public repository and restored four days later. Showing the thinking without exposing the domain guides is an ongoing trade-off, and the reason the domain guides stay private.

## Next evaluation (planned)

Rerun the same scenarios with v1.1 to assess whether the Color Setup Flow increases art-direction flexibility without reducing structural consistency. Planned protocol changes:

1. Pin the model instead of using "Auto", and record the model version.
2. Re-run Tests 1 and 2 against v1.1, with at least three runs per condition.
3. Add an art-direction-led scenario ([`examples/03`](../examples/03-campaign-landing-page/)) to test consistency vs. sameness directly.
4. Add automated checks (type-check, lint, axe) to the rubric.
5. Test the architecture with a general-purpose frontend skill enabled, as a complementary layer.
