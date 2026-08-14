---
name: expo-feature-based-structure
description: Use when building, refactoring, or reviewing an Expo Router (React Native) app so that routing stays in src/app, screen implementations live in src/features, and shared reusable UI lives in src/components/app. Triggers on requests to organize, restructure, split, or migrate Expo app routes, screens, features, or components into a feature-based folder structure.
---

# Expo Feature-Based Structure

## Overview

Keep Expo Router routing in `src/app/**`, screen implementation in `src/features/**/screens`, and cross-feature reusable UI in `src/components/app/**`. This separates navigation from implementation and prevents monolithic screen files.

## Directory Contract

```text
src/
  app/                 # routing only (Expo Router)
  features/            # domain/feature-owned screens, components, hooks, data
  components/app/      # cross-feature shared UI primitives
  lib/                 # generic helpers (formatting, etc.)
```

Each `src/features/<feature>/` folder contains:

```text
src/features/<feature>/
  screens/       # one screen component per file, ends with -screen.tsx
  components/    # feature-local sections and UI
  hooks/         # feature-local state/hooks
  data/          # optional mock/stitch/demo data
```

## Core Rules

- `src/app/**` is routing only. Route files do not contain screen implementation.
- Route folders use Expo Router conventions: `index.tsx` and optional `_layout.tsx`.
- Each route `index.tsx` re-exports exactly one screen from `src/features/**/screens`.
- Screen files orchestrate state and compose sections.
- Reusable feature-specific sections live under the owning feature's `components/`.
- Cross-feature reusable primitives live in `src/components/app/`.
- Feature, screen, and route names use English only. UI copy may remain localized.
- No file defines multiple full screens.

## Naming Conventions

| Kind | File | Component |
|------|------|-----------|
| Route | lowercase kebab-case folders, `index.tsx` | n/a (re-export only) |
| Screen | `<name>-screen.tsx` | PascalCase ending in `Screen` |
| Feature component | kebab-case describing the section | PascalCase |
| Shared component | kebab-case | PascalCase |

Examples:

- `src/app/auth/sign-in/index.tsx`
- `src/features/auth/screens/sign-in-screen.tsx` exports `SignInScreen`
- `src/features/checkout/components/payment-method-card.tsx` exports `PaymentMethodCard`

## Screen Responsibilities

Each `screens/*-screen.tsx` should:

- define the route-level screen component
- own screen-level state
- own navigation actions for that screen
- compose feature sections and shared UI primitives

It should not:

- define large reusable UI blocks inline
- contain multiple unrelated screens
- own unrelated feature logic

Split a section into a feature component when the screen exceeds roughly 200 lines, or when a section is reused by a second screen.

## Shared vs Feature-Specific

Put code in `src/components/app/**` only when at least one is true:

- used by multiple features
- encodes app-wide layout behavior
- is a stable design-system primitive

Otherwise keep it in the feature. When reuse is uncertain, keep it feature-local first.

Shared examples: `screen-container`, `primary-button`, `status-badge`, `app-icon`.
Feature-local examples: `payment-method-list`, `promo-card`, `transaction-group-list`.

Organize shared UI by role:

```text
src/components/app/
  actions/       # buttons
  display/       # icons, images, badges, tiles
  form/          # inputs
  layout/        # containers, cards, headers
  navigation/    # nav bars
```

## Data and State Placement

- hardcoded/demo/stitch-derived content stays under `src/features/<feature>/data`
- feature hooks stay with the owning feature: `src/features/<feature>/hooks/*.ts`
- generic formatting helpers move to `src/lib` (for example `src/lib/format/currency.ts`)

## Example Route Entry

```tsx
// src/app/auth/sign-in/index.tsx
export { SignInScreen as default } from '@/features/auth/screens/sign-in-screen';
```

This is the desired size and responsibility of a route file.

## Migration Order

1. normalize route names to English
2. move route files into route folders using `index.tsx`
3. split screens into per-screen files under `src/features`
4. extract feature-specific sections from screen files
5. extract true shared primitives into `src/components/app`
6. move data/helpers into feature `hooks`/`data` and `src/lib`
7. remove old monolithic files

## Constraints

- preserve Expo Router behavior and current UI output unless revising intentionally
- target the app's current Expo SDK compatibility
- prefer small focused files over a shared dumping ground
