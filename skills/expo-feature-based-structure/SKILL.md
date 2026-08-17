---
name: expo-feature-based-structure
description: Use when building, refactoring, or reviewing an Expo Router (React Native) app so that routing stays in src/app, screen implementations live in src/features, and shared reusable UI lives in src/components/app. Triggers on requests to organize, restructure, split, or migrate Expo app routes, screens, features, or components into a feature-based folder structure.
---

# Expo Feature-Based Structure

## Overview

Keep Expo Router routing in `src/app/**`, screen implementation in `src/features/**/screens`, and cross-feature reusable UI in `src/components/app/**`. This separates navigation from implementation and prevents monolithic screen files. Every file defines exactly one component, so no file ever holds two component functions.

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
  components/    # feature-local components, one per file
  hooks/         # feature-local state/hooks
  data/          # optional mock/stitch/demo data
```

## Core Rules

- `src/app/**` is routing only. Route files do not contain screen implementation.
- Route folders use Expo Router conventions: `index.tsx` and optional `_layout.tsx`.
- Each route `index.tsx` re-exports exactly one screen from `src/features/**/screens`.
- Screen files orchestrate state and compose sections.
- All feature-specific components live under the owning feature's `components/`, whether or not they are reused.
- Cross-feature reusable primitives live in `src/components/app/`.
- Feature, screen, and route names use English only. UI copy may remain localized.
- Every file defines exactly one component: a screen file contains only the screen component, and a component file contains only that component.
- Never define two component functions in one file. Extract even small ones (for example a `RememberMeCheckbox`) into their own file.

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
- `src/features/auth/components/remember-me-checkbox.tsx` exports `RememberMeCheckbox`
- `src/features/checkout/components/payment-method-card.tsx` exports `PaymentMethodCard`

## Screen Responsibilities

Each `screens/*-screen.tsx` should:

- define the route-level screen component
- own screen-level state
- own navigation actions for that screen
- compose feature sections and shared UI primitives

It should not:

- define any other component inline, even a small one such as a `RememberMeCheckbox` row or a single list item
- define multiple components of any kind
- own unrelated feature logic

Extract every non-screen component into its own file as soon as it exists — size and reuse are never factors. If the screen itself exceeds roughly 200 lines, treat that as a signal to decompose more of its content into component files, not as permission to keep components inline.

## One Component Per File

A function is a component when it returns JSX and is rendered as `<Foo />`. Such functions always live in their own file, with no exceptions for size or single use.

- Screen files contain only the screen component. Component files contain only their one component and export only it.
- `renderItem`-style render callbacks must delegate to a named component file, for example `renderItem={({ item }) => <PaymentMethodCard method={item} />}`.
- Plain non-component helpers (formatters, mappers, validators) and event handlers that do not render JSX may stay in the screen file, following the Data and State Placement rules.

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
4. extract every non-screen component from screen files into its own per-component file
5. extract true shared primitives into `src/components/app`
6. move data/helpers into feature `hooks`/`data` and `src/lib`
7. remove old monolithic files

## Constraints

- preserve Expo Router behavior and current UI output unless revising intentionally
- target the app's current Expo SDK compatibility
- prefer small focused files over a shared dumping ground
