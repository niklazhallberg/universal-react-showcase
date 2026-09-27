# Instruction Architecture (tool-neutral)

This document describes the *design* of the instruction system independent of any single AI coding tool. The public [`.cursorrules`](../.cursorrules) file is one concrete, Cursor-format expression of it. The same structure maps onto `CLAUDE.md`, `AGENTS.md`, agent skills or other rule files.

It explains how the layers relate and why they're separated. It deliberately doesn't reproduce the private domain guides or rule sets.

**Version key:** parts marked *v1.0* were in place during the [documented tests](../comparison.md). Parts marked *added after v1.0 testing* date from 2026-02-13 and are untested.

---

## 1. Separate the stable from the variable *(v1.0)*

The central design decision: **principles are strict, values are flexible.**

| Stable (the same in every project) | Variable (set per project or brief) |
|---|---|
| The token-based approach | Token values: palette, type scale, radius, motion |
| Accessibility floor | Visual style and tone |
| Type safety and component structure | Component variants |
| Semantic HTML | Spacing density |
| Performance guidelines | Art direction |

That split is intended to allow consistency without visual sameness: the agent is instructed to build the same *way* every time, while the *look* is decided per project. Whether it achieves this is what [scenario 03](../examples/03-campaign-landing-page/) is planned to test.

## 2. Layers *(v1.0)*

```
┌───────────────────────────────────────────────┐
│ Core rules          what must always happen   │  ← short, always loaded
├───────────────────────────────────────────────┤
│ Project overrides   style, tone, fidelity     │  ← filled in per project
├───────────────────────────────────────────────┤
│ Design tokens       every visual value        │  ← single source of truth
├───────────────────────────────────────────────┤
│ Domain guides       deep patterns per topic   │  ← loaded on demand
└───────────────────────────────────────────────┘
```

- **Core rules** stay short enough to be read every time. They point to deeper material instead of containing it.
- **Project overrides** is a structured template (style, tone, corners, motion, dos and don'ts) where a creative lead captures art direction in words the agent can act on.
- **Design tokens** hold values, never rules. Components reference tokens, so a change of direction is one edit rather than a refactor.
- **Domain guides** cover components, data, UX states, accessibility, layout, performance, routing, testing and anti-patterns. *(Not public.)*

## 3. Generation modes and precedence *(v1.0)*

Consistency rules become a creative liability when a real design exists. Three modes make the trade-off explicit:

| Mode | When | What the system controls | What the design controls |
|---|---|---|---|
| **Template** | No design provided | Structure and all visual values (via tokens) | — |
| **Balanced** | Design provided as a reference | Structure, accessibility, tokens where sensible | Character, palette, feel |
| **Design-First** | Pixel-perfect spec | Structure, accessibility, performance | Exact visual values |

Order of precedence: explicit user instruction → provided design → project overrides → universal rules. Accessibility and semantic HTML are never overridden, because they don't change the visual result.

## 4. Guardrails

- *(v1.0)* An anti-pattern set covering state, effects, architecture, typing, data fetching, accessibility and styling, treated as hard constraints. *(Full rule set not public.)*
- *(added after v1.0 testing)* An explicit build order: types → schema → API → hook → component.
- *(added after v1.0 testing)* A read-only self-audit: after a feature, the agent reports pass/fail per anti-pattern rule before fixing anything, so problems reach the human reviewer instead of being silently patched.

## 5. Setup flows: asking instead of assuming *(v1.1, added after v1.0 testing)*

Instead of shipping default colours that quietly end up in production, the Color Setup Flow asks for creative input the first time the agent generates something:

1. **Choose an input mode:** a full palette, one to three seed colours (or an image), or a mood/industry description.
2. **The agent completes the palette** and checks contrast in both themes.
3. **A human approves** before anything is written.
4. **Only values change**; token names and structure stay fixed.

The same pattern is designed to extend to typography, spacing density and corner "personality". Those extensions haven't been built.

## 6. The feedback loop

The instructions are versioned and changed on the basis of observed output:

```
brief → generate (baseline + guided) → review against rubric
      → note tendencies and failure modes → change instructions → re-test
```

The architecture is intended as a complementary layer to general-purpose agent guidance, such as frontend skills, not a replacement for it (see the [README](../README.md#beyond-a-general-purpose-frontend-skill)). What has changed so far, and what is untested, is in [`docs/iterations.md`](../docs/iterations.md).
