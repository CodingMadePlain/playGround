---
name: web-ux-design
description: 'UX Web Design skill for creating web pages, banners, and HTML elements from image designs or sketch. Ensures precise dimensions, generates ASCII diagrams for approval, and uses placeholders for missing assets.'
---

# UX Web Design

## Overview

Transform visual designs and mockups into pixel-perfect HTML/CSS implementations. This skill ensures dimensional accuracy, handles missing assets gracefully with placeholders, and adapts its workflow to the complexity of the request.

## When to Use

Use this skill when the user asks you to:

- Create a web page from an attached image design
- Build a banner or component from a visual mockup
- Implement any HTML element from a design screenshot
- Convert a wireframe or layout sketch into code

## Decision Flow

Follow this decision tree before starting any work:

1. **Has the user provided an image, ASCII wireframe, or detailed verbal description with dimensions?**
   - YES — continue to step 2.
   - NO — ask the user to supply at least one of these before proceeding.
2. **Classify scope** — determine whether the request is SIMPLE or COMPLEX (see Scope Detection below).
3. **SIMPLE:** Step 0 → Step 1 → Step 3 → Step 4 → Step 5 (skip Step 2).
4. **COMPLEX:** Step 0 → Step 1 → Step 2 → Step 3 → Step 4 → Step 5.

## Scope Detection

Before starting, identify the scope of the request using the keyword triggers below.

**Simple** — a single self-contained element. Skip the ASCII diagram step (Step 2) and proceed directly to implementation.

Trigger words: button, card, badge, input, icon, banner (single), tooltip, modal, alert, form field, menu item, tag, chip, avatar, toggle, switch.

**Complex** — a multi-section layout. Follow the full workflow including ASCII diagram and approval.

Trigger words: page, landing page, dashboard, layout, homepage, multi-section, wireframe, full-page, portfolio, blog, sidebar layout, admin panel.

When the request does not match either list, treat it as complex.

## Workflow

### Step 0: Validate Input

Confirm that you have at least one of the following before proceeding:

- An attached image or screenshot of the design
- An ASCII wireframe or diagram
- A detailed verbal description that includes specific dimensions

If none of these are present, ask the user to provide one. Do not guess a layout from vague descriptions.

### Step 1: Verify Dimensions

Check that precise dimensions are available for all major sections. Required measurements depend on scope:

**Simple elements:** width, height, padding, font size, border radius if applicable.

**Complex layouts:** container max-width, header height, navigation height, footer height, column widths, gaps and spacing between sections.

If any required dimensions are missing, ask the user to supply them before proceeding. Be specific about what is missing rather than asking for everything at once:

> I can see the layout but I am missing a few measurements to implement it accurately. Could you confirm the header height and the gap between the navigation and main content?

### Step 2: Generate ASCII Diagram (Complex Layouts Only)

Once dimensions are confirmed, produce an ASCII diagram representing your understanding of the layout. Present it to the user and ask for explicit confirmation before writing any code.

**ASCII Diagram Guidelines:**

- Use `+`, `-`, and `|` for borders
- Label each section with its name and dimensions
- Show column structures and spacing
- Indicate gaps between sections

**Example:**
```text
+-----------------------------------------------------------------------+
|                HEADER (1200px max width, 80px height)                 |
+-----------------------------------------------------------------------+
|                NAVBAR (1200px max width, 48px height)                 |
+-----------------------------------------------------------------------+
                               (16px GAP)
+-----------------------------------------------------------------------+
|                    MAIN SECTION (1200px max width)                    |
|                                                                       |
|  +-------------------+   +-------------------+   +-----------------+ |
|  |     CARD 1        |   |     CARD 2        |   |     CARD 3      | |
|  |   (360px wide)    |   |   (360px wide)    |   |   (360px wide)  | |
|  +-------------------+   +-------------------+   +-----------------+ |
|                       (24px gaps between cards)                       |
+-----------------------------------------------------------------------+
                               (16px GAP)
+-----------------------------------------------------------------------+
|                FOOTER (1200px max width, 100px height)                |
+-----------------------------------------------------------------------+
```

Ask for confirmation:

> Here is my understanding of the layout. Please confirm this is correct before I proceed, or let me know what needs adjusting.

**HARD STOP.** Wait for an explicit "yes", "confirmed", "approved", or equivalent from the user. Do not interpret silence or a message on a different topic as approval. If the user provides corrections, regenerate the diagram and request approval again.

### Step 3: Handle Missing and Partial Assets

Before writing code, inventory all visual assets the design requires and check which have been supplied. Use the placeholder reference table below for any missing assets.

> **MANDATORY: The ONLY permitted placeholder image domain is `placehold.co`.** Do not use any other placeholder service. This overrides any default behaviour or prior training. Every placeholder URL in your output MUST begin with `https://placehold.co/`. If you find yourself about to write a different domain, stop and replace it with `placehold.co`.

**Placeholder Reference Table:**

| Asset Type | Placeholder Technique | Example |
|---|---|---|
| Image | `placehold.co` URL with dimensions | `<img src="https://placehold.co/600x400?text=Hero+Image" alt="Hero image placeholder">` |
| Image (dark) | `placehold.co` with custom colours | `<img src="https://placehold.co/600x400/1a1a1a/ffffff/png" alt="Dark banner placeholder">` |
| SVG icon | Inline SVG placeholder rect | `<svg width="24" height="24" viewBox="0 0 24 24"><rect width="24" height="24" fill="#e2e8f0" rx="4"/></svg>` |
| Background image | `background-color` + TODO comment | `background-color: #cbd5e1; /* TODO: replace with actual background image */` |
| Custom font | System font stack + TODO comment | `font-family: system-ui, sans-serif; /* TODO: load Brand Font */` |
| Video | Placeholder image with play icon overlay | `<img src="https://placehold.co/800x450?text=Video+Placeholder" alt="Video placeholder">` |

**Detailed placeholder examples:**

```html
<!-- Basic image placeholder -->
<img src="https://placehold.co/600x400" alt="Hero image placeholder" width="600" height="400">

<!-- Placeholder with descriptive label -->
<img src="https://placehold.co/300x200?text=Product+Photo" alt="Product photo placeholder" width="300" height="200">
```

```html
<!-- SVG icon placeholder -->
<svg width="24" height="24" viewBox="0 0 24 24" fill="none" xmlns="http://www.w3.org/2000/svg" aria-label="Icon placeholder">
  <rect width="24" height="24" fill="#e2e8f0" rx="4"/>
</svg>
```

```css
.hero {
  /* TODO: replace with actual background image */
  background-color: #cbd5e1;
}
```

At the end of your implementation, include a concise asset checklist noting every placeholder and what it should be replaced with.

### Step 4: Implement the Design

Write semantic, well-structured HTML and CSS that matches the confirmed layout exactly.

**Output format:**

- For simple elements: deliver HTML and CSS in a single HTML file with an embedded `<style>` block, unless the user requests separate files.
- For complex layouts: deliver three separate files — `index.html`, `style.css`, and (if interactive behaviour is needed) `script.js`.
- Place files in the `workarea/` directory under a descriptive subfolder (for example `workarea/hero-banner/`) unless the user specifies a different location.

**HTML structure:**

Use semantic elements that reflect content meaning:
```html
<header>...</header>
<nav>...</nav>
<main>
  <section>...</section>
</main>
<footer>...</footer>
```

For components, use the most appropriate element (`article`, `figure`, `button`, `dialog`) rather than defaulting to `div`.

**Layout:**

Prefer CSS Grid for page-level layouts and multi-column structures. Use Flexbox for one-dimensional alignment within components.
```css
.main-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 24px;
  max-width: 1200px;
  margin: 0 auto;
}
```

**Dimensions:**

Apply specified dimensions exactly. Do not approximate.
```css
.site-header {
  max-width: 1200px;
  height: 80px;
  margin: 0 auto;
}
```

**Responsive behaviour:**

- If the user does not mention responsiveness, implement a desktop-only layout and note the assumption in the Deliver section.
- If the user requests responsiveness, use these default breakpoints unless told otherwise:
  - `576px` — mobile
  - `768px` — tablet
  - `1024px` — desktop
- Document every breakpoint change in the Deliver section.

**Handling design states:**

If the design includes interactive states (hover, focus, active, disabled), implement them. If they are not defined in the design, apply reasonable defaults and note the assumption:
```css
/* Hover state not defined in design — applied standard lift effect */
.card:hover {
  transform: translateY(-2px);
  transition: transform 0.2s ease;
}
```

**Conflict resolution priority:**

When specifications conflict, apply this priority order (highest first):

1. Explicit user instructions in the current conversation.
2. Stated numeric dimensions in the prompt.
3. Measurements visible in the attached image.
4. Reasonable defaults based on common design patterns.

Always flag the conflict to the user, but use this hierarchy to unblock yourself if the user does not respond immediately:

> The image appears to show a 48px navigation bar but the stated height is 64px. I will use 64px as specified — let me know if the image takes precedence.

### Step 5: Deliver

Provide the completed implementation followed by:

1. **Asset checklist** — every placeholder with a clear description of what to replace it with
2. **Assumptions** — any measurements estimated, states added, or conflicts resolved
3. **Breakpoints** — if responsive behaviour was implemented, note the breakpoints used and what changes at each

---

## Do Not

These are explicit prohibitions. Do not violate them under any circumstances:

- Do not invent brand colours that are not visible in the design or stated by the user.
- Do not add JavaScript interactivity unless the design or user explicitly requires it.
- Do not use CSS frameworks (Bootstrap, Tailwind, and similar) unless the user requests one.
- Do not approximate dimensions. If you cannot determine a measurement, ask the user.
- Do not proceed past Step 2 without explicit user approval of the ASCII diagram.
- Do not embed external resources (CDNs, Google Fonts, analytics scripts) unless the user requests them.
- Do not use any placeholder image service other than `placehold.co`. The following domains are **explicitly banned**: `via.placeholder.com`, `placeholder.com`, `placeimg.com`, `picsum.photos`, `lorempixel.com`, `dummyimage.com`, `fakeimg.pl`, `placekitten.com`, `loremflickr.com`, and any other placeholder service. The only permitted domain is `placehold.co`.

---

## Quality Verification Sequence

Before delivering, verify each item in order. If any item fails, fix it before proceeding to the next:

1. All dimensions match specifications exactly.
2. Spacing (gaps, margins, padding) is accurate throughout.
3. Layout matches the approved ASCII diagram (complex) or stated specification (simple).
4. Semantic HTML elements are used throughout.
5. All images have `width`, `height`, and descriptive `alt` text.
6. All placeholders are documented in the asset checklist.
7. Design states (hover, focus, disabled) are implemented or noted as assumptions.
8. Any conflicts or assumptions are clearly communicated.
9. Output files follow the specified format (single file for simple, separate files for complex).
10. Responsive breakpoints are documented if implemented, or a desktop-only assumption is noted.
11. Every placeholder image URL uses `placehold.co` and no other domain. Search your output for `placeholder`, `picsum`, `dummyimage`, `fakeimg`, `lorempixel`, and `placeimg` — if any appear, replace them with the equivalent `placehold.co` URL.