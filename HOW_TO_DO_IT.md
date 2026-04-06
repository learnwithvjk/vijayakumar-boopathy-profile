# HOW_TO_DO_IT — Recreate this single-file dark personal site (prompt + code recipe)

This repo’s `index.html` is a **single self-contained personal site** (no build tools) that delivers a polished “dark glass” aesthetic with:

- **Inline CSS design system** (CSS variables, gradients, glass panels, hover motion, responsive grids).
- **Tabbed navigation** (buttons toggle `.panel.active` sections).
- **Dark/Light theme toggle** using the `data-theme="light"` attribute and `localStorage` persistence.
- **Interactive skill pills** that render an accessible tooltip with star ratings and level labels.

Use this document as a **prompt playbook** to recreate the same kind of result (and to iterate/fine-tune it).

---

## Introduction — what you’re building (plan)

### Goal
Generate a single `index.html` that looks and behaves like this site:

- **Modern dark UI** with subtle glows and glass cards
- **Topbar** with brand, theme toggle, and tabs
- **4 panels**: About, Work (timeline), Projects (grid), Contact (enhanced contact card)
- **No frameworks**, no external CSS/JS (optional external links only)
- **Mobile responsive** and **reduced-motion friendly**

### Implementation blueprint (same structure as this `index.html`)
- **CSS**
  - `:root` variables for dark theme, and `[data-theme="light"]` override variables for light theme
  - Layout primitives: `.wrap`, `.topbar`, `.tabs`, `.tab`, `.panel`, `.card`
  - Visual polish: `backdrop-filter`, gradients, glow blobs, hover transitions, `@media` breakpoints, `prefers-reduced-motion`
  - Specialty: `.pill.skill` tooltip UI, `.contact-card` gradient border using masking
- **HTML**
  - Header with theme toggle button and tab buttons (`data-tab="about"`, etc.)
  - Panels: `<section class="panel" id="about"> ... </section>` etc.
- **JS**
  - Tabs: click handler toggles `.active` on tabs and panels
  - Theme: toggle `data-theme` and persist to `localStorage`
  - Skills: create tooltip nodes based on `data-rating` + `data-skill`

---

## Prompt limiters (critical constraints to get this result)

These are the constraints that keep the output “single-file, polished, and predictable”.

### Limiter prompt (paste as-is)

Use this as your **System** message (or prepend to every request).

```text
You are generating a single, self-contained HTML file (index.html) for a modern personal portfolio.
Hard constraints:
- Deliver exactly ONE file: index.html.
- Use only vanilla HTML/CSS/JS (no frameworks, no external dependencies).
- Put all CSS in a <style> tag and all JS in a <script> tag.
- Provide a dark theme by default and a light theme via [data-theme="light"] overrides.
- Implement tabbed navigation (4 tabs) by toggling .active classes on buttons and panels.
- Include a theme toggle button that persists to localStorage.
- Must be responsive (mobile-first) and support prefers-reduced-motion.
- Must be accessible: correct ARIA for tablist and tooltips, focus-visible styles, adequate contrast.
Quality constraints:
- Use a cohesive design system with CSS variables, subtle gradients, glassmorphism panels, and tasteful hover transitions.
- Keep markup semantic (<header>, <section>, <aside> where appropriate).
Output constraints:
- Output ONLY the final HTML file content; no explanations before/after.
```

### “No surprise” limiter add-ons (optional)

Add these when the model tends to wander:

```text
- Do not use canvas/WebGL or heavy animation libraries.
- Do not include analytics, trackers, or large inline SVG backgrounds beyond small icons.
- Keep total JS under ~120 lines and avoid complex state managers.
- Avoid images unless explicitly requested; prefer gradients/glows.
```

---

## Step-by-step prompt sequence (to achieve the same result reliably)

This is a **multi-prompt workflow** that mirrors how `index.html` is constructed: structure first, then style system, then behaviors, then polish.

### Step 1 — Define content + sections (skeleton)

```text
Create the semantic HTML skeleton for a single-file portfolio.
Sections: About, Work (timeline), Projects (4 cards), Contact (grid of 4 contact methods).
Include a topbar with brand on the left, theme toggle and tabs on the right.
Use class names: wrap, topbar, brand, tabs, tab, panel, card, hero-grid, grid, timeline, item, pill, contact-card.
Add placeholder copy that sounds like a senior backend/full-stack engineer.
Do not write CSS or JS yet—HTML only.
```

### Step 2 — Add the design system (CSS variables + cards + grids)

```text
Now add an inline <style> tag implementing a cohesive dark design system using CSS variables.
Requirements:
- :root defines --bg, --panel, --text, --muted, --line, --accent, --accent2, --shadow, --bg-grad, --glow1, --glow2.
- body uses background: var(--bg-grad) and a modern font stack.
- Implement wrap max-width ~1120px, topbar with border bottom, tabs with pill buttons.
- Implement card glass effect using background: var(--panel), border: 1px solid var(--line), border-radius ~22px, backdrop-filter blur, shadow.
- Implement responsive grids with breakpoints around 860px and 720px.
- Include prefers-reduced-motion: disable transitions/animations.
Output the full HTML with CSS included (still no JS).
```

### Step 3 — Implement interactions (tabs + theme toggle)

```text
Add a <script> tag with vanilla JS:
- Tabs: clicking a .tab removes .active from all tabs and .panel, then activates the matching panel by id from data-tab.
- Theme toggle: button id="themeToggle" toggles documentElement data-theme between light and dark.
- Persist theme to localStorage key "theme" and restore on page load.
- Toggle button icon should reflect theme (☀ for light, ◐ for dark).
Keep code small and readable.
Output the full final HTML.
```

### Step 4 — Add “skill pill tooltip” micro-interaction

This matches the existing logic: each `.pill.skill` gets a tooltip with stars + a label.

```text
Enhance the About panel with a skills row:
- Each skill is a <span class="pill skill" tabindex="0" data-skill="..." data-rating="1..5">Label</span>
Add JS that:
- For each .pill.skill[data-rating], inject a child <span class="skill-rating" role="tooltip"> ... </span>
- Render stars via '★' and '☆' based on rating (1..5)
- Add a mapping from rating to text levels: 1 Beginner, 2 Elementary, 3 Intermediate, 4 Proficient, 5 Expert, default Familiar
Add CSS for tooltip:
- Hidden by default (opacity: 0; transform translateY(4px))
- Visible on :hover and :focus-visible
- Styled glass tooltip with a small arrow ::before
Output the full final HTML.
```

### Step 5 — Add the “contact card” premium styling

The contact section in this `index.html` uses a gradient border via masking and a subtle radial glow overlay.

```text
Upgrade the Contact panel:
- Make the outer card use class "contact-card" with position: relative; overflow: hidden; padding ~26px.
- Add ::before that draws a 1px gradient border using mask-composite exclude technique.
- Add ::after for a radial highlight glow at the top.
- Inside, render a 2-column grid of contact items (email, LinkedIn, LeetCode, Resume download) that collapses to 1 column under 720px.
- Each contact item has an icon (small inline SVG), label, and value link with ellipsis overflow handling.
Output the full final HTML.
```

---

## The key code patterns (copy/paste snippets)

These are the “signature” patterns from your `index.html`. Use them as anchors when prompting.

### Tabs (HTML pattern)

```html
<div class="tabs" role="tablist" aria-label="Site sections">
  <button class="tab active" data-tab="about">About</button>
  <button class="tab" data-tab="work">Work</button>
  <button class="tab" data-tab="projects">Projects</button>
  <button class="tab" data-tab="contact">Contact</button>
</div>
```

```html
<section class="panel active" id="about">...</section>
<section class="panel" id="work">...</section>
<section class="panel" id="projects">...</section>
<section class="panel" id="contact">...</section>
```

### Tabs (JS pattern)

```js
const tabs = document.querySelectorAll('.tab');
const panels = document.querySelectorAll('.panel');
tabs.forEach(tab => tab.addEventListener('click', () => {
  tabs.forEach(t => t.classList.remove('active'));
  panels.forEach(p => p.classList.remove('active'));
  tab.classList.add('active');
  document.getElementById(tab.dataset.tab).classList.add('active');
}));
```

### Theme toggle (JS pattern)

```js
const themeToggle = document.getElementById('themeToggle');
const root = document.documentElement;

const stored = localStorage.getItem('theme');
if (stored === 'light' || stored === 'dark') root.setAttribute('data-theme', stored);

const syncToggleIcon = () => {
  themeToggle.textContent = root.getAttribute('data-theme') === 'light' ? '☀' : '◐';
};
syncToggleIcon();

themeToggle.addEventListener('click', () => {
  const next = root.getAttribute('data-theme') === 'light' ? 'dark' : 'light';
  root.setAttribute('data-theme', next);
  localStorage.setItem('theme', next);
  syncToggleIcon();
});
```

### Theme variables (CSS pattern)

```css
:root {
  --bg: #0b0f17;
  --panel: rgba(15,20,32,.72);
  --text: #f4f7fb;
  --muted: #a7b0c0;
  --line: rgba(255,255,255,.10);
  --accent: #7fb2ff;
  --accent2: #c5a06d;
  --shadow: 0 18px 60px rgba(0,0,0,.24);
  --bg-grad: radial-gradient(circle at top, rgba(72,104,164,.18), transparent 35%),
             linear-gradient(180deg, #0b0f17 0%, #090c12 100%);
}
[data-theme="light"] {
  --bg: #f6f2eb;
  --panel: rgba(255,255,255,.86);
  --text: #121212;
  --muted: #5f5b55;
  --line: rgba(18,18,18,.12);
  --accent: #1f4f82;
  --accent2: #7a5c3c;
}
```

---

## Fine-tuning prompts (iterate like a designer + engineer)

Use these when you want the same structure, but with improved polish, accessibility, or a different “vibe”.

### Fine-tune: spacing + typography

```text
Keep the layout and components the same, but refine typography and spacing:
- Improve vertical rhythm (consistent spacing scale: 6/10/14/18/22/26).
- Ensure headings have tight line-height and slightly negative letter-spacing.
- Keep body text readable: ~1.6 line-height, muted color for lede paragraphs.
Return the updated full HTML.
```

### Fine-tune: card depth + hover behavior (subtle)

```text
Reduce hover intensity so it feels premium:
- Decrease translateY hover from 4px to 2px.
- Reduce shadow spread/opacity.
- Ensure focus-visible styles are as strong as hover.
Return updated HTML.
```

### Fine-tune: accessibility (tabs + tooltips)

```text
Improve accessibility without changing visuals:
- Add role="tab" to tab buttons and aria-selected/aria-controls wiring.
- Panels should use role="tabpanel" and be focusable when activated.
- Ensure tooltip has an id and trigger uses aria-describedby.
Return updated HTML and keep JS simple.
```

### Fine-tune: performance + compatibility

```text
Make it more compatible/performant:
- Provide graceful fallback when backdrop-filter isn’t supported.
- Avoid expensive blur filters on large fixed glows on low-end devices (reduce blur radius / opacity).
- Keep prefers-reduced-motion strict (remove animations).
Return updated HTML.
```

### Fine-tune: content tailoring (swap persona)

```text
Keep the same UI structure, but rewrite copy for this persona:
[paste persona: role, industries, achievements, keywords]
Keep it concise, confident, and recruiter-friendly.
Return updated HTML only.
```

---

## “Prompt packs” you can reuse (copy/paste templates)

### Prompt pack A — Generate a fresh site in one shot (strict)

```text
Generate index.html only (single-file). Use the same structure and behaviors:
- Dark glass UI with light theme override via [data-theme="light"] and a theme toggle (localStorage).
- Tabbed panels: About, Work, Projects, Contact.
- Skill pills in About with tooltip stars + level labels rendered by JS.
- Contact card with premium gradient border and a 2-column contact grid.
- Responsive + prefers-reduced-motion.
Use semantic HTML, inline CSS, inline JS. Output ONLY the HTML file content.
```

### Prompt pack B — Modify safely (diff-friendly)

```text
I will paste my current index.html next.
Task: apply ONLY the requested change, preserving all existing structure and styles unless necessary.
Return the full updated index.html.
Change request:
- [describe exactly what to tweak: colors, spacing, content, add/remove a project card, etc.]
```

---

## Verification checklist (use after each iteration)

- **Tabs work**: only one `.panel.active` at a time; clicking tabs switches sections.
- **Theme works**: toggle switches and persists after refresh.
- **Mobile layout**: hero grid collapses; contact grid becomes 1 column under ~720px.
- **Reduced motion**: animations and transitions disabled when `prefers-reduced-motion: reduce`.
- **Keyboard UX**: tab buttons and skill pills reachable; focus styles visible; tooltips appear on focus.
- **Contrast**: muted text still readable on both themes.

