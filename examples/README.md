# Examples and Evidence

Each scenario folder holds a brief, the generation conditions, and slots for baseline and guided evidence. **An empty slot is labelled as empty.** Nothing here is illustrative or reconstructed.

| # | Scenario | What it exercises | Status |
|---|---|---|---|
| 01 | [UserProfileCard](./01-user-profile-card/) | Variants, data states, a toggle interaction | Run (v1.0). Written review only; artifacts to add |
| 02 | [NotificationCenter](./02-notification-center/) | Polling, focus management, responsive behaviour | Run (v1.0). Written review only; artifacts to add |
| 03 | [Campaign landing page](./03-campaign-landing-page/) | Art direction, typography, hierarchy, mood | **Planned** |

---

## Evidence protocol

For every run, capture:

- [ ] **Conditions:** date, editor and version, mode, the exact model (not "Auto"), and the instruction version (git hash)
- [ ] **Brief:** pasted verbatim
- [ ] **Output source:** the complete generated files, unedited, committed under `baseline/` and `guided/`
- [ ] **Screenshots:** desktop and mobile, light and dark, plus each UI state (loading, empty, error, populated)
- [ ] **Automated checks:** `tsc --noEmit`, eslint, axe (violation count by severity)
- [ ] **Keyboard pass:** a short note on tab order, focus visibility and focus return
- [ ] **Time and cleanup:** minutes and number of edits needed to reach "ready for review"
- [ ] **Notes:** surprises, failures and anything the rubric didn't capture

## Evaluation rubric

Defined before reviewing the output. Each area is scored **0 = absent, 1 = partial, 2 = consistent**, with a one-line justification.

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

Scores are observed tendencies from a small number of runs, not benchmark numbers.
