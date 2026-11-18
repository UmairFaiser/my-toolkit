- For the number of fonts you should use, fewer is almost always better. It of course depends on the context too. Marketing pages, for example, can be more expressive than apps, but I rarely use more than three different fonts in a single project.

- x-height
The height of a lowercase x.

cap height
The height of uppercase letters.

baseline
The invisible line the letters sit on.

ascender
The part of a letter that rises above the x-height.

descender
The part that drops below the baseline.

- Large headings often look better with slightly negative letter-spacing. Small uppercase labels usually need a subtle positive letter-spacing so the letters don’t feel crowded.

- Headings usually want a tighter line height around 1.1. For body copy, 1.5 to 1.6 is a good place to start and adjust from there. Unitless values scale with the font size. Fixed values like line-height: 24px don’t.


Tailwind also gives you line-height classes like leading-snug, leading-normal and leading-relaxed. These are sensible defaults and you rarely need to use other values.

- Every line also has a little extra space above and below the letters, which is why text can sometimes sit slightly too low inside a button or a badge.


text-box trims that space so the box hugs the letters. It consists of two parts: which edges to trim (trim-both, trim-start or trim-end) and where to trim.

cap cuts the top at the cap height, alphabetic cuts the bottom at the baseline and text cuts at the font’s own text edge, which keeps room for descenders below.

- When building a component first think about its state too.

- when you want to debug an animation you can slow the animation down

- I’m pretty sure you’ve tried to interact with an element on the web and struggled because it was too small. There’s a common trick you can use to increase the hit area of an element without increasing its actual size.


Apple’s Human Interface Guidelines recommend a hit area of at least 44x44, but that mostly applies to mobile. WCAG 2.2 requires at least 24x24, but that feels quite small so I generally do not go below 32x32 on desktop and below 44x44 on mobile.

The easiest way to increase an element’s hit area without changing its size or affecting the layout is to use a pseudo-element.

```css
.icon-button {
  position: relative;
}

.icon-button::after {
  content: "";
  position: absolute;
  inset: -8px;
}
```

- using reduced motion. You can wrap the whole app once in the root layout With `reducedMotion="user"` so it does not need to be handled for each and every component
```js
"use client";

import { domMax, LazyMotion, MotionConfig } from "motion/react";

export function MotionProvider({ children }: { children: React.ReactNode }) {
  return (
    <LazyMotion features={domMax}>
      <MotionConfig reducedMotion="user">{children}</MotionConfig>
    </LazyMotion>
  );
}
```
MotionConfig does not affect animations or transitions defined in CSS, so anything animated there needs a separate solution. Luckily Tailwind offers two options.

```js
// Scales for everyone
<button className="transition-transform duration-200 active:scale-[0.97]">

// Scales only for users who have not requested reduced motion
<button className="transition-transform duration-200 motion-safe:active:scale-[0.97]">
```
Both are media queries under the hood. `motion-safe` applies styles when the user has not requested reduced motion. `motion-reduce` applies styles when they have.

- `aria-label` is used for. You can describe elements, so instead of announcing button the screen reader announces Copy link, button.
```html
<button aria-label="Copy link">
  <IconLink />
</button>
```
When an element already has visible text, that text is automatically used by screen readers, so you don’t need to add an aria-label. What people see and what aria-label says should be in sync, because some people also use voice control to navigate.

For body text, the web’s default is 16px and it’s the perfect size for reading.
UI text can be smaller. I often use 14px for things like inputs and menus and 13px for captions, but I rarely go below 12px.

### Typography rules i follow (jakub)
- Stick to a type scale and give the sizes semantic names
- Use `16px` for long-form text, `14px` for UI and avoid going below `12px`
- Keep prose lines between `60ch` and `75ch`
- Use `text-balance` for headings and `text-pretty` for paragraphs
- Set `antialiased` on the body once so every text element inherits it
- Self-host fonts and use `font-display: swap` with a similar fallback
- Tighten `letter-spacing` on headings, loosen it on small uppercase labels and leave body text alone
- Use `tabular-nums` anywhere numbers change or need to line up
- Keep `line-height` tight on headings on larger sizes and relaxed on paragraphs
- Truncate or clamp text instead of letting it overflow
- Keep input text at `16px` on mobile to avoid the iOS zoom
- Meet `4.5:1` contrast for regular text and `3:1` for large text
- Style `::selection` so highlighted text matches your brand

Sometimes there might not be a font that matches perfectly and the fallback font can have a different x-height from the font you’re using. This can make the text appear to shrink or grow when the real font loads.
font-size-adjust scales the fallback to match the x-height of the font you’re loading. The value is based on the font’s x-height divided by its em size.
It’s a bit complicated but for Inter it’s equal to 0.545. If you pick fallbacks that already look similar, the jump is small enough that you rarely need to tweak this.
