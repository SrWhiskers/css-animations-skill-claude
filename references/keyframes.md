# Keyframes Reference

Full keyframe library and Tailwind animation config for the `css-animations` skill.

---

## Fade

```css
@keyframes fadeIn {
  from { opacity: 0; }
  to   { opacity: 1; }
}

@keyframes fadeOut {
  from { opacity: 1; }
  to   { opacity: 0; }
}

@keyframes fadeInUp {
  from { opacity: 0; transform: translate3d(0, 20px, 0); }
  to   { opacity: 1; transform: translate3d(0, 0, 0); }
}

@keyframes fadeInDown {
  from { opacity: 0; transform: translate3d(0, -20px, 0); }
  to   { opacity: 1; transform: translate3d(0, 0, 0); }
}

@keyframes fadeInLeft {
  from { opacity: 0; transform: translate3d(-20px, 0, 0); }
  to   { opacity: 1; transform: translate3d(0, 0, 0); }
}

@keyframes fadeInRight {
  from { opacity: 0; transform: translate3d(20px, 0, 0); }
  to   { opacity: 1; transform: translate3d(0, 0, 0); }
}
```

---

## Scale

```css
@keyframes scaleIn {
  from { opacity: 0; transform: scale(0.9); }
  to   { opacity: 1; transform: scale(1); }
}

@keyframes scaleOut {
  from { opacity: 1; transform: scale(1); }
  to   { opacity: 0; transform: scale(0.95); }
}

@keyframes pulse {
  0%, 100% { transform: scale(1); }
  50%       { transform: scale(1.05); }
}

@keyframes subtlePulse {
  0%, 100% { opacity: 0.85; transform: scale(1); }
  50%       { opacity: 1;    transform: scale(1.02); }
}

/* Use bounce sparingly — repeated bouncing is distracting */
@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50%       { transform: translateY(-10px); }
}
```

---

## Slide

```css
@keyframes slideInUp {
  from { opacity: 0; transform: translate3d(0, 16px, 0); }
  to   { opacity: 1; transform: translate3d(0, 0, 0); }
}

@keyframes slideInDown {
  from { opacity: 0; transform: translate3d(0, -16px, 0); }
  to   { opacity: 1; transform: translate3d(0, 0, 0); }
}

@keyframes slideInLeft {
  from { opacity: 0; transform: translate3d(-16px, 0, 0); }
  to   { opacity: 1; transform: translate3d(0, 0, 0); }
}

@keyframes slideInRight {
  from { opacity: 0; transform: translate3d(16px, 0, 0); }
  to   { opacity: 1; transform: translate3d(0, 0, 0); }
}
```

---

## Loading

```css
@keyframes spin {
  from { transform: rotate(0deg); }
  to   { transform: rotate(360deg); }
}

@keyframes shimmer {
  0%   { background-position: -200% 0; }
  100% { background-position:  200% 0; }
}

@keyframes skeletonPulse {
  0%, 100% { opacity: 1; }
  50%       { opacity: 0.55; }
}

@keyframes dotPulse {
  0%, 80%, 100% { opacity: 0.4; transform: scale(0.8); }
  40%           { opacity: 1;   transform: scale(1); }
}
```

---

## Custom Tailwind config

Only modify `tailwind.config.js` when project-wide reusable animation utilities are clearly needed. Always merge into the existing config — never replace the file.

```js
// tailwind.config.js
module.exports = {
  theme: {
    extend: {
      animation: {
        "fade-in":        "fadeIn 0.5s ease-out forwards",
        "fade-in-up":     "fadeInUp 0.5s ease-out forwards",
        "slide-in-right": "slideInRight 0.3s ease-out forwards",
        "scale-in":       "scaleIn 0.2s ease-out forwards",
        shimmer:          "shimmer 2s infinite",
      },
      keyframes: {
        fadeIn: {
          "0%":   { opacity: "0" },
          "100%": { opacity: "1" },
        },
        fadeInUp: {
          "0%":   { opacity: "0", transform: "translateY(20px)" },
          "100%": { opacity: "1", transform: "translateY(0)" },
        },
        slideInRight: {
          "0%":   { opacity: "0", transform: "translateX(20px)" },
          "100%": { opacity: "1", transform: "translateX(0)" },
        },
        scaleIn: {
          "0%":   { opacity: "0", transform: "scale(0.95)" },
          "100%": { opacity: "1", transform: "scale(1)" },
        },
        shimmer: {
          "0%":   { backgroundPosition: "-200% 0" },
          "100%": { backgroundPosition: "200% 0" },
        },
      },
    },
  },
};
```
