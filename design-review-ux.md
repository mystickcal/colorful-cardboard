# ColorfulCardboard.com — UX & Information Architecture Review (Hugo + PaperMod)

Scope reviewed:
- Screenshots: `cc-homepage.jpeg`, `cc-article.jpeg`
- Layouts: `layouts/index.html`, `layouts/_default/single.html`, `layouts/_default/list.html`
- Config: `hugo.toml`

---

## 1) NAVIGATION: 8 items is too many → consolidate to max 5

### What’s happening now
`hugo.toml` defines 8 top-level menu items:
- Start Here
- Market Analysis
- Card Picks
- Set Reviews
- Beginner's Guide
- Guides
- Tools
- About

This creates decision fatigue, and several items overlap (Start Here vs Beginner’s Guide vs Guides).

### Recommended max-5 nav
1. **Start Here** (primary onboarding hub)
2. **Analysis** (umbrella for Market Analysis + Card Picks + Set Reviews)
3. **Guides** (umbrella for Beginner’s Guide + Guides)
4. **Tools**
5. **About**

### Specific code changes (Hugo menu)
In `hugo.toml`, replace the current `[[menu.main]]` entries with (example):

```toml
[[menu.main]]
name = "Start Here"
url = "/start-here/"
weight = 1

[[menu.main]]
name = "Analysis"
url = "/categories/"
weight = 2

[[menu.main]]
name = "Guides"
url = "/guides/"
weight = 3

[[menu.main]]
name = "Tools"
url = "/tools/"
weight = 4

[[menu.main]]
name = "About"
url = "/about/"
weight = 5
```

Notes:
- `/categories/` can be a landing page (taxonomy terms) that explains the 3 content pillars and links to each.
- If you still want direct access to Market Analysis, Card Picks, Set Reviews, add them as **in-page links** on the homepage filter row (already present) and on the `/categories/` landing page.

---

## 2) ARTICLE PAGE STRUCTURE: title above cover + cover is too large

### What’s happening now
In `layouts/_default/single.html`, cover renders **before** the title:

```go-html-template
<header class="post-header">
  {{ if .Params.cover }}
    {{ partial "cover.html" ... }}
  {{ end }}

  <h1 class="post-title">{{ .Title }}</h1>
  {{ partial "post_meta.html" . }}
</header>
```

In the screenshot, the cover dominates the viewport and the title is not visible above it.

### Recommended structure
- Title + meta first
- Smaller cover second
- Optional: place cover inside its own wrapper so it can be constrained via CSS

### Specific code changes (single.html)
Move the `<h1>` and meta above the cover, and wrap the cover:

```go-html-template
<header class="post-header cc-post-header">
  <h1 class="post-title">{{ .Title }}</h1>
  {{- partial "post_meta.html" . -}}

  {{- if .Params.cover -}}
    <div class="cc-cover">
      {{- partial "cover.html" (dict "cxt" . "IsHome" false "isHidden" false) -}}
    </div>
  {{- end -}}
</header>
```

### Specific code changes (CSS)
Add constraints for the cover so it doesn’t take over the fold:

```css
/* Example: in assets/css/extended/cc.css (or whatever file you’re using) */
.cc-cover img {
  max-height: 360px;
  width: 100%;
  object-fit: cover;
}

/* Reduce dead space between nav and content if PaperMod adds it */
.cc-single .post-header { margin-top: 0; padding-top: 1rem; }
```

If PaperMod’s cover partial outputs a `figure` or `.post-cover`, target that instead:

```css
.post-single .post-cover img { max-height: 360px; object-fit: cover; }
```

---

## 3) CONTENT HIERARCHY (Homepage): hero → filter → articles → newsletter

### What’s working
- The intended journey is sensible.
- Category filter pills are a strong pattern for returning visitors.
- Grid cards show date/category/title/description (good scanning).

### What’s confusing / hurting clarity
**A) Hero legibility + value clarity**
The hero text sits on a busy image without sufficient overlay. The headline repeats the brand name (“Colorful Cardboard”) instead of telling users what they get.

**B) Redundant branding**
Header already shows “Colorful Cardboard”; hero repeats it again as the biggest text on page.

**C) Filter pills are not true navigation**
The pills filter posts via JS by matching `data-cat` to the first category. This creates problems:
- Categories displayed are dependent on `.Params.categories[0]` only.
- No URL changes → not shareable, not indexable.
- If category casing differs (`Market Analysis` vs `market-analysis`), filtering can silently fail.

### Specific code changes (homepage hero)
In `layouts/index.html`, change:

```html
<h1 class="cc-hero__title">Colorful Cardboard</h1>
```

to something value-led, and keep the brand in the header only:

```html
<h1 class="cc-hero__title">Data-driven Pokémon card investing</h1>
<p class="cc-hero__tagline">Market trends, undervalued picks, and set breakdowns—so you can buy with conviction.</p>
```

(Then remove the current kicker or repurpose it.)

### Specific code changes (hero contrast)
Add a gradient overlay via CSS using the existing `.cc-hero`:

```css
.cc-hero {
  position: relative;
  background-image: var(--cc-hero-bg);
  background-size: cover;
  background-position: center;
}
.cc-hero::before {
  content: "";
  position: absolute;
  inset: 0;
  background: linear-gradient(90deg, rgba(0,0,0,.65) 0%, rgba(0,0,0,.35) 45%, rgba(0,0,0,.15) 100%);
}
.cc-hero__inner { position: relative; z-index: 1; }
```

### Make the filter row act as navigation (optional but recommended)
Instead of JS filtering only, convert pills to links (indexable) OR keep JS but add a “View category” link.

**Option A (best for SEO): use links**
Replace each `<button>` with `<a>` to category pages:

```html
<a class="cc-pill" href="/categories/market-analysis/">Market Analysis</a>
```

**Option B: keep JS + add “See all in category”**
Add a link that updates when a pill is selected.

---

## 4) MOBILE EXPERIENCE: responsive issues to address

### Observed risk areas
- **Top nav has 8 items** (currently) → will wrap/collapse poorly on smaller widths.
- **Hero has two CTAs side-by-side** → likely wraps awkwardly.
- **Filter pill bar** may overflow horizontally.
- **Post grid** needs a clean 1-col layout on small screens.

### Specific code changes (CSS breakpoints)
Ensure:
- Hero CTAs stack
- Filter pills scroll horizontally
- Grid becomes 1 column

Example:

```css
@media (max-width: 768px) {
  .cc-hero__cta { display: grid; gap: .75rem; }
  .cc-hero__cta a { width: 100%; text-align: center; }

  .cc-filter__bar {
    overflow-x: auto;
    -webkit-overflow-scrolling: touch;
    gap: .5rem;
    padding-bottom: .5rem;
  }

  .cc-post-grid { grid-template-columns: 1fr; }
}
```

Also ensure tap targets:

```css
.cc-pill { padding: .6rem .9rem; }
```

---

## 5) DARK MODE: remove toggle + force light

### Recommendation
If the goal is a “Riverside.fm”-style bright editorial feel, forcing light mode is reasonable. Dark mode also conflicts with the bright, colorful brand imagery.

### Specific code changes
**A) Disable theme toggle in PaperMod**
Add to `hugo.toml`:

```toml
[params]
  defaultTheme = "light"
  disableThemeToggle = true
```

**B) Ensure no auto dark theme**
If present in PaperMod version:

```toml
[params]
  disableScrollToTop = false # unrelated; just don’t add night mode scripts
```

(Primary keys are `defaultTheme` and `disableThemeToggle`.)

---

## 6) REDUNDANCY: “Colorful Cardboard” in header AND hero

### What’s happening now
- Header label: `params.label.text = "Colorful Cardboard"`
- Hero H1 also: “Colorful Cardboard”

### Recommendation
Keep brand identity in the header. Use hero H1 for value proposition.

### Specific code change
In `layouts/index.html`, change hero `<h1>` from brand name to value prop (see Issue #3).

If you want a softer brand nod, make it a small line:

```html
<p class="cc-hero__brand">Colorful Cardboard</p>
```

…and style it subtle.

---

## 7) CATEGORY PAGES: currently “just lists” → make them conversion + browsing hubs

### What’s happening now
Taxonomy list pages use `layouts/_default/list.html` which prints title/description and then a grid of posts.

### Recommendation
For each major category (Market Analysis, Card Picks, Set Reviews, Beginner’s Guide), add:
- A short “What you’ll learn / Who it’s for” intro
- Links to the 3–6 best evergreen posts (“Start with these”)
- Optional: newsletter CTA tailored to that category

### Specific code changes
**A) Create taxonomy term template** (more control than `_default/list.html`):
Create:
- `layouts/_default/terms.html` (for `/categories/`)
- `layouts/_default/taxonomy.html` (for `/categories/<term>/`)

Then in `taxonomy.html` add a featured section. Example approach:
- In front matter of key posts: `featuredIn = ["market-analysis"]`
- Or use Hugo `where` on a param like `weight` / `pinned`.

Pseudo:

```go-html-template
{{ $term := .Title | urlize }}
{{ $featured := where .Pages "Params.featured" true }}
```

**B) At minimum: improve list header in existing `list.html`**
Add a CTA block after `.page-description`:

```go-html-template
<div class="cc-taxonomy-cta">
  <a class="cc-btn cc-btn--primary" href="/start-here/">New? Start here</a>
  <a class="cc-btn cc-btn--ghost" href="#cc-posts">Browse posts</a>
</div>
```

---

## 8) INTERNAL LINKING: improve SEO and user flow

### Recommendation
Add structured internal linking at three levels:
1. **Within articles**: “Related posts” module based on same category.
2. **Category hubs**: “Best of” lists.
3. **Inline links**: editorial links in intros/outros.

### Specific code changes
**A) Add related posts partial to `single.html`**
After `.Content` (before post nav links), insert:

```go-html-template
{{- $cat := index (.Params.categories | default (slice)) 0 -}}
{{- if $cat -}}
  {{- $related := first 4 (where (where site.RegularPages "Type" "in" (slice "posts" "post")) "Params.categories" "intersect" (slice $cat)) -}}
  {{- if gt (len $related) 1 -}}
  <aside class="cc-related">
    <h2>Related</h2>
    <ul>
      {{- range $related -}}
        {{- if ne .RelPermalink $.RelPermalink -}}
          <li><a href="{{ .RelPermalink }}">{{ .Title }}</a></li>
        {{- end -}}
      {{- end -}}
    </ul>
  </aside>
  {{- end -}}
{{- end -}}
```

**B) Add CSS**

```css
.cc-related { margin-top: 2.5rem; padding-top: 1.5rem; border-top: 1px solid var(--border); }
.cc-related ul { margin: .75rem 0 0; padding-left: 1.2rem; }
```

---

## 9) CALL-TO-ACTION: newsletter form is placeholder — when to wire it up?

### Recommendation
Wire it up *before* you push traffic (YouTube/social) because:
- Newsletter is the primary retention loop for a content site.
- A placeholder form damages trust (“wiring soon”).

### Specific code changes
In `layouts/index.html`, replace:

```html
<form class="cc-newsletter__form" action="#" method="post">
```

with your email provider endpoint (ConvertKit/Kit example):

```html
<form class="cc-newsletter__form" action="https://YOUR-CONVERTKIT-FORM-URL" method="post">
```

Also remove placeholder fine print:

```html
<small class="cc-newsletter__fine">Placeholder form — wiring coming soon.</small>
```

Replace with trust text:

```html
<small class="cc-newsletter__fine">No spam. Unsubscribe anytime.</small>
```

If you want to keep it non-functional temporarily, hide the form and use a simple button:

```html
<a class="cc-btn cc-btn--primary" href="/newsletter/">Join the newsletter</a>
```

---

## 10) PAGE SPEED: remove unnecessary JS/CSS and reduce render cost

### Current cost centers observed
- Homepage inline JS for client-side filtering.
- Article page inline JS for scroll progress bar.
- Large hero background image (likely heavy), plus large cover images.

None of these are inherently “bad,” but for a content site you want to minimize JS and ship smaller images.

### Recommendations + specific code changes
**A) Replace homepage JS filtering with links (best)**
As noted in Issue #3, convert pills from `<button>` to `<a>` and delete the inline `<script>` block in `layouts/index.html` entirely.

**B) Make scroll progress optional**
In `layouts/_default/single.html`, the progress script runs on all posts. Either:
- Remove the entire progress bar block, or
- Gate it behind a param:

```go-html-template
{{ if site.Params.showReadingProgress }}
  ...progress markup/script...
{{ end }}
```

Then in `hugo.toml`:

```toml
[params]
  showReadingProgress = false
```

**C) Compress & constrain images**
- Hero: use a smaller JPG/WebP and consider `image-set()` with WebP.
- Cover: constrain height (Issue #2) and ensure images are optimized.

**D) Remove unused PaperMod outputs if not needed**
You currently have:

```toml
[outputs]
home = ["HTML", "RSS", "JSON"]
```

If you’re not using JSON for search, remove it:

```toml
[outputs]
home = ["HTML", "RSS"]
```

(Only do this if you confirmed PaperMod search is disabled.)

---

# Quick punch list (highest impact)
1. **Move H1 above cover** in `single.html` + cap cover height in CSS.
2. **Reduce top nav to 5 items** in `hugo.toml`.
3. **Fix hero contrast** with overlay + change hero H1 to value prop.
4. **Convert filter pills to links** (SEO + no JS).
5. **Disable dark mode toggle** and force light theme.
6. **Wire newsletter form** before promoting.
