# 02 · NotificationCenter

**Version:** v1.0 · **Status:** run; written review published in [`comparison.md`](../../comparison.md#test-2-notificationcenter) (2026-02-09)

## Brief

The full brief is in [`comparison.md`](../../comparison.md#test-2-notificationcenter). In short: a bell with an unread badge, a dropdown list, mark as read / mark all / dismiss, fetching and adapting remote data, a 30-second refresh, closing on outside click, an animated open/close, a full-screen panel on mobile, a focus trap, and screen-reader announcements.

## Conditions

The same as [01](../01-user-profile-card/#conditions).

## Observed (v1.0)

- **Baseline:** type-cast response; hand-built polling with `setInterval` and `AbortController` (~100 lines); 350+ lines of global CSS with hardcoded values and an 80+ line light-mode override block; focus trap without focus return; hardcoded focus ring.
- **Guided:** schema-validated response; polling via `refetchInterval`; layered files; ~220 lines of scoped, token-based CSS; dialog semantics; focus returns to the trigger; 44px targets.

## Limitations

One run per condition; model not pinned; line counts from a single run; accessibility assessed by reading code, not by testing.

## Evidence pending (v1.0)

| Artifact | What belongs here | Supports |
|---|---|---|
| `baseline/`, `guided/` | Complete generated source, unedited | Composition, data handling and CSS-volume observations |
| Screenshots | Desktop dropdown and mobile full-screen panel; light and dark; empty and error states | Visual consistency, responsive behaviour |
| Screen recording | Keyboard only: open, navigate, dismiss, close, focus return | Focus-management observation |
| Model record | The model Cursor served in each condition | Comparability of the two runs |
| axe report | Violations by severity, both conditions | The accessibility observation |
