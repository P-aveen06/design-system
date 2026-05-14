---
name: design-system
description: The user's canonical design system. Invoke before generating any UI code so you reference the user's tokens, components, and rules instead of guessing. The user customizes their theme in design-system.html; that file is the source of truth.
---

# Design System

This skill makes the user's customized design system the source of truth for any UI work.

## Source of truth

`~/.claude/skills/design-system/design-system.html` — a single self-contained HTML file the user has tuned to their taste. It defines:

- All design tokens as CSS custom properties (`--color-*`, `--space-*`, `--radius-*`, `--shadow-*`, `--font-*`, etc.)
- Every component, with predictable BEM-ish class names (`.btn`, `.btn--primary`, `.card`, `.card__header`, etc.)
- Patterns: app shell, dashboard, settings, auth, data management, forms
- States: empty, loading, error
- An icon set (inline SVG)

## When to invoke

Whenever the user asks for any of the following, read `design-system.html` first:

- "build a [page / component / form / dashboard / modal / ...]"
- "style this"
- "match our design"
- "make this look like the rest of the app"
- Any frontend work that produces user-visible UI

## How to use it

1. **Read `design-system.html`.** Find the `:root { ... }` block to learn the current token values. Find the component you're being asked to build to learn the exact class names and markup.
2. **Apply tokens, never hardcode.** Use `var(--color-accent)`, `var(--space-4)`, `var(--radius-md)` — never raw `#3B5BDB`, `16px`, `8px`.
3. **Reuse class names from the file.** If the file defines `.btn--primary`, use that — don't invent `.button-primary` or `.primary-button`.
4. **Follow the rules block** in the file (search for "Rules for AI agents" or read the embedded design.md export logic). Examples: buttons use `--radius-md`, cards use `--shadow-sm`, focus states use a 3px ring of `--color-accent-soft`.
5. **When composing patterns** (dashboards, auth pages, settings), refer to the Patterns section of the file for canonical compositions before assembling your own.

## Token quick reference

If for any reason you cannot read the file, fall back to these conventions:

- Colors use `--color-{role}` (`bg`, `surface`, `sunken`, `border`, `text`, `text-muted`, `accent`, `accent-hover`, `accent-soft`, `success`, `warning`, `danger`, `info`)
- Spacing uses `--space-1` through `--space-9` (geometric scale from base)
- Type scale uses `--text-xs` through `--text-3xl`
- Radius uses `--radius-sm` / `--radius-md` / `--radius-lg` / `--radius-xl` / `--radius-pill`
- Shadow uses `--shadow-xs` through `--shadow-xl`
- Font: `--font-sans` (body) and `--font-mono` (code)

Always prefer reading the actual file — these are fallbacks only.

## Boundaries

- Do not modify `design-system.html` unless the user explicitly asks. It is the user's customized artifact.
- Do not invent new tokens. If something the user wants isn't covered by an existing token, ask before adding one.
- Do not generate inline styles that bypass the token system. If you need to override, do it with another token, not a magic number.
