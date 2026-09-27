# 02 · NotificationCenter

**Status:** run against instruction v1.0 (published 2026-02-09). The written review is in [`comparison.md`](../../comparison.md#test-2-notificationcenter). Source code and screenshots haven't been added yet.

## Brief

See the full brief in [`comparison.md`](../../comparison.md#test-2-notificationcenter). In short: a bell with an unread badge, a dropdown list, mark as read / mark all / dismiss, fetching and adapting remote data, a 30-second refresh, closing on outside click, an animated open/close, a full-screen panel on mobile, a focus trap, and screen-reader announcements.

## Conditions

The same as [01](../01-user-profile-card/#conditions).

## Outcome summary

- **Baseline:** type-cast response; hand-built polling with `setInterval` and `AbortController` (~100 lines); 350+ lines of global CSS with hardcoded values and an 80+ line light-mode override block; focus trap without focus return; hardcoded focus ring.
- **Guided:** schema-validated response; polling via `refetchInterval`; layered files; ~220 lines of scoped, token-based CSS; dialog semantics; focus returns to the trigger; 44px targets.

## Limitations

One run per condition; model not pinned; line counts from a single run; accessibility assessed by reading code, not by testing.

## Evidence to add

- [ ] `baseline/`: generated source, unedited
- [ ] `guided/`: generated source, unedited
- [ ] Screenshots: desktop dropdown and mobile full-screen panel, light and dark, empty and error states
- [ ] Screen recording of keyboard-only use (open, navigate, dismiss, close, focus return)
- [ ] axe results for both
- [ ] Exact model used in each condition
