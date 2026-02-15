# AI Instruction Architecture: A/B Testing AI Code Generation

## Method

Two A/B tests. Same prompt, same IDE (Cursor), same model selection (Auto), same requirements. The only variable: one project has an instruction architecture (15 docs, a design token system, and a `.cursorrules` file). The other is a clean Vite + React + TypeScript scaffold with zero constraints.

Both tests used Cursor's Agent mode with model set to "Auto", meaning the AI chose which model to use. No manual intervention, no follow-up prompts, no Plan mode. One prompt in, result out.

---

## Test 1: UserProfileCard

**Prompt given to both projects:**

> Build a UserProfileCard component in React + TypeScript.
>
> Requirements:
> - Three visual variants: compact, default, featured
> - Fetch user data from https://jsonplaceholder.typicode.com/users/1
> - Show loading/error/empty states
> - A Follow/Following toggle button
> - Responsive and accessible
> - Well-structured file organization

### Results

**Data fetching:**
Without system — raw `useEffect` + `fetch` + three manual `useState` calls. Response cast directly: `const data: User = await response.json()`. No runtime validation.
With system — React Query + Zod schema. Response typed as `unknown` before parsing. Type inferred from schema with `z.infer<typeof schema>`.

**File structure:**
Without system — everything inside `components/UserProfileCard/` (types, hooks, CSS, component).
With system — separated into `schemas/` → `api/` → `hooks/` → `components/`. Each layer independently testable.

**Styling:**
Without system — custom local CSS variables with hardcoded hex (`--upc-bg: #ffffff`, `padding: 24px`). Self-contained island, incompatible with any design system.
With system — every value references a global token (`var(--color-surface)`, `var(--space-6)`, `var(--radius-md)`). Change one token, every component updates.

**Dark mode:**
Without system — per-component implementation using `prefers-color-scheme` with Catppuccin palette. Every new component needs its own dark mode.
With system — no dark mode code in the component. Inherits automatically from `tokens.css`.

**Accessibility:**
Both produced good ARIA attributes. The system version additionally used token-based focus rings and `prefers-reduced-motion`.

**Error recovery:**
Without system — error message, no retry. With system — "Try again" button using React Query's `refetch()`.

---

## Test 2: NotificationCenter (Complex Component)

A more demanding test with state management, polling, animations, responsive behavior, and advanced accessibility requirements.

**Prompt given to both projects:**

> Build a NotificationCenter in React + TypeScript.
>
> Requirements:
> - A notification bell icon in the top-right corner showing unread count as a badge
> - Clicking the bell opens a dropdown panel listing notifications
> - Each notification has: title, message, timestamp, read/unread status, and a type (info, success, warning, error)
> - User can mark individual notifications as read, or mark all as read
> - User can dismiss (delete) individual notifications
> - Fetch notifications from https://jsonplaceholder.typicode.com/posts (adapt the response to fit notification shape)
> - Auto-refresh notifications every 30 seconds
> - Close dropdown when clicking outside
> - Animate the dropdown open/close
> - Responsive: full-screen panel on mobile, dropdown on desktop
> - Accessible: keyboard navigation, focus trap in dropdown, screen reader announcements for new notifications
>
> Create all necessary files.

### Results

**API & validation:**
Without system — `(await res.json()) as Post[]`. Cast without validation. TypeScript satisfied, runtime unprotected.
With system — separate Zod schema (`rawPostArraySchema.parse(data)`), separate API layer, response typed as `unknown` before parsing. If the API shape changes, Zod throws a descriptive error instead of silent property access failures.

**Data fetching & polling:**
Without system — manual `useEffect` + `setInterval` + `AbortController`. ~100 lines of handwritten boilerplate for fetch, error handling, loading state, and cleanup.
With system — `useQuery({ queryFn: fetchNotifications, refetchInterval: 30_000 })`. Caching, deduplication, background refetch, and error handling included. The hook file focuses on business logic (read/dismiss state), not plumbing.

**File structure:**
Without system:
```
hooks/useNotifications.ts      ← fetch + state + transform in one file
components/NotificationCenter/
  NotificationCenter.tsx
  NotificationCenter.css       ← global CSS
```

With system:
```
schemas/notificationSchema.ts  ← validation layer
api/notifications.ts           ← fetch + transform layer
hooks/useNotifications.ts      ← React Query wrapper + local state
components/NotificationCenter/
  NotificationCenter.tsx
  NotificationCenter.module.css ← scoped CSS Modules
```

**Styling:**
Without system — 350+ lines CSS with hardcoded values (`background-color: #1e1e2e`, `border-radius: 16px`, `padding: 16px 20px`, `transition: opacity 200ms ease`). Light mode duplicated in a separate `@media (prefers-color-scheme: light)` block — 80+ lines of overrides.

With system — 220 lines CSS Modules using tokens throughout (`var(--color-surface)`, `var(--radius-md)`, `var(--space-3)`, `var(--transition-base)`). No duplicated dark mode — theme switching handled by tokens.css. 37% less CSS with better dark mode support.

**Accessibility:**
Without system: `aria-expanded`, `aria-live="polite"`, focus trap, `role="status"`. Focus ring: `outline: 2px solid #646cff` (hardcoded).
With system: `aria-expanded` + `aria-haspopup` + `aria-modal`, `aria-live="polite"`, focus trap with focus-return to bell button, `role="dialog"` + `role="feed"`. Focus ring: `var(--focus-ring-width) solid var(--focus-ring-color)` (token-based). Swedish aria-labels ("Notifikationer, 3 olästa"). 44px minimum touch targets (WCAG 2.5.5).

**Comments:**
Without system — English, minimal.
With system — Swedish JSDoc comments explaining purpose and architecture, per `.cursorrules`.

---

## Summary — Both Tests Combined

| Aspect | Without System | With System |
|---|---|---|
| API validation | `as Type` cast | Zod schema + `unknown` |
| Data fetching | useEffect + fetch + manual state | React Query |
| Polling | setInterval + AbortController | `refetchInterval` (one line) |
| File structure | Everything co-located | schema → api → hook → component |
| Styling | Hardcoded hex/px/rgba | Design tokens throughout |
| Dark mode | Per-component, duplicated CSS | Automatic via tokens.css |
| CSS volume (NotificationCenter) | ~350 lines | ~220 lines (37% less) |
| Focus rings | Hardcoded color/width | Token-based, consistent |
| ARIA completeness | Good basics | Comprehensive (dialog, feed, modal, haspopup) |
| Touch targets | Not specified | 44px minimum (WCAG 2.5.5) |
| Error recovery | No retry | Retry via React Query |
| Comments | English, minimal | Swedish JSDoc, architectural |
| Consistency across components | No guarantee | Guaranteed via shared tokens and patterns |

---

## What This Shows

Both projects produced functional components. Both render correctly. A code reviewer looking at a single component from either project would see competent React code.

The difference becomes visible in two places: the architecture behind the code, and what happens when you build the next 20 components.

The unconstrained AI invents solutions per component — its own colors, its own data patterns, its own dark mode approach, its own CSS variables. Each component is an island. The constrained AI follows a shared architecture — the same tokens, the same validation pipeline, the same data patterns, the same accessibility standards — automatically, without being asked.

The instruction architecture doesn't make the AI smarter. It makes it consistent. And consistency at scale is what separates a collection of components from a maintainable product.

---

The instruction architecture is available at [github.com/niklazhallberg/universal-react-template](https://github.com/niklazhallberg/ai-instruction-architecture).*
