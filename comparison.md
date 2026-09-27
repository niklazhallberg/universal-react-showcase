# Structured Comparison: Baseline vs. Guided AI Output

This document records the method, briefs, observations and limitations behind the comparison summarised in the [README](./README.md).

*This is a structured showcase evaluation, not a statistically generalisable benchmark.*

> **Version boundary:** every result in this document relates to instruction **v1.0**. The Color Setup Flow, build order and self-audit protocol were added on 2026-02-13, after this evaluation, and haven't yet been tested under the same conditions.

---

## Method

### Conditions

| | Baseline | Guided |
|---|---|---|
| Project | Clean Vite + React + TypeScript scaffold | The same scaffold plus the instruction architecture (v1.0: core rules, design tokens, domain guides) |
| Editor | Cursor, Agent mode | Cursor, Agent mode |
| Model setting | "Auto" | "Auto" |
| Brief | Identical, pasted verbatim | Identical, pasted verbatim |
| Budget | One prompt; no follow-ups, Plan mode or manual edits | Same |

### Areas reviewed

Each output was reviewed by reading the generated code across the same areas:

1. **Data handling:** how responses are fetched, typed and validated
2. **Composition:** how responsibilities are split across files
3. **Design-system alignment:** whether visual values come from a shared source
4. **Theming:** how light and dark modes are handled
5. **Accessibility fundamentals:** semantics, ARIA, focus management and touch targets
6. **Error recovery:** what the user can do when something fails

For the fuller rubric to be used in future runs, see [`examples/README.md`](./examples/README.md#evaluation-rubric).

### Threats to validity

Stated up front so the findings can be read in proportion:

- **Model not pinned.** "Auto" lets Cursor pick the model, so the two conditions may have been served by different models.
- **One run per condition.** LLM output varies between runs; a single sample shows a tendency, not a distribution.
- **Instructions specify parts of the stack.** The guided condition was *told* to use React Query, Zod and a layered structure; the baseline was not told anything. Those differences show instruction-following and consistency, not independent engineering judgement.
- **Review by the author.** The output was assessed by the person who designed the instructions. There was no blind review.
- **No automated checks.** No type-check, lint, axe or Lighthouse runs were recorded.
- **Source not published.** The observations below are the author's written review. Code and screenshots are listed as evidence to add in [`examples/`](./examples/).

---

## Test 1: UserProfileCard

**Brief (both conditions):**

> Build a UserProfileCard component in React + TypeScript.
>
> Requirements:
> - Three visual variants: compact, default, featured
> - Fetch user data from https://jsonplaceholder.typicode.com/users/1
> - Show loading/error/empty states
> - A Follow/Following toggle button
> - Responsive and accessible
> - Well-structured file organization

### Observations

| Area | Baseline | Guided (v1.0) |
|---|---|---|
| Data handling | `useEffect` + `fetch` + three `useState` calls; `const data: User = await response.json()` with no runtime validation | React Query plus a Zod schema; response typed `unknown` before parsing, with the type inferred from the schema |
| Composition | Types, hook, CSS and component all inside `components/UserProfileCard/` | Split into `schemas/` → `api/` → `hooks/` → `components/` |
| Design-system alignment | Component-local variables with hardcoded values (`--upc-bg: #ffffff`, `padding: 24px`) | Global tokens throughout (`var(--color-surface)`, `var(--space-6)`, `var(--radius-md)`) |
| Theming | Its own `prefers-color-scheme` block with a Catppuccin palette | No theme code in the component; inherited from the token file |
| Accessibility | Good ARIA basics | Similar ARIA, plus a token-based focus ring and `prefers-reduced-motion` |
| Error recovery | Error message only | "Try again" action via `refetch()` |

**Note:** the brief asked for loading, error and empty states explicitly, so both conditions attempted them. The difference was in recovery, not coverage.

---

## Test 2: NotificationCenter

A more demanding brief: polling, focus management, animation and different layouts per viewport.

**Brief (both conditions):**

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

### Observations

**Data handling and polling.**
Baseline: `(await res.json()) as Post[]`, a cast without validation. Polling was built by hand with `useEffect` + `setInterval` + `AbortController`, roughly 100 lines covering fetching, loading, errors and cleanup.
Guided: a separate schema (`rawPostArraySchema.parse(data)`) and API layer, with polling as `useQuery({ queryFn: fetchNotifications, refetchInterval: 30_000 })`. The hook file mostly holds business logic (read/dismiss state).

**Composition.**

```
Baseline                                   Guided
hooks/useNotifications.ts                  schemas/notificationSchema.ts
  (fetch + state + transform)              api/notifications.ts
components/NotificationCenter/             hooks/useNotifications.ts
  NotificationCenter.tsx                   components/NotificationCenter/
  NotificationCenter.css   (global)          NotificationCenter.tsx
                                             NotificationCenter.module.css
```

**Design-system alignment and theming.**
Baseline: 350+ lines of global CSS with hardcoded values (`#1e1e2e`, `border-radius: 16px`, `padding: 16px 20px`, `transition: opacity 200ms ease`), and light mode duplicated in an 80+ line `@media (prefers-color-scheme: light)` block.
Guided: about 220 lines of scoped CSS Modules using tokens (`var(--color-surface)`, `var(--radius-md)`, `var(--space-3)`, `var(--transition-base)`), with no theme code in the component.

The line count comes from one run of one component. It's a side effect of pulling values from a shared source, not a quality measure in itself.

**Accessibility.**
Baseline: `aria-expanded`, `aria-live="polite"`, a focus trap, `role="status"`. Focus ring hardcoded (`outline: 2px solid #646cff`).
Guided: `aria-expanded`, `aria-haspopup`, `aria-modal`, `aria-live="polite"`, a focus trap **that returns focus** to the bell, `role="dialog"` and `role="feed"`, a token-based focus ring, and 44px minimum touch targets.
Both were assessed by reading the code only. Neither has been checked with a screen reader or an automated audit.

**House conventions.**
The guided output wrote Swedish JSDoc comments and Swedish `aria-label`s ("Notifikationer, 3 olästa"), following a project convention in the instructions. That shows the convention being followed; it isn't counted as a quality difference.

---

## Summary (v1.0)

| Area | Baseline tendency | Guided tendency (v1.0) | Seen in |
|---|---|---|---|
| Data validation | Type cast | Runtime schema | Tests 1, 2 |
| Data fetching | Manual effect + state | Query library | Tests 1, 2 |
| Composition | Co-located | Layered: schema → API → hook → component | Tests 1, 2 |
| Styling | Hardcoded values | Tokens | Tests 1, 2 |
| Theming | Rebuilt per component | Inherited | Tests 1, 2 |
| Focus ring | Hardcoded | Token-based | Tests 1, 2 |
| ARIA | Good basics | Basics + dialog semantics + focus return | Test 2 |
| Touch targets | Not specified | 44px minimum | Test 2 |
| Error recovery | Message only | Retry | Tests 1, 2 |

## What this shows

Both conditions produced working components, and a reviewer looking at either one in isolation would see competent React.

The difference shows in two places: the architecture behind the code, and what would happen at the next twenty components. The baseline invented its solutions per component: its own colours, its own data pattern, its own dark mode. The guided output reused one shared system without being asked.

The instructions didn't make the model smarter. They made its decisions **consistent and inspectable**, which is what lets design and engineering review focus on the parts that need human judgement.

## What this doesn't show

- That the effect holds across models, runs or other kinds of brief. In particular, no visual or art-direction-led brief has been tested yet.
- That the guided output needed less manual cleanup before review (not measured).
- Anything about v1.1. The Color Setup Flow, build order and self-audit protocol post-date these tests.
- Any comparison with third-party skills or rule sets.

**Next evaluation:** rerun the same scenarios with v1.1 to assess whether the Color Setup Flow increases art-direction flexibility without reducing structural consistency. See the [evidence roadmap](./examples/README.md#evidence-roadmap).
