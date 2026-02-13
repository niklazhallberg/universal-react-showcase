# Universal React Template

I built an instruction architecture that controls how AI coding assistants generate React + TypeScript code. Instead of prompting and hoping for the best, I created a system of rules, tokens, and documentation that ensures every component follows the same patterns, regardless of which AI model generates it.

## The Problem

As a digital designer who works extensively with generative AI for image and video production, I've learned that the best outputs come from well-designed constraints, not better prompts. My hypothesis was that the same applies to code generation. AI assistants like Cursor are super powerful and versatile, but if there's one thing I've learned, it's that AI systems are likely to drift over time if you don't give them the right directives and rules.

Ask for 20 components and you'll probably get 20 different approaches to styling, data fetching, state management, and accessibility. Without explicit constraints, the AI doesn't know what you want. It guesses to the best of its ability.

This raised a question: how do you design a framework that consistently produces high-quality UI/UX code — without being overly rigid? A system that balances strict architectural standards with the flexibility to adapt to different needs and contexts.

## The Result: 37% Less Code. Significantly Better Architecture.

I ran A/B tests: same prompt, same IDE (Cursor), same model (Cursor: Auto), no follow-up prompts. One project had my instruction architecture. The other was a default clean Vite scaffold.

The system-guided project produced a NotificationCenter component with **220 lines of CSS** compared to **350+ lines** in the unconstrained version, and the shorter version had better dark mode support, token-based consistency, and comprehensive ARIA coverage. Less code but more capability.

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

Same input. Different output. Full breakdown with code examples in [comparison.md](./comparison.md).

## How I Built It

I built this system in collaboration with Claude Opus 4.6.

It has three layers, each solving a different problem:

```
.cursorrules              <- Principles: what the AI must always do
src/styles/tokens.css     <- Values: the design system source of truth
docs/                     <- Guides: deep patterns for specific domains
```

On top of that, I wrote 15 documentation files that the AI reads before generating any code. Together they cover everything from component architecture to accessibility compliance:

| File | What It Covers |
|---|---|
| `.cursorrules` | Core rules, State Decision Tree, Complexity Ladder |
| `CLAUDE.md` | Entry point for Claude-based tools |
| `docs/design-system.md` | Tokens, typography, colors, spacing (8px grid) |
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

After extensive research, I identified the most common mistakes AI assistants make in React code and turned them into hard constraints. The AI is instructed to stop and propose the correct pattern when it detects any of these:

- State modeling mistakes (derived state, boolean explosion, direct mutation, prop mirroring)
- Effect pitfalls (useEffect for transformation, missing deps, stale closures)
- Architecture issues (god components, prop drilling, nested components, index keys)
- TypeScript safety (`any`, untyped event handlers)
- Data fetching errors (static queryKeys, inline fetch, unvalidated responses, conditional hooks)
- Accessibility violations (div as button, missing labels)
- Styling mistakes (hardcoded values, inline styles, removed focus outlines, layout animations)

### Design Token System

Every visual value lives in one file — `tokens.css`. Zinc gray palette, 8px spacing grid, modular typography scale, semantic color tokens with full dark mode support. Change one variable, every component updates. No hunting through files for hardcoded hex values.

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
3. `.cursorrules` tells the AI to read the relevant `docs/` files
4. Every component generated follows the same architecture — TypeScript interfaces, CSS Modules with tokens, semantic HTML, React Query, Zod validation, and explicit loading/error/empty states

I don't have to ask for any of this. The system ensures it happens automatically.

## Three Modes

I designed three implementation modes depending on the context:

| Mode | When | Approach |
|---|---|---|
| **Template** (default) | No design provided | Strict tokens, 8px grid |
| **Balanced** | Design provided | Structure from system, character from design |
| **Design-First** | Pixel-perfect spec | Exact values, system structure |

## What I Learned

This was a small-scale test. But even with a relatively simple challenge, the results showed that systematic, holistic guidance produces measurably better output. Anthropic describes this shift in their 2026 Agentic Coding Trends Report — engineers moving from writing code to orchestrating agents, focusing on architecture and constraints while AI handles implementation. That's exactly what this project explores.

The real leverage in AI-assisted development isn't in writing better prompts, it's in building better constraints. The model doesn't need to be smarter, it needs clear boundaries. Consistency at scale is what separates a collection of components from a maintainable product.

---

*Built by Niklaz Hallberg, February 2026.*
