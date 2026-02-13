# Universal React Template

I built an instruction architecture that controls how AI coding assistants generate React + TypeScript code. Instead of prompting and hoping for the best, I created a system of rules, tokens, and documentation that ensures consistent, high-quality UI / UX — following the same patterns regardless of which AI model generates the code.

## The Problem

As a digital designer who works extensively with generative AI for image and video production, I've learned that the best outputs come from well-designed constraints, not just better prompts. My hypothesis was that the same applies to code generation. AI assistants like Cursor are powerful and versatile, but if there's one thing I've learned, it's that AI systems are likely to drift and become inconsistent over time if you don't give them the right directives and rules to follow.

In the context of React JS, ask for 20 components and you'll probably get 20 different approaches to styling, data fetching, state management, and accessibility. Without explicit constraints, the AI doesn't know what you want. It guesses to the best of its ability.

This raised a question: how do you design a framework that consistently produces high-quality code — building exceptional UI/UX components, sections, and websites — without being overly rigid? A system that balances strict architectural standards with the flexibility to adapt to different needs and contexts. That was the challenge I set out to solve.

## The Result: 37% Less Code. Significantly Better Architecture.

I ran A/B tests: same prompt, same IDE (Cursor), same model (Auto), no follow-up prompts. One project had my instruction architecture. The other was a default clean Vite scaffold.

The system-guided project produced a NotificationCenter component with **220 lines of CSS** compared to **350+ lines** in the unconstrained version, and the shorter version had better dark mode support, token-based consistency, and comprehensive ARIA coverage. Less code but more capability. See details in the table below.

| Aspect | Without My System | With My System |
|---|---|---|
| API validation | `as Type` cast | Zod schema + `unknown` |
| Data fetching | useEffect + fetch + manual state | React Query |
| Polling (30s refresh) | setInterval + AbortController | `refetchInterval` (one line) |
| File structure | Everything co-located | schema → api → hook → component |
| Styling | Hardcoded hex/px/rgba | Design tokens throughout |
| Dark mode | Per-component, duplicated CSS | Automatic via tokens.css |
| CSS volume | ~350 lines | ~220 lines (37% less) |
| Focus rings | Hardcoded color/width | Token-based, consistent |
| ARIA completeness | Good basics | Comprehensive (dialog, feed, modal, haspopup) |
| Touch targets | Not specified | 44px minimum (WCAG 2.5.5) |
| Error recovery | No retry | Retry via React Query |
| Comments | English, minimal | Swedish JSDoc, architectural |

Same input. Vastly different output. Full breakdown with code examples in [comparison.md](./comparison.md).

## How I Built It

I built this system in collaboration with Claude Opus 4.6, Perplexity AI for deep research, and hands-on exploration across developer forums and documentation.

The system has three layers, each solving a different problem:

```
.cursorrules              <- Principles: what the AI must always do
src/styles/tokens.css     <- Values: the design system source of truth
docs/                     <- Guides: deep patterns for specific domains
```

On top of that, I wrote 15 documentation files that the AI reads before generating any code. Together they cover everything from component architecture to accessibility compliance:

| File | What It Covers |
|---|---|
| `.cursorrules` | Core rules, Color Setup Flow, State Decision Tree, Complexity Ladder |
| `CLAUDE.md` | Entry point for Claude-based tools |
| `docs/design-system.md` | Tokens, typography, colors, spacing (8px grid), Color Setup reference |
| `docs/component-patterns.md` | Component structure, composition, discriminated unions |
| `docs/data-fetching.md` | React Query, Zod validation, API layer patterns |
| `docs/accessibility.md` | WCAG 2.1 AA compliance, ARIA, keyboard navigation |
| `docs/anti-patterns.md` | 25 enforced rules against common AI mistakes |
| `docs/ux-patterns.md` | Loading, error, empty, success states |
| `docs/styling.md` | CSS Modules, token usage, dark mode, responsive |
| `docs/layout-grids.md` | Grid system, responsive layout, container queries |
| `docs/performance.md` | Code splitting, virtualization, image optimization |
| `docs/design-first.md` | Three implementation modes |
| `docs/implementation-checklist.md` | Setup and verification steps |
| `docs/routing.md` | Route structure, protected routes, code splitting |
| `docs/testing.md` | Vitest + React Testing Library + jest-axe |

### 25 Anti-Pattern Rules

To learn from the best and avoid common pitfalls, I did extensive research — studying what senior React and TypeScript developers wish they'd known earlier and the most common mistakes they encounter. I also investigated the patterns AI assistants get wrong most often, and distilled everything into 25 hard constraints. When the AI detects any of these, it's instructed to stop and propose the correct approach:

- State modeling mistakes (derived state, boolean explosion, direct mutation, prop mirroring)
- Effect pitfalls (useEffect for transformation, missing deps, stale closures)
- Architecture issues (god components, prop drilling, nested components, index keys)
- TypeScript safety (`any`, untyped event handlers)
- Data fetching errors (static queryKeys, inline fetch, unvalidated responses, conditional hooks)
- Accessibility violations (div as button, missing labels)
- Styling mistakes (hardcoded values, inline styles, removed focus outlines, layout animations)

### Design Token System

Every visual value lives in one file — `tokens.css`. The file ships with placeholder colors that get replaced through a guided **Color Setup Flow** on first use. The AI asks how you want to define your palette:

| Mode | You provide | AI handles |
|---|---|---|
| **Full Palette** | All 6 colors | Variants, neutrals, WCAG validation |
| **Seed Colors** | 1–3 colors or an image | Completes palette using color theory, validates WCAG |
| **AI Picks** | A mood or industry description | Full palette generation, WCAG validation |

Every color combination is validated against WCAG 2.1 AA before being written to `tokens.css` — including dark mode. No component ships with unchecked contrast.

Beyond colors, the token system includes an 8px spacing grid, modular typography scale, and semantic naming throughout. Change one variable, every component updates.

### What's Not Included (Yet)

The Color Setup Flow currently covers palette configuration only. In a production environment, the same guided approach could extend to:

- **Typography** — font selection with the same three-mode pattern (provide fonts / seed a mood / let AI choose)
- **Spacing scale** — 8px grid is the default, but a 4px base works better for dense UIs like dashboards
- **Border radius personality** — a single question ("Sharp / Soft / Rounded?") changes the entire feel of the system

These were intentionally left out to keep the scope of this test focused. The architecture supports adding them as setup flows for Template and Balanced mode.

## What's In This Repo

This is the **public showcase** of the project. It includes:

- **README.md** — This overview
- **comparison.md** — Full A/B test results with code examples from both projects
- **.cursorrules** — The core instruction file that drives AI behavior

The complete template with all 15 documentation files, design tokens, Zod schemas, and React Query setup is available in a private repository. If you'd like to see a live demo or discuss the architecture, reach out via [LinkedIn](https://www.linkedin.com/in/niklazhallberg/).

## Stack

- React 18 + TypeScript (strict)
- Vite
- CSS Modules + design tokens
- React Query (@tanstack/react-query)
- Zod (runtime validation)

## How It Works In Practice

1. Open the project in Cursor (or any AI-assisted IDE)
2. The AI reads `.cursorrules` automatically
3. On first use, the Color Setup Flow configures your project palette with WCAG validation
4. `.cursorrules` tells the AI to read the relevant `docs/` files
5. Every component generated follows the same architecture — TypeScript interfaces, CSS Modules with tokens, semantic HTML, React Query, Zod validation, and explicit loading/error/empty states

I don't have to ask for any of this. The system ensures it happens automatically.

## Three Modes

I designed three implementation modes depending on the context:

| Mode | When | Color handling | Approach |
|---|---|---|---|
| **Template** (default) | No design provided | Color Setup Flow (required) | Strict tokens, 8px grid |
| **Balanced** | Design provided | Seed Colors from design | Structure from system, character from design |
| **Design-First** | Pixel-perfect spec | Colors from design spec | Exact values, system structure |

## What I Learned

This was a small-scale test. But even with a relatively simple challenge, the results showed that systematic, holistic guidance produces measurably better output. Anthropic describes this shift in their [2026 Agentic Coding Trends Report](https://claude.com/blog/eight-trends-defining-how-software-gets-built-in-2026) — engineers moving from writing code to orchestrating agents, focusing on architecture and constraints while AI handles implementation. That's exactly what this project explores.

The real leverage in AI-assisted development isn't in writing better prompts, it's in building better constraints. The model doesn't need to be smarter, it needs clear boundaries. Consistency at scale is what separates a collection of components from a maintainable product.

---

*Built by Niklaz Hallberg [ https://www.niklaz.works ], February 2026.*
