# BUILD SPEC — Prem's Claude Code Quickstart

This is a handoff spec. The goal: take `content.md` (sibling file in this folder) and produce a single-file static HTML site, ready to drop into a GitHub repo and serve via GitHub Pages. No build pipeline, no framework, no npm. Hand-rolled HTML/CSS/vanilla JS.

**Read this entire spec before writing any code. Use plan mode. Surface any ambiguities before implementing.**

---

## 1. The deliverable

A single file: `index.html`, around 1500–3000 lines depending on choices. All CSS inline in a `<style>` block in the head. All JavaScript inline in a `<script>` block at the end of the body. No external dependencies except optionally one Google Font.

When the user opens this file in a browser, they see the full quickstart guide rendered as a calm, editorial-feeling web page. The content of the guide is converted from `content.md` into HTML at *build time* (i.e., right now, by you, Claude Code) — not at runtime in the browser. The HTML file ships with all content already baked in.

---

## 2. Content source

The source of truth is `content.md`. Every word of prose, every prompt, every code block in the rendered site comes from there.

### Section structure

Each section in `content.md` looks like:

```markdown
## Section N — Title
<!-- tag: TAG -->

[content here]
```

Where `TAG` is one of four values:

- `cc-only` — collapse for Lovable readers; show with a small marker
- `also-lovable` — works in Lovable too; show with a small marker
- `lovable-handles` — Lovable handles automatically; show with a small marker
- `meta` — no marker, never collapsed

Parts are H1 headers (`# Part N — Title`).

### OS-specific content

Inside section 4, install commands are wrapped:

```markdown
<!-- os: macos -->
[content for Mac]
<!-- /os -->

<!-- os: linux -->
[content for Linux]
<!-- /os -->

<!-- os: windows -->
[content for Windows]
<!-- /os -->
```

In the rendered page, only the block matching the user's selected OS is visible.

---

## 3. Visual design — calm and editorial

This is for a meditation community audience, mostly non-technical. The design must feel inviting and unhurried, not aggressively technical.

**Typography.**
- Body text: a serif. Source Serif 4, Lora, or similar. ~18px on desktop, ~17px on mobile. Line height 1.6–1.7. Generous.
- Headings: same serif, larger and bolder. H1 at 2.25rem, H2 at 1.5rem, H3 at 1.2rem. Headings have generous top margin (separation > attachment).
- Code: a clean monospace. JetBrains Mono, Fira Code, or system mono fallback. ~15px.

**Colors.**
- Background: warm off-white (`#fafaf7` or similar — not pure white, not gray).
- Body text: deep charcoal, not pure black (`#1a1a1a` or `#222`).
- Accent: a single muted color used sparingly. Suggest a deep teal or warm terracotta. Use for links, the toggle's "on" state, and the "Lovable" markers.
- Code blocks: slightly cooler background (`#f0efea`), monospace, generous padding.
- Inline code: same background, smaller padding, normal weight.

**Layout.**
- Centered single column, max-width ~720px on desktop. Generous side padding on mobile (~20px).
- A sticky top bar (small, unobtrusive) holding the OS picker and the Lovable toggle. ~50px tall. Subtle border-bottom, same warm off-white background with slight transparency/blur.
- Section markers ("Also works in Lovable" / "Claude Code only" / "Lovable handles this") appear as a small italic line directly under the section header, in the muted accent color. Easy to ignore if irrelevant, easy to spot if relevant.

**Mobile.**
Mobile is a first-class priority, not an afterthought. The site should be a pleasure to read on a phone:
- Single column, full-width minus 20px gutters.
- Sticky top bar collapses gracefully — OS picker becomes a small dropdown, Lovable toggle stays visible.
- Code blocks scroll horizontally on overflow rather than wrapping.
- Tap targets minimum 44px.

Test the layout at 375px wide (iPhone SE), 414px (typical modern phone), 768px (tablet), and 1200px+ (desktop). All four should look intentional.

---

## 4. Interactive features

Three features. Implement in this order — get each working before moving to the next.

### 4a. OS picker

A small dropdown or three-button segment control in the sticky top bar. Options: macOS / Linux / Windows.

**Behavior:**
- On first load, attempt to detect the user's OS from `navigator.userAgent` and select that. Fall back to macOS if uncertain.
- Selection persists across page loads via `localStorage` key `os-preference`.
- When the user changes OS, all `<!-- os: X -->` blocks in section 4 update to show only the matching one. Implement by giving each block a class like `os-block os-macos` and toggling `display: none` on non-matching ones.
- Smooth, no jarring reflow.

### 4b. "On Lovable instead?" toggle

A toggle switch in the sticky top bar, labeled "On Lovable instead?" Off by default.

**Behavior:**
- When ON: sections tagged `cc-only` are collapsed. Replace each with a small placeholder card that says: *"Section N — [Title]. Claude Code only — skip, or click to expand."* Clicking the card expands it inline.
- Sections tagged `also-lovable` and `lovable-handles` keep their existing markers but get no other change.
- Sections tagged `meta` never change.
- Selection persists via `localStorage` key `lovable-mode`.
- A single short banner appears just below the top bar when toggle is ON: *"You're seeing the Lovable view. Claude-Code-only sections are hidden — click any to expand."* Banner is dismissable but reappears next session.

### 4c. Copy buttons on code blocks

Every `<pre><code>` block in the rendered output gets a small "Copy" button in its top-right corner.

**Behavior:**
- Button is visually subtle by default — appears more clearly on hover (desktop) or always visible (mobile, since hover doesn't apply).
- Clicking copies the contents of the code block to clipboard via `navigator.clipboard.writeText()`.
- After successful copy, button text changes to "Copied!" for 1.5 seconds, then reverts.
- Falls back gracefully if clipboard API isn't available — show "Press Cmd+C / Ctrl+C" message.

---

## 5. Markdown to HTML conversion

You're converting `content.md` to HTML by hand (well, by code). Don't pull in a Markdown library — the content's structure is small and regular enough to handle directly.

**What you need to handle:**

- `# Header` → `<h1>`
- `## Header` → `<h2>` (these are sections)
- `### Header` → `<h3>`
- `**bold**` → `<strong>`
- `*italic*` → `<em>`
- Paragraphs → `<p>`
- Bullet lists (`- item`) → `<ul><li>...`
- Numbered lists (`1. item`) → `<ol><li>...`
- Inline code (`` `code` ``) → `<code>`
- Fenced code blocks (` ```lang ... ``` `) → `<pre><code class="language-lang">`
- Tables (Markdown pipe syntax) → `<table>` with proper `<thead>`/`<tbody>`
- Blockquotes (`> text`) → `<blockquote>`
- Horizontal rules (`---`) → `<hr>`
- Links (`[text](url)`) → `<a href="...">`
- HTML comments like `<!-- tag: cc-only -->` → consumed by the build (used for tagging), never rendered

**What you can ignore:**
- Setext-style headers (`===` underlines)
- Reference-style links
- Footnotes
- Raw HTML in the Markdown (other than the comments above)

**Section tagging:** when you encounter `<!-- tag: TAG -->` immediately after a section header, attach a `data-section-tag="TAG"` attribute to the section's wrapper element. The Lovable toggle script reads this attribute.

**OS blocks:** when you encounter `<!-- os: X -->...<!-- /os -->`, wrap the contents in a `<div class="os-block os-X">`. The OS picker script reads these classes.

---

## 6. Code highlighting

Don't pull in highlight.js or Prism. Keep code blocks as plain `<pre><code>` with the language as a class. The visual treatment is just monospace + background color — no syntax coloring. This fits the calm/editorial aesthetic better than rainbow code, and saves ~30KB of dependencies.

The exception: if the language is `bash`, `powershell`, or terminal-prompt-style, render the leading `> ` or `$` in a slightly muted color so the prompt visually separates from the command. This is optional; skip if it complicates things.

---

## 7. Navigation

A small sidebar/dropdown showing all sections, allowing one-click jump.

**Desktop:** a fixed sidebar on the right, showing the table of contents. Highlight the current section as the user scrolls (using `IntersectionObserver`). Section titles in the sidebar dim slightly when collapsed by the Lovable toggle.

**Mobile:** the sidebar would be a usability nightmare on mobile. Replace with a "Jump to..." button in the top bar that opens a full-screen overlay with the same TOC. Tap a section, overlay closes, page scrolls.

The skip-ahead banner from the content (the line near the top about navigating by Part) becomes redundant once the TOC is in place — keep it anyway, since some readers won't notice the TOC immediately.

---

## 8. File structure for the repo

When done, the repository should contain:

```
claude-code-quickstart/
├── index.html        # the entire site
├── content.md        # the source of truth (so contributors can edit Markdown and rebuild)
├── BUILD_SPEC.md     # this file (so future you can rebuild)
├── README.md         # short repo readme — what this is, how to contribute, deploy URL
└── .gitignore        # standard .gitignore, exclude editor cruft
```

If editing content later, the workflow is:

1. Edit `content.md`
2. Re-run the build (which regenerates `index.html` from `content.md`)
3. Commit and push
4. GitHub Pages auto-deploys

For step 2, also generate a small `build.sh` or `build.js` in the repo so future-Prem (or contributors) can rebuild without re-handing the whole spec to Claude Code. Keep the build script minimal — under 200 lines.

---

## 9. Deploying to GitHub Pages

After the file is built and committed:

1. Repo: `claude-code-quickstart` (public).
2. Settings → Pages → Deploy from branch → `main` / `/ (root)`.
3. URL will be `https://[github-username].github.io/claude-code-quickstart/`.
4. No custom domain for now — that's a later concern.

Add a section to the README explaining how to clone, edit, rebuild, and push. Keep it 5 lines.

---

## 10. Quality bar

Before claiming done, verify each of these:

**Content fidelity.**
- [ ] Every section in `content.md` is rendered.
- [ ] Section tags are correctly applied — collapse behavior works as specified.
- [ ] OS blocks correctly hide/show based on picker state.
- [ ] Code blocks preserve formatting, indentation, and special characters.
- [ ] Tables render correctly.
- [ ] No HTML comments leak into the rendered page.

**Visual quality.**
- [ ] At desktop width (1200px+): the page reads like a thoughtful publication.
- [ ] At tablet width (768px): layout adapts gracefully.
- [ ] At mobile width (375px): every section is readable, every interactive element is tappable.
- [ ] Typography hierarchy is clear without being shouty.
- [ ] Whitespace is generous, not cramped.

**Interactivity.**
- [ ] OS picker remembers selection across reloads.
- [ ] Lovable toggle remembers selection across reloads.
- [ ] Lovable toggle correctly collapses `cc-only` sections, leaves others alone.
- [ ] Collapsed sections can be individually expanded by clicking the placeholder.
- [ ] Copy buttons work on every code block, on both desktop and mobile.
- [ ] TOC highlights current section on scroll.
- [ ] No console errors, no layout shifts, no jank.

**Accessibility minimums.**
- [ ] Toggle and picker are keyboard-navigable.
- [ ] Color contrast meets WCAG AA for body text.
- [ ] Section landmarks (`<section>` elements with `aria-labelledby`).
- [ ] Toggle has proper `role` and `aria-checked`.

**Code quality.**
- [ ] HTML validates (no obvious errors — run through validator.w3.org).
- [ ] CSS is organized into sensible groups (typography / layout / components / responsive).
- [ ] JavaScript is split into clear functions, not one giant script. Small enough to read top-to-bottom in one sitting.
- [ ] No console.log statements in shipped code.

---

## 11. What to do if something is unclear

This spec was written before the file was built, so something will probably be ambiguous. When you hit ambiguity:

1. **Lean toward simplicity.** If the spec gives two options, pick the simpler one and note the choice in a comment.
2. **Lean toward calm.** If a design decision could go in two directions, pick the one that feels less aggressive — fewer animations, less color, more whitespace.
3. **Surface the ambiguity in your plan** before implementing. Don't silently make load-bearing decisions; flag them so Prem can correct course before you've written 500 lines.

---

## 12. The very first prompt to run after reading this

Before writing any code, run plan mode with this:

```
> [Shift+Tab to plan mode]
> I've read content.md and BUILD_SPEC.md. Walk me through
  your implementation plan. Specifically:
  1. How are you going to parse content.md into HTML?
  2. What's the structure of index.html — sections, scripts,
     CSS organization?
  3. What's the build script (build.sh or build.js) going to
     do, and where does it live?
  4. What ambiguities did you find in the spec, and what are
     you defaulting to?
  Don't write any code yet. I want to see the plan first.
```

Take Prem's feedback on the plan. Then build.

---

*— End of spec —*
