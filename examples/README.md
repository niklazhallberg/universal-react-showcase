# Examples and Evidence

Each scenario folder holds a brief, its generation conditions, what was observed, and the evidence planned for it. Nothing here is illustrative or reconstructed. Where an artifact isn't in the repository yet, it's listed in the roadmap below with what it will validate.

| # | Scenario | What it exercises | Version | Status |
|---|---|---|---|---|
| 01 | [UserProfileCard](./01-user-profile-card/) | Variants, data states, a toggle interaction | v1.0 | Run; written review published |
| 02 | [NotificationCenter](./02-notification-center/) | Polling, focus management, responsive behaviour | v1.0 | Run; written review published |
| 03 | [Campaign landing page](./03-campaign-landing-page/) | Art direction, typography, hierarchy, mood | v1.1 | Planned evaluation |

## Evidence roadmap

| Evidence | Scenario | Version | Status | What it will validate |
|---|---|---|---|---|
| Unedited generated source, baseline and guided | 01, 02 | v1.0 | Evidence pending | The written observations in `comparison.md` |
| Side-by-side screenshots of states and themes | 01, 02 | v1.0 | Evidence pending | Visual consistency and state coverage |
| Keyboard-only screen recording | 02 | v1.0 | Evidence pending | Focus trap, focus return, dialog behaviour |
| Automated checks (type-check, lint, axe) | 01, 02 | v1.0 | Evidence pending | Accessibility and code-quality claims |
| Rerun, pinned model, 3 runs per condition | 01, 02 | v1.1 | Planned evaluation artifact | Whether v1.1 keeps v1.0's structural consistency |
| Art-direction comparison, baseline / Template / Balanced | 03 | v1.1 | Planned evaluation artifact | Controlled visual variation after the Color Setup Flow |

## Evidence protocol

Each run records:

- **Conditions:** date, editor and version, mode, the exact model (not "Auto"), and the instruction version (git hash)
- **Brief:** verbatim
- **Output source:** the complete generated files, unedited, under `baseline/` and `guided/`
- **Screenshots:** desktop and mobile, light and dark, plus each UI state (loading, empty, error, populated)
- **Automated checks:** `tsc --noEmit`, eslint, axe (violation count by severity)
- **Keyboard pass:** tab order, focus visibility, focus return
- **Time and cleanup:** minutes and number of edits needed to reach "ready for review"
- **Notes:** surprises, failures and anything the rubric didn't capture

## Evaluation rubric

For v1.1 runs onward, fixed before any output is reviewed. Each area is scored **0 = absent, 1 = partial, 2 = consistent**, with a one-line justification.

| Area | 2 looks like |
|---|---|
| Design-system alignment | All visual values come from tokens; no hardcoded colour or spacing |
| Component composition | Clear boundaries; presentation separate from data; no god components |
| UI state coverage | Loading, empty, error and success all handled, with recovery where relevant |
| Accessibility fundamentals | Semantic elements, labelled controls, visible focus, zero serious axe violations |
| Theming | Works in both themes without theme code inside the component |
| Art-direction fidelity *(visual briefs)* | Follows the brief's stated mood and hierarchy; not a generic default |
| Visual distinctiveness *(visual briefs)* | Distinct from the other scenarios' output under the same rules |
| Review readiness | A reviewer could start on design and logic rather than on cleanup |

Scores will be reported as observed tendencies from a small number of runs, not benchmark numbers.
