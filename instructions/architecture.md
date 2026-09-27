# Instruction Architecture (tool-neutral)

This document describes the *design* of the instruction system independent of any single AI coding tool. The public [`.cursorrules`](../.cursorrules) file is one concrete, Cursor-format expression of it. The same structure maps onto `CLAUDE.md`, `AGENTS.md`, agent skills or other rule files.

It explains how the layers relate and why they're separated. It deliberately doesn't reproduce the private domain guides or rule sets.

---

## 1. Separate the stable from the variable

The central design decision: **principles are strict, values are flexible.**

| Stable (the same in every project) | Variable (set per project or brief) |
|---|---|
| The token-based approach | Token values: palette, type scale, radius, motion |
| Accessibility floor | Visual style and tone |
| Type safety and component structure | Component variants |
| Semantic HTML | Spacing density |
| Performance guidelines | Art direction |

That split is what makes consistency possible without visual sameness: the agent always builds the same *way*, but the *look* is decided per project.

## 2. Layers

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

## 3. Generation modes and precedence

Consistency rules become a creative liability when a real design exists. Three modes make the trade-off explicit:

| Mode | When | What the system controls | What the design controls |
|---|---|---|---|
| **Template** | No design provided | Structure and all visual values (via tokens) | — |
| **Balanced** | Design provided as a reference | Structure, accessibility, tokens where sensible | Character, palette, feel |
| **Design-First** | Pixel-perfect spec | Structure, accessibility, performance | Exact visual values |

Order of precedence: explicit user instruction → provided design → project overrides → universal rules.

Accessibility and semantic HTML are never overridden, because they don't change the visual result.

## 4. Setup flows: asking instead of assuming

Rather than shipping default colours that quietly end up in production, a setup flow asks for creative input the first time the agent generates something:

1. **Choose an input mode:** a full palette, one to three seed colours (or an image), or a mood/industry description.
2. **The agent completes and checks** the palette against contrast requirements, in both themes.
3. **A human approves** before anything is written.
4. **Only values change**; token names and structure stay fixed.

The same pattern is designed to extend to typography, spacing density and corner "personality". Those extensions haven't been built yet.

## 5. Guardrails and self-audit

- A build order that makes dependencies explicit: types → schema → API → hook → component.
- A set of anti-patterns covering state, effects, architecture, typing, data fetching, accessibility and styling. *(Full rule set not public.)*
- After a feature, the agent audits its changes against those anti-patterns **read-only** and reports pass/fail per rule before fixing anything. This keeps problems visible to the human reviewer instead of silently patched.

## 6. The feedback loop

The instructions are versioned and changed on the basis of observed output, not intuition alone:

```
brief → generate (baseline + guided) → review against rubric
      → note tendencies and failure modes → change instructions → re-test
```

See [`docs/iterations.md`](../docs/iterations.md) for what has changed so far and what is still untested.

## 7. Where it sits alongside other tools

This architecture is a **complementary layer** to general-purpose agent guidance such as frontend-design skills. A skill can shape how the agent approaches an individual task; this architecture defines the project-level constraints, modes and review loop around that task. Combining the two is a planned experiment, not a tested result.
