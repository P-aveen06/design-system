# Design System

A single-file, self-contained design system you can customize live in the browser — and feed to Claude as the canonical reference for every UI you build.

Open `design-system.html`, tweak the theme until it feels right, hit Save. Claude reads the same file as ground truth.

## Why this exists

Most design systems live as `design.md` files — token tables in prose. They work, but they have a blind spot: **AI agents can't *see* what your design looks like.** They read "primary color is #4F46E5, shadow-md is 0 4px 8px rgba(0,0,0,0.06)" and produce technically-correct UIs that still don't *feel* like your product.

This file solves that by being three things at once:

1. **A visual artifact** — Claude (with vision) can screenshot it and use it as pixel-level ground truth.
2. **A readable source** — every token is a CSS custom property, every component is named with predictable classes. Claude can grep it.
3. **An editor** — you, the human, customize it interactively. No build step, no config files.

When Claude is wired up as a skill (below), it reads this file before generating UI code, so the result actually matches your taste.

## Setup

This installs the design system as a Claude Code skill at `~/.claude/skills/design-system/`.

```sh
git clone --branch main --single-branch --depth 1 https://github.com/P-aveen06/design-system.git ~/.claude/skills/design-system && cd ~/.claude/skills/design-system && ./setup
```

After setup:

- Open `~/.claude/skills/design-system/design-system.html` in any browser.
- Adjust the controls drawer (accent, font, spacing, radius, shadows, density, custom token colors).
- Hit **Save**.
- In any Claude Code session, ask Claude to build UI — it'll automatically reference your design system.

> **Currently supports Claude only.** Other agents (Cursor, Copilot, etc.) aren't wired up yet — the skill format here is Claude Code-specific.

## Customizing

The controls drawer (right side of the page) lets you tune:

- **Mode** — light / dark
- **Accent** — curated swatches, hex input, or full color picker
- **Font** — Inter, system, serif, mono
- **Base font size + type scale ratio** — drives the whole type scale geometrically
- **Spacing base** — 4 / 6 / 8 px
- **Radius style** — sharp / soft / round / pill
- **Shadow style** — none / flat / soft / dramatic
- **Density** — compact / comfortable / spacious
- **Border weight** — 1 / 1.5 / 2 px
- **Per-token colors** — click any swatch in the Color section to pick a custom color for that specific token

Presets at the top of the drawer (Default / Mono / Warm / Vibrant) give you a starting point if you don't want to tune from scratch.

## Export

The **Export** button produces three artifacts your project (or any AI agent) can ingest:

- **`design.md`** — token table + rules + component snippets. Token-efficient, drop into any repo as the canonical AI reference.
- **`tokens.css`** — drop-in CSS custom properties.
- **`design-system.html`** — the entire customized file, with your theme baked in. Use this when an agent needs *visual* ground truth (screenshots).

Each artifact has Copy and Download in the modal.

## Aligning an existing app

The skill works in two modes:

- **Generate mode (default)** — ask Claude to build any UI and it'll use your tokens and components.
- **Alignment mode** — ask Claude to align/audit/refactor an existing codebase to your design system. It will:
  1. Detect your stack (framework, styling approach).
  2. Inventory hardcoded colors, spacing, radii, shadows, typography, and overlapping component classes.
  3. Produce a mapping report (current value → proposed token) for your review.
  4. Wait for your approval before editing anything.
  5. Drop in `tokens.css`, then refactor in reviewable passes — one commit per concern (colors → spacing → radii → shadows → type → components).

Trigger it with phrases like "align this app to my design system", "audit this codebase against the design system", or "refactor existing UI to use my tokens".

## How Claude uses this

After setup, Claude Code discovers the skill at `~/.claude/skills/design-system/SKILL.md`. When you ask Claude to build UI, it reads your `design-system.html` to learn the current tokens and rules before writing any code. Every component it generates uses your custom properties — never hardcoded values.

You can also invoke it explicitly: `/design-system` in Claude Code.

## What's inside

The file documents every UI primitive your apps will ever need: typography, color, spacing, radius, shadows, icons, buttons, inputs, navigation, cards, tables, lists, tabs, modals, toasts, badges — plus full pattern compositions (app shell, dashboard, settings, auth, data management, forms) and state demos (empty, loading, error).

## Roadmap

- Components and patterns are being filled in iteratively. Foundations are complete.
- Multi-agent support (beyond Claude Code) is on the table once the Claude integration is proven.

## License

MIT.
