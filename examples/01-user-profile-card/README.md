# 01 · UserProfileCard

**Status:** run against instruction v1.0 (published 2026-02-09). The written review is in [`comparison.md`](../../comparison.md#test-1-userprofilecard). Source code and screenshots haven't been added yet.

## Brief

> Build a UserProfileCard component in React + TypeScript.
>
> Requirements:
> - Three visual variants: compact, default, featured
> - Fetch user data from https://jsonplaceholder.typicode.com/users/1
> - Show loading/error/empty states
> - A Follow/Following toggle button
> - Responsive and accessible
> - Well-structured file organization

## Conditions

| | Baseline | Guided |
|---|---|---|
| Project | Clean Vite + React + TS | The same, plus instructions v1.0 |
| Editor | Cursor, Agent mode | Cursor, Agent mode |
| Model | "Auto" *(exact model not recorded)* | "Auto" *(exact model not recorded)* |
| Budget | One prompt, no follow-ups | One prompt, no follow-ups |

## Outcome summary

- **Baseline:** fetched data in an effect with manual state and a type cast; component-local hardcoded CSS variables; its own dark palette; an error message with no retry.
- **Guided:** a query library plus schema validation; layered files; global tokens; inherited theming; token-based focus ring; reduced-motion support; retry.
- **Both:** attempted all three states (the brief asked for them) and produced reasonable ARIA.

## Limitations

One run per condition; model not pinned; reviewed by the author; no automated checks.

## Evidence to add

- [ ] `baseline/`: generated source, unedited
- [ ] `guided/`: generated source, unedited
- [ ] Screenshots: the three variants × loading / error / populated, light and dark
- [ ] Exact model used in each condition (Cursor chat history)
- [ ] axe results for both
