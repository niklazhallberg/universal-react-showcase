# 03 · Campaign landing page

**Version:** v1.1 · **Status:** planned evaluation. No results yet.

## Why this scenario

Scenarios 01 and 02 are data components tested with v1.0. They show structural consistency but not whether the architecture leaves room for art direction. This scenario tests the central trade-off directly, and it's the first test of the v1.1 Color Setup Flow: **can the architecture stay consistent without everything looking the same?**

## Brief

> Build a single-page landing page in React and TypeScript for "Signal / Noise", a three-night audiovisual event in Stockholm exploring the meeting point between experimental electronic music, generative visuals and spatial sound.
>
> **Audience:** design-aware music listeners, creative technologists, artists, students and culturally curious visitors aged approximately 20–45.
>
> **Mood:** nocturnal, precise, tactile, cinematic, experimental and confident; avoid cyberpunk clichés, generic "AI" visuals and conventional festival-site patterns.
>
> **Creative direction:** the page should feel like an art-directed cultural identity rather than a SaaS product. Use a strong typographic hierarchy, editorial asymmetry, large image/video moments, restrained but intentional colour, and room for atmosphere. Make the palette a deliberate decision through the Color Setup Flow rather than inheriting placeholder token values.
>
> **Required sections:** hero with event identity and date/location; programme highlights; artist or contributor lineup; venue information; ticket CTA; practical information; newsletter signup; footer.
>
> **Required states:** ticket CTA states for available, low availability and sold out; newsletter success and error states; accessible mobile navigation.
>
> **Evaluation focus:** compare the baseline and guided outputs for art direction, visual hierarchy, controlled variation, colour-system decisions, responsive composition, component structure and the coverage of required UI states.

This is a planned v1.1 evaluation. It must not be presented as completed evidence until the same conditions have been run and documented.

## Planned conditions

| Condition | Setup |
|---|---|
| A · Baseline | Clean scaffold, no instructions |
| B · Guided, Template mode | Instructions v1.1, Color Setup Flow run first (inputs logged) |
| C · Guided, Balanced mode | Instructions v1.1 plus a reference image as seed |
| D · Guided + frontend skill *(optional)* | Condition B with a general-purpose frontend skill enabled, to explore the complementary-layer idea |

The model is pinned and recorded, with three runs per condition.

## What to look for

- Art-direction fidelity and visual distinctiveness (see the [rubric](../README.md#evaluation-rubric))
- Whether B and C diverge visually while sharing structure
- Whether token discipline flattens typography or motion

## Planned evaluation artifacts (v1.1)

| Artifact | What belongs here | Validates |
|---|---|---|
| Brief and references | The brief above plus any mood imagery used | Same input across conditions |
| Color Setup Flow log | The inputs given and the palette approved in B and C | That the flow ran as designed |
| Source per condition and run | Unedited output under `A/`, `B/`, `C/` (and `D/`) | Structural consistency |
| Screenshot grid | Hero and full page, desktop and mobile, every condition side by side | Controlled visual variation |
| Rubric scores | Per condition, with justifications | Observed tendencies |
