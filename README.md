# Designing More Reliable AI-Generated React Interfaces

A creative-technology experiment that turns design intent into reusable AI instructions for more consistent, controllable and review-ready React + TypeScript interface generation.

> **At a glance**
> - **Problem:** AI generates UI fast, but each generation invents its own styling, data patterns and accessibility choices, so every component becomes an island that people have to reconcile by hand.
> - **What I designed:** a layered instruction architecture (shared rules, a design-token layer, generation modes that make room for art direction, anti-pattern guardrails and a self-audit step) that sits around an AI coding agent.
> - **How I tested it:** the same brief, the same editor and the same one-shot budget, run in a clean baseline project and in a guided project. Two components so far.
> - **What changed:** the guided output reused a shared token system, a layered file structure and runtime validation without being asked. The baseline hardcoded its own values and duplicated its theming in every component.
> - **What this shows:** designing the constraints, the comparison method and the feedback loop around AI-generated UI, not just prompting it.
>
> *A structured showcase evaluation, not a statistically generalisable benchmark.*

---

## The challenge

AI coding agents can produce a working interface in minutes. The trouble starts at the second, fifth and twentieth component. Without shared direction, each generation makes its own decisions about colour values, spacing, file structure, data fetching, theming and accessibility. Each output can be competent on its own and still be inconsistent with the one before it.

That's a **workflow quality problem**, not a claim that AI output is bad. The cost shows up later: in review, in reconciling styles, and in cleanup before a designer or engineer can sign anything off.

## The hypothesis

> A structured instruction architecture, covering component composition, design tokens, UI states, anti-pattern prevention and generation modes, can make AI-generated React UI more consistent and closer to review-ready than a baseline workflow given the same brief.

## What I designed

The architecture treats generation as one step in a loop, not the end of the process:

```mermaid
flowchart LR
    A[Creative brief] --> B[Reusable instruction architecture]
    B --> C[Project-specific art direction<br/>and constraints]
    C --> D[AI coding agent]
    D --> E[Generated interface]
    E --> F[Human creative +<br/>technical review]
    F --> G[Evaluation rubric]
    G -->|observations| H[Updated instructions]
    H --> B
```

| Layer | Role | Design decision behind it |
|---|---|---|
| **Principles** (`.cursorrules`) | What the agent must always do: structure, types, semantics, accessibility floor | Rules describe *how* to build, never *what it should look like* |
| **Values** (design tokens) | One source of truth for colour, spacing, type, radius and motion | Change one variable and every component follows |
| **Domain guides** (private) | In-depth patterns for components, data, UX states, accessibility, layout and testing | Loaded only when the task needs them |
| **Generation modes** | Template / Balanced / Design-First, with an explicit order of precedence | **Consistency must not become visual sameness.** A provided design overrides the defaults |
| **Setup flows** | A guided palette setup with contrast checks and a human approval gate | Creative input is asked for, not assumed |
| **Guardrails and self-audit** | Anti-pattern checks the agent reports on before fixing anything | Problems surface for review instead of being silently patched |

More detail in [`instructions/architecture.md`](./instructions/architecture.md).

## What I tested

Each comparison follows the same protocol:

- The same written brief, pasted verbatim into both projects
- The same editor (Cursor, Agent mode) and the same model setting
- **Baseline:** a clean Vite + React + TypeScript scaffold with no instructions
- **Guided:** the same scaffold plus the instruction architecture (v1.0)
- The same budget: one prompt, no follow-ups, no manual edits
- Review of the output against a fixed set of areas: data handling, structure, styling, theming, accessibility and error recovery

Two scenarios have been run: a **UserProfileCard** (variants, data, states) and a **NotificationCenter** (polling, focus management, responsive dropdown). Full method, briefs and caveats are in [`comparison.md`](./comparison.md).

## Observed differences

Observed tendencies from two single-run comparisons. The evidence is the written code review in `comparison.md`; source code and screenshots are still to be added (see [`examples/`](./examples/)).

| Area | Baseline tendency | Guided tendency | Evidence |
|---|---|---|---|
| Design-system alignment | Hardcoded hex/px values; component-local CSS variables | Every value referenced a global token | [Test 1 & 2 styling](./comparison.md#test-1-userprofilecard) |
| Theming | Dark mode rebuilt per component (80+ lines of overrides in Test 2) | No theme code in the component; inherited from tokens | [Test 2](./comparison.md#test-2-notificationcenter) |
| Component composition | Fetching, state and UI co-located | Separated into schema → API → hook → component | [Test 2 file tree](./comparison.md#test-2-notificationcenter) |
| Data and error handling | Response type-cast without validation; no retry | Runtime schema validation; retry action | [Both tests](./comparison.md) |
| Accessibility fundamentals | Solid basics (`aria-expanded`, live region, focus trap) | Same basics plus focus return, dialog semantics, token-based focus ring and 44px targets | Attribute review only; no automated audit yet |
| Styling volume | ~350 lines of CSS (Test 2) | ~220 lines of CSS (Test 2) | One run; indicative only |
| Visual consistency across components | — | — | **Not yet evidenced.** Screenshots to add |
| Manual cleanup before review | — | — | **Not measured** |

**Reading these fairly:** some guided choices (React Query, Zod, the folder layout) were *specified* by the instructions and never requested in the baseline. They show that the agent follows a shared system without being re-prompted, not that it independently makes better engineering calls. The rows that matter most for the hypothesis are token alignment, theming and composition.

## Beyond a general-purpose frontend skill

General-purpose frontend skills, such as Anthropic's published `frontend-design` skill, help an AI agent make more intentional visual decisions and avoid generic defaults. This project focuses on a **complementary layer**: how creative and technical teams can structure, test and evaluate AI-assisted UI generation as a repeatable workflow.

Rather than treating generation as the end of the process, the architecture connects creative briefs, reusable constraints, project-specific art direction, structured comparison, human review and iteration. The two approaches can be combined: a frontend skill can be one input inside this workflow. That combination hasn't been tested here yet, and nothing in this repository compares this project's performance against any skill.

| Dimension | General-purpose frontend skill | This project's focus |
|---|---|---|
| Primary purpose | Guides the agent's design approach during an individual frontend task | Makes AI-assisted UI production more controllable, reviewable and improvable across components |
| Visual direction | Primary focus: intentional aesthetic choices grounded in the brief, avoiding templated defaults | Connects visual direction to generation modes that decide which values stay fixed and which remain open to creative interpretation |
| Reusable UI-system constraints | Typically proposes a token plan per brief | A persistent, shared token layer and component rules reused by every generation in a project |
| Loading, empty and error states | Guidance depends on the skill; the published `frontend-design` skill covers how to write for them | Part of the generation rules and the review criteria |
| Baseline vs. guided comparison | Outside a skill's scope | Central method of this case study |
| Failure modes and iteration | In-task self-critique | Versioned changes to the instructions themselves, documented across tests |
| Human review | Supports the agent's own plan review | Explicit human gates (palette approval, audit-before-fix) plus human creative and technical review |
| Tool scope | Packaged as an Agent Skill | Principles documented in tool-neutral form; the artifact is Cursor-format and tested in Cursor only |

## Iterations and learning

The instructions are treated as a living system. Three documented steps, from the repository history:

1. **v1.0 → tested (9 Feb 2026).** The two comparisons above ran against v1.0.
2. **Formalised what the tests showed (13 Feb).** A build order (types → schema → API → hook → component) and a read-only self-audit protocol were added as explicit rules.
3. **Separated creative input from structural rules (v1.1, 13 Feb).** A palette setup flow with three input modes (full palette, seed colours or an image, mood description), contrast checks and an approval gate replaced silent default colours.

**Open learning:** v1.1 hasn't been re-tested against the same briefs. Until it is, its effect is a design intention, not a result. See [`docs/iterations.md`](./docs/iterations.md).

## Limitations

- Instructions raise the quality floor. They don't replace creative direction, engineering review or accessibility validation.
- Two scenarios, one run each. Not statistically generalisable.
- Cursor's "Auto" model setting doesn't guarantee the same model served both conditions. This is the main confound.
- No automated checks (type-check, lint, axe) have been run on the outputs yet.
- Model behaviour changes over time, so the instructions need re-evaluating.
- Outcomes depend on the brief, the model, references and human review.
- Some implementation details are intentionally omitted to preserve the project's commercial and competitive value.

## Public vs. intentionally omitted

| Public | Omitted |
|---|---|
| The core instruction file (`.cursorrules`, v1.1) | The domain guides, including the full anti-pattern rule set |
| Architecture, modes and the feedback loop | The production token file and setup scripts |
| Test briefs, method, observations and limitations | Private template repository |

A walkthrough of the private template is available on request via [LinkedIn](https://www.linkedin.com/in/niklazhallberg/).

## Repository map

| Path | What it is |
|---|---|
| `README.md` | Portfolio overview and summary of the evaluation |
| [`comparison.md`](./comparison.md) | Detailed method, briefs, findings and threats to validity |
| [`.cursorrules`](./.cursorrules) | Representative Cursor-compatible instruction artifact (v1.1) |
| [`instructions/`](./instructions/) | Tool-neutral description of the instruction architecture |
| [`examples/`](./examples/) | Scenario briefs, the evidence protocol and baseline/guided evidence slots |
| [`docs/iterations.md`](./docs/iterations.md) | What changed between versions, and why |
| [`CHANGELOG.md`](./CHANGELOG.md) | Version history of the architecture and this case study |

## Author

Niklaz Hallberg is a Creative Technologist working across generative AI, creative tools, interactive media and AI-assisted production workflows. This repository is a public case study in making generative frontend workflows more controllable, inspectable and useful in real creative production.

[niklaz.works](https://www.niklaz.works) · [LinkedIn](https://www.linkedin.com/in/niklazhallberg/)
