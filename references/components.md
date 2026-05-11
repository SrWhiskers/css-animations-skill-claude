# Component Patterns Reference

Ready-to-use CSS animation component patterns for the `css-animations` skill.
All patterns include accessibility semantics and reduced-motion handling.

---

## Loading Spinner

If the spinner communicates loading, expose it to assistive technologies via `role="status"`.
If visible text already says "Loading", use `aria-hidden="true"` instead.

```tsx
export function Spinner({
  size = "md",
  label = "Loading",
}: {
  size?: "sm" | "md" | "lg";
  label?: string;
}) {
  const sizes = {
    sm: "h-4 w-4 border-2",
    md: "h-8 w-8 border-2",
    lg: "h-12 w-12 border-4",
  };

  return (
    <div role="status" aria-label={label}>
      <div
        className={`${sizes[size]} animate-spin rounded-full border-blue-500 border-t-transparent motion-reduce:animate-none`}
      />
    </div>
  );
}
```

CSS version:

```css
.spinner {
  width: 2rem;
  height: 2rem;
  border: 2px solid #3b82f6;
  border-top-color: transparent;
  border-radius: 999px;
  animation: spin 1s linear infinite;
}

@keyframes spin {
  to { transform: rotate(360deg); }
}

@media (prefers-reduced-motion: reduce) {
  .spinner { animation: none; }
}
```

---

## Skeleton Loading

Skeletons are decorative placeholders. Use `aria-hidden="true"` on each element.
Give the container a `role="status"` or `aria-label` so loading is announced.

```tsx
export function Skeleton({
  className = "",
  ...props
}: React.HTMLAttributes<HTMLDivElement>) {
  return (
    <div
      aria-hidden="true"
      className={`animate-pulse rounded bg-gray-200 motion-reduce:animate-none ${className}`}
      {...props}
    />
  );
}
```

Usage:

```tsx
<div role="status" aria-label="Loading content" className="space-y-2">
  <Skeleton className="h-4 w-3/4" />
  <Skeleton className="h-4 w-1/2" />
  <Skeleton className="h-4 w-5/6" />
</div>
```

Do not use skeleton animations to hide real errors or delay important messages.

---

## Shimmer Effect

Use sparingly. Always disable for reduced-motion users.

```css
.shimmer {
  background: linear-gradient(90deg, #f0f0f0 0%, #e0e0e0 50%, #f0f0f0 100%);
  background-size: 200% 100%;
  animation: shimmer 1.5s infinite;
}

@keyframes shimmer {
  0%   { background-position: -200% 0; }
  100% { background-position:  200% 0; }
}

@media (prefers-reduced-motion: reduce) {
  .shimmer { animation: none; }
}
```

---

## Hover Lift Card

Always use explicit transition properties, not `transition: all`.
Always include `:focus-visible` alongside `:hover`.

```css
.card {
  transition:
    transform 0.2s ease-out,
    box-shadow 0.2s ease-out;
}

.card:hover,
.card:focus-visible {
  transform: translateY(-4px);
  box-shadow: 0 12px 24px rgb(0 0 0 / 15%);
}

@media (prefers-reduced-motion: reduce) {
  .card { transition: none; }
  .card:hover, .card:focus-visible { transform: none; }
}
```

Tailwind:

```tsx
<div className="transition-[transform,box-shadow] duration-200 ease-out hover:-translate-y-1 hover:shadow-xl focus-visible:-translate-y-1 focus-visible:shadow-xl motion-reduce:transition-none motion-reduce:hover:translate-y-0 motion-reduce:focus-visible:translate-y-0">
  Card content
</div>
```

---

## Focus Ring Animation

Focus states must remain visible. Never remove outlines without a strong visible replacement.

```css
.interactive {
  transition:
    outline-color 0.2s ease,
    box-shadow 0.2s ease;
}

.interactive:focus-visible {
  outline: 2px solid transparent;
  box-shadow: 0 0 0 3px rgb(59 130 246 / 40%);
}
```

Avoid:

```css
/* Don't do this without a replacement */
button:focus { outline: none; }
```

---

## Button Press

```css
.button {
  transition:
    transform 0.12s ease-out,
    opacity 0.12s ease-out;
}

.button:active {
  transform: scale(0.98);
}
```

Tailwind:

```tsx
<button className="transition-transform duration-150 active:scale-[0.98] motion-reduce:transition-none motion-reduce:active:scale-100">
  Save
</button>
```

Do not use animation to obscure disabled, loading, error, or destructive action states.

---

## Dot Pulse Loader

```css
.dot-loader {
  display: inline-flex;
  gap: 0.25rem;
}

.dot-loader span {
  width: 0.5rem;
  height: 0.5rem;
  border-radius: 999px;
  background: currentColor;
  animation: dotPulse 1.2s infinite ease-in-out;
}

.dot-loader span:nth-child(2) { animation-delay: 0.15s; }
.dot-loader span:nth-child(3) { animation-delay: 0.3s; }

@keyframes dotPulse {
  0%, 80%, 100% { opacity: 0.4; transform: scale(0.8); }
  40%           { opacity: 1;   transform: scale(1); }
}

@media (prefers-reduced-motion: reduce) {
  .dot-loader span { animation: none; }
}
```

```tsx
export function DotLoader({ label = "Loading" }: { label?: string }) {
  return (
    <span className="dot-loader" role="status" aria-label={label}>
      <span />
      <span />
      <span />
    </span>
  );
}
```

---

## Examples

### Hover lift on cards

```css
.card {
  transition:
    transform 0.2s ease-out,
    box-shadow 0.2s ease-out;
}

.card:hover,
.card:focus-visible {
  transform: translateY(-4px);
  box-shadow: 0 12px 24px rgb(0 0 0 / 15%);
}

@media (prefers-reduced-motion: reduce) {
  .card { transition: none; }
  .card:hover, .card:focus-visible { transform: none; }
}
```

---

### Skeleton loading state

```tsx
function ContentSkeleton() {
  return (
    <div role="status" aria-label="Loading content" className="space-y-2">
      <div aria-hidden="true" className="h-4 w-3/4 animate-pulse rounded bg-gray-200 motion-reduce:animate-none" />
      <div aria-hidden="true" className="h-4 w-1/2 animate-pulse rounded bg-gray-200 motion-reduce:animate-none" />
      <div aria-hidden="true" className="h-4 w-5/6 animate-pulse rounded bg-gray-200 motion-reduce:animate-none" />
    </div>
  );
}
```

---

### Staggered list entrance

```css
.stagger-item {
  opacity: 0;
  animation: fadeInUp 0.45s ease-out forwards;
  animation-delay: min(calc(var(--index) * 80ms), 600ms);
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translateY(12px); }
  to   { opacity: 1; transform: translateY(0); }
}

@media (prefers-reduced-motion: reduce) {
  .stagger-item { opacity: 1; transform: none; animation: none; }
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
