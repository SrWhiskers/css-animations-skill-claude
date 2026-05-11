---
name: css-animations
description: Expert CSS animations for frontend UI — keyframes, transitions, loading spinners, skeletons, shimmer effects, hover lifts, microinteractions, staggered entrances, and Tailwind animation utilities, without JavaScript dependencies. Always use this skill when the user asks for any CSS-based animation or motion effect, including: hover transitions, focus animations, entrance and exit effects, loading states, skeleton loading, pulsing, spinning, fading, scaling, sliding, bouncing, button press effects, card lift effects, dot loaders, CSS-only microinteractions, performance-conscious animations, or reduced-motion-safe animations. Also trigger when the user says "animate this", "add a transition", "make this feel smoother", "add a loading state", or "make it more interactive" — even without explicitly mentioning CSS. Use for Tailwind projects too when animation utilities are involved. Do not wait for the user to ask for a "skill" — if the task involves CSS motion or animation, use this.
---

A pure frontend presentation skill. Handles CSS keyframes, transitions, and motion only.

Do not touch business logic, authentication, routing, API behavior, database logic, payment flows, analytics, user-data handling, or state management unrelated to visual animation — unless explicitly requested outside this skill's scope.

## Scope constraints

CSS-specific rules that override Claude's general judgment:

- Do not animate layout-triggering properties (`width`, `height`, `top`, `left`, `margin`, `padding`) unless explicitly justified
- Do not use `transition: all` in production — use explicit properties
- Do not modify `tailwind.config.js` unless project-wide reusable utilities are clearly needed; always merge, never replace the file
- Do not install animation libraries or add dependencies
- Do not add remote stylesheets, remote fonts, or remote animation assets without explicit user approval
- Do not put untrusted user input into CSS properties, style attributes, class names, or CSS variables
- Do not use CSS animation to hide, delay, or obscure: error messages, auth errors, validation messages, consent prompts, payment details, pricing, legal notices, or destructive action confirmations
- Prefer local CSS modules, scoped classes, or existing Tailwind utilities for small changes over global CSS edits

## Core principles

CSS animations should be:

1. Lightweight — prefer `transform` and `opacity`; avoid layout-affecting properties
2. Scoped — prefer component-level over global
3. Accessible — always handle `prefers-reduced-motion`
4. Predictable — content must remain visible if animation is disabled or fails
5. Performant — use GPU-accelerated properties; use `will-change` sparingly
6. Non-invasive — do not touch unrelated files or logic

## CSS Transitions

Use for simple state changes: hover, focus, active, open/closed, selected, disabled.

```css
/* Preferred: explicit properties */
.element {
  transition:
    transform 0.2s ease-out,
    opacity 0.2s ease-out,
    box-shadow 0.2s ease-out;
}

.element:hover {
  transform: translateY(-4px);
  box-shadow: 0 12px 24px rgb(0 0 0 / 15%);
}
```

Avoid `transition: all` in production — it may animate unintended properties and cause layout, paint, or accessibility issues.

Always include `:focus-visible` alongside `:hover` for interactive elements:

```css
.card:hover,
.card:focus-visible {
  transform: translateY(-4px);
  box-shadow: 0 12px 24px rgb(0 0 0 / 15%);
}
```

Never remove focus outlines without providing an equally visible replacement.

## CSS Keyframe Animations

Use for reusable animations not tied to a single state transition.

```css
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translate3d(0, 20px, 0);
  }
  to {
    opacity: 1;
    transform: translate3d(0, 0, 0);
  }
}

.element {
  animation: fadeInUp 0.5s ease-out forwards;
}
```

When using `animation-fill-mode: forwards`, confirm the element remains accessible and does not get stuck invisible.

Shorthand:

```css
.element {
  animation: slideIn 0.5s ease-out 0.2s 1 normal forwards;
}
```

Use `infinite` animations only when necessary, subtle, and reduced-motion-safe. Acceptable uses: loading spinners, skeleton shimmer, small notification pulse.

**For the full keyframe library** (fade, scale, slide, loading, bounce variants), read `references/keyframes.md`.

## Timing functions

```css
/* Standard */
ease-out          /* Most UI interactions */
ease-in-out       /* Symmetric enter/exit */
linear            /* Spinners, progress bars */

/* Material Design standard */
cubic-bezier(0.4, 0, 0.2, 1)

/* Overshoot — use sparingly, can feel distracting */
cubic-bezier(0.68, -0.55, 0.265, 1.55)

/* Stepped — for sprite-like animations */
steps(4, end)
```

## Staggered animations with CSS custom properties

Use internally generated numeric index only — never untrusted user input.

```css
.stagger-item {
  opacity: 0;
  animation: fadeInUp 0.45s ease-out forwards;
  animation-delay: min(calc(var(--index) * 80ms), 600ms);
}

@media (prefers-reduced-motion: reduce) {
  .stagger-item {
    opacity: 1;
    transform: none;
    animation: none;
  }
}
```

```tsx
{items.map((item, index) => (
  <div
    key={item.id}
    className="stagger-item"
    style={{ "--index": index } as React.CSSProperties}
  >
    {item.content}
  </div>
))}
```

## Tailwind

Use Tailwind animation classes only if Tailwind is already present. Do not add Tailwind to a project for this skill.

```tsx
/* Built-in */
<div className="animate-spin motion-reduce:animate-none" />
<div className="animate-pulse motion-reduce:animate-none" />
<div className="animate-ping motion-reduce:animate-none" />
<div className="animate-bounce motion-reduce:animate-none" />

/* Prefer motion-safe for opt-in approach */
<div className="motion-safe:animate-pulse motion-reduce:animate-none" />
```

For custom Tailwind animations and `tailwind.config.js` extension, read `references/keyframes.md`.

## Performance

Prefer GPU-accelerated properties:

```
transform, opacity, background-position, filter (sparingly)
```

Avoid animating:

```
width, height, top, left, margin, padding, border-width, font-size
```

`will-change: transform` — add only when motion stutters, remove after. Do not apply broadly.

Animating `box-shadow` and `filter` is expensive — limit to small components, short durations.

## Reduced motion

Always scope reduced-motion rules to the specific component, not globally, unless the project has agreed to a global policy.

```css
@media (prefers-reduced-motion: reduce) {
  .fade-in-up {
    animation: none;
    opacity: 1;
    transform: none;
  }
}
```

Tailwind:

```tsx
<div className="transition-transform hover:-translate-y-1 motion-reduce:transition-none motion-reduce:hover:translate-y-0">
```

Global override (use only if project policy agrees):

```css
@media (prefers-reduced-motion: reduce) {
  *, *::before, *::after {
    animation-duration: 0.01ms !important;
    animation-iteration-count: 1 !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
```

## Component patterns

For ready-to-use components — Spinner, Skeleton, Shimmer, Hover Lift Card, Focus Ring, Button Press, Dot Pulse Loader — with full accessibility semantics and Tailwind variants, read `references/components.md`.

## Production checklist

- [ ] Animates `transform` / `opacity`, not layout properties
- [ ] No `transition: all` in production
- [ ] `prefers-reduced-motion` handled
- [ ] Content visible if animation is disabled or fails
- [ ] Error, auth, consent, payment, and legal messages not hidden or delayed
- [ ] Focus states remain visible
- [ ] Keyboard users get equivalent visual feedback
- [ ] `will-change` used sparingly
- [ ] No large animated `filter` or `blur`
- [ ] Infinite animations are subtle and necessary
- [ ] Tailwind config merged (not replaced) if modified
- [ ] No untrusted input in CSS, style props, class names, or CSS variables
- [ ] No remote assets added without approval
- [ ] No dependencies installed
- [ ] No unrelated files modified
- [ ] Spinners and skeletons have appropriate `role` and `aria-label`
- [ ] `aria-hidden="true"` on purely decorative markup layers
- [ ] Project lint and build pass
