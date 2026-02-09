# Implementation Guide - Colorful Cardboard Design System

## Quick Start

The custom CSS is already in place at `assets/css/extended/custom.css`. Hugo PaperMod will automatically load this file.

---

## Step 1: Verify CSS Loading

1. Start Hugo dev server:
```bash
cd /mnt/d/AiMe/03_PROJECTS/colorful-cardboard
hugo server -D
```

2. Open browser to `http://localhost:1313`

3. Inspect page and verify custom CSS variables are loaded:
   - Open DevTools → Elements → Styles
   - Look for `:root` with `--brand-yellow`, `--brand-blue`, etc.

---

## Step 2: Add Category Badges to Posts

To make the category badges and colored borders appear on post cards, you need to add a custom partial template.

### Create Category Badge Partial

**File:** `layouts/partials/post-card.html`

```html
{{- $class := "post-entry" }}
{{- $user_preferred := or site.Params.disableSpecial1stPost site.Params.homeInfoParams }}
{{- if (and (.IsHome) (.Param "cover.hidden") (not $user_preferred)) }}
{{- $class = "post-entry-inline" }}
{{- end }}

{{- $category := "" }}
{{- if .Params.categories }}
{{- $category = index .Params.categories 0 | lower }}
{{- $category = replace $category " " "-" }}
{{- end }}

<article class="{{ $class }}" data-category="{{ $category }}">
  {{- $isHidden := (.Param "cover.hidden") }}
  {{- $cover := (.Param "cover.image") }}
  {{- if (and $cover (not $isHidden)) }}
  <figure class="entry-cover">
    {{- $alt := (.Param "cover.alt") }}
    {{- $caption := (.Param "cover.caption") }}
    {{- $relative := (.Param "cover.relative") }}
    {{- $linkFullImages := (.Param "cover.linkFullImages") }}
    {{- $addLink := (and $cover $linkFullImages) }}
    {{- if $addLink }}<a href="{{ $cover | absURL }}" target="_blank" rel="noopener noreferrer">{{ end }}
    <img loading="lazy" src="{{ $cover | absURL }}" alt="{{ $alt }}" />
    {{- if $addLink }}</a>{{ end }}
    {{- if or $caption $alt }}
    <p>{{ with $caption }}{{ . }}{{ else }}{{ $alt }}{{ end }}</p>
    {{- end }}
  </figure>
  {{- end }}
  <header class="entry-header" data-category="{{ $category }}">
    <h2>
      {{- .Title }}
    </h2>
  </header>
  {{- if (.Param "ShowSummary") }}
  <div class="entry-content">
    <p>{{ .Summary | plainify | htmlUnescape }}{{ if .Truncated }}...{{ end }}</p>
  </div>
  {{- end }}
  {{- if not (.Param "hideMeta") }}
  <footer class="entry-footer">
    {{- partial "post_meta.html" . -}}
  </footer>
  {{- end }}
  <a class="entry-link" aria-label="post link to {{ .Title | plainify }}" href="{{ .Permalink }}"></a>
</article>
{{- end }}
```

### Update Hugo Config

**File:** `hugo.toml`

Add this parameter to show summaries on homepage:

```toml
[params]
  # ... existing params ...
  ShowSummary = true
  summaryLength = 150
```

---

## Step 3: Create Example Posts

### Market Analysis Post Example

**File:** `content/posts/market-analysis-example.md`

```markdown
---
title: "January 2026 Pokemon Card Market Report"
date: 2026-01-15
categories: ["Market Analysis"]
tags: ["market trends", "vintage", "modern"]
description: "Deep dive into Q4 2025 market performance and predictions for 2026"
ShowToc: true
TocOpen: false
draft: false
---

## Executive Summary

The Pokemon TCG market showed resilience in Q4 2025 with vintage sealed products appreciating 23% year-over-year...

## Vintage Market Performance

### Base Set Sealed Products

| Product | Q4 2024 | Q4 2025 | Change |
|---------|---------|---------|--------|
| Booster Box | $42,000 | $51,660 | +23% |
| Booster Pack | $850 | $1,020 | +20% |

**Key Takeaway:** Base Set continues to be the blue-chip investment of the hobby.

## Modern Market Analysis

Scarlet & Violet sets are showing early strength...
```

---

### Card Picks Post Example

**File:** `content/posts/undervalued-picks-february.md`

```markdown
---
title: "5 Undervalued Cards to Buy in February 2026"
date: 2026-02-08
categories: ["Card Picks"]
tags: ["undervalued", "investment", "modern"]
description: "These cards are flying under the radar but showing strong fundamentals"
draft: false
---

## 1. Umbreon VMAX (Evolving Skies)

**Current Price:** $85 PSA 10  
**Target Price (12mo):** $150+

### Why It's Undervalued

- Iconic Pokemon with strong collector demand
- Evolving Skies is under-opened relative to other SWSH sets
- Alt art version at $600+ creates halo effect
```

---

### Set Reviews Post Example

**File:** `content/posts/prismatic-evolutions-set-review.md`

```markdown
---
title: "Prismatic Evolutions Set Review: Investment Potential"
date: 2026-02-01
categories: ["Set Reviews"]
tags: ["prismatic evolutions", "set review", "eevee"]
description: "Complete breakdown of chase cards, box EV, and long-term investment thesis"
draft: false
---

## Set Overview

**Release Date:** January 17, 2026  
**Set Size:** 191 cards + secret rares  
**Theme:** Eeveelutions Special Set

## Chase Cards Analysis

### Tier 1: Must-Haves

1. **Umbreon Moonlight SIR**
   - Pull rate: 1:500 packs
   - Current price: $280
   - Investment grade: A+
```

---

### Beginner's Guide Post Example

**File:** `content/posts/how-to-start-investing-in-pokemon-cards.md`

```markdown
---
title: "How to Start Investing in Pokemon Cards (2026 Beginner's Guide)"
date: 2026-02-05
categories: ["Beginner's Guide"]
tags: ["beginner", "getting started", "basics"]
description: "Everything you need to know to start building a Pokemon card portfolio"
ShowToc: true
TocOpen: true
draft: false
---

## Welcome, Future Collector!

Thinking about investing in Pokemon cards? You're in the right place. This guide will walk you through everything you need to know to get started safely and strategically.

## Step 1: Set Your Budget

**Rule of thumb:** Only invest money you can afford to lock away for 5-10 years.

### Recommended Starting Amounts

| Budget | Strategy |
|--------|----------|
| $100-500 | Singles only, modern chase cards |
| $500-2000 | Mix of singles + 1-2 booster boxes |
| $2000+ | Diversified portfolio with vintage |

## Step 2: Understand Card Grading

The difference between a raw card and a PSA 10 can be 10x the value...

### What is PSA?

Professional Sports Authenticator (PSA) is the gold standard for card grading...
```

---

## Step 4: Add Category Descriptions

Create archive pages with descriptions for each category.

**File:** `layouts/_default/terms.html`

```html
{{- define "main" }}

{{- $category := .Data.Singular }}
{{- $categoryName := .Title }}

{{- if .Params.description }}
<header class="archive-header" data-category="{{ .Data.Term | urlize }}">
  <h1>{{ .Title }}</h1>
  <div class="archive-description">{{ .Params.description }}</div>
</header>
{{- end }}

{{- $paginator := .Paginate .Pages }}

{{- range $paginator.Pages }}
  {{- partial "post-card.html" . }}
{{- end }}

{{- if gt $paginator.TotalPages 1 }}
<footer class="page-footer">
  <nav class="pagination">
    {{- partial "pagination.html" . }}
  </nav>
</footer>
{{- end }}

{{- end }}{{/* end main */}}
```

---

## Step 5: Customize Logo Text

If you want to change the logo from the default Pokéball symbol:

**File:** `hugo.toml`

```toml
[params]
  # ... existing params ...
  
  [params.label]
    text = "Colorful Cardboard"
    icon = "◉"  # Pokéball symbol
    iconHeight = 28
```

Or to customize the logo completely, create:

**File:** `layouts/partials/logo.html`

```html
<a href="/" class="logo" aria-label="Colorful Cardboard">
  <span class="logo-icon">◉</span>
  <span class="logo-text">Colorful Cardboard</span>
</a>
```

---

## Step 6: Add Social Icons

Update your social icons to match the design:

**File:** `hugo.toml`

```toml
[[params.socialIcons]]
  name = "youtube"
  url = "https://youtube.com/@omgitsderek"

[[params.socialIcons]]
  name = "twitter"  
  url = "https://x.com/omgitsderek"

[[params.socialIcons]]
  name = "reddit"
  url = "https://reddit.com/r/PokeInvesting"  # If you create a subreddit
```

---

## Step 7: Create About Page

**File:** `content/about.md`

```markdown
---
title: "About Colorful Cardboard"
layout: "single"
ShowToc: false
---

## Our Mission

Colorful Cardboard exists to help Pokemon card collectors make informed investment decisions through data-driven analysis and market research.

## Who We Are

We're Pokemon fans who grew up collecting cards and rediscovered the hobby as adults. When we saw the investment potential, we decided to share our research with the community.

## Our Approach

1. **Data-Driven:** We track market prices, sales volume, and trends
2. **Transparent:** We share our methodology and sources
3. **Balanced:** We highlight both opportunities and risks
4. **Community-Focused:** We're building this together

## Not Financial Advice

This website is for educational and entertainment purposes. We are not financial advisors. Pokemon cards are speculative collectibles. Never invest more than you can afford to lose.

## Contact

Questions? Suggestions? Reach out on [Twitter](https://x.com/omgitsderek) or email colorfulcardboard@gmail.com
```

---

## Step 8: Test Category Styling

Create one post in each category and verify:

1. **Category badges appear** with correct colors
2. **Left border shows on hover** with category color
3. **Archive headers** have gradient backgrounds
4. **Navigation highlights** current category

Test both light and dark modes.

---

## Step 9: Performance Optimization

### Enable WebP Images

**File:** `hugo.toml`

```toml
[imaging]
  quality = 85
  resampleFilter = "Lanczos"
  
  [imaging.exif]
    disableDate = false
    disableLatLong = true
```

### Add Preload for Fonts

**File:** `layouts/partials/head.html` (create if doesn't exist)

```html
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link rel="preload" as="style" href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&family=Space+Grotesk:wght@500;600;700&display=swap">
```

---

## Step 10: Deploy

### Build Site

```bash
hugo --minify
```

### Deploy to Netlify/Vercel

1. Connect GitHub repo
2. Set build command: `hugo --minify`
3. Set publish directory: `public`
4. Add environment variable: `HUGO_VERSION = 0.134.0` (or latest)

### Custom Domain

Point DNS to hosting:
- A record: `colorfulcardboard.com` → hosting IP
- CNAME: `www.colorfulcardboard.com` → hosting domain

---

## Troubleshooting

### CSS Not Loading

**Issue:** Custom styles not appearing

**Fix:**
1. Verify file is at `assets/css/extended/custom.css` (not `static/`)
2. Clear Hugo cache: `hugo --gc`
3. Hard refresh browser: Ctrl+Shift+R (Cmd+Shift+R on Mac)

### Category Badges Not Showing

**Issue:** Badge colors not appearing

**Fix:**
1. Ensure post has `categories: ["Market Analysis"]` in front matter
2. Verify `layouts/partials/post-card.html` exists
3. Check console for JavaScript errors

### Fonts Not Loading

**Issue:** Space Grotesk or Inter not displaying

**Fix:**
1. Verify internet connection (fonts load from Google Fonts CDN)
2. Check browser console for CORS errors
3. Add fonts to Hugo config if self-hosting needed

### Dark Mode Issues

**Issue:** Dark mode colors broken

**Fix:**
1. PaperMod uses `localStorage` to store theme preference
2. Clear browser localStorage
3. Check `data-theme="dark"` attribute on `<html>` element

---

## Testing Checklist

### Visual Regression Testing

- [ ] Homepage loads with hero banner
- [ ] Post cards show category badges
- [ ] Left border appears on hover
- [ ] Colors match design spec hex codes
- [ ] Fonts load correctly (Space Grotesk + Inter)
- [ ] Logo shows Pokéball icon
- [ ] Navigation hovers work
- [ ] Footer social icons styled correctly

### Category Testing

- [ ] Market Analysis posts show purple badge
- [ ] Card Picks posts show green badge
- [ ] Set Reviews posts show red badge  
- [ ] Beginner's Guide posts show yellow badge
- [ ] Archive pages have gradient headers

### Responsive Testing

- [ ] Mobile: Single column layout
- [ ] Mobile: Reduced heading sizes
- [ ] Mobile: Logo icon only (optional)
- [ ] Tablet: 2-column grid
- [ ] Desktop: Full layout with sidebars

### Dark Mode Testing

- [ ] Toggle theme switch works
- [ ] All text remains readable
- [ ] Shadows visible on dark background
- [ ] Category colors adjust appropriately
- [ ] Code blocks styled correctly

### Performance Testing

- [ ] Lighthouse score 90+ (Performance)
- [ ] First Contentful Paint < 1.5s
- [ ] Largest Contentful Paint < 2.5s
- [ ] Cumulative Layout Shift < 0.1
- [ ] Total page weight < 500KB (without images)

---

## Next Steps

1. **Create Content:** Write 5-10 posts across all categories
2. **SEO Setup:** Add meta descriptions, Open Graph tags
3. **Analytics:** Install Google Analytics or Plausible
4. **Newsletter:** Integrate ConvertKit or Mailchimp signup
5. **Comments:** Add Disqus or Giscus if desired
6. **Social Sharing:** Add share buttons to posts
7. **Related Posts:** Show 3 related posts at end of articles
8. **Search:** Enable built-in PaperMod search feature

---

## Resources

- **Hugo Docs:** https://gohugo.io/documentation/
- **PaperMod Wiki:** https://github.com/adityatelange/hugo-PaperMod/wiki
- **Google Fonts:** https://fonts.google.com/
- **Color Contrast Checker:** https://webaim.org/resources/contrastchecker/
- **Hugo Modules:** https://gohugo.io/hugo-modules/

---

**Questions?** Open an issue in the GitHub repo or check the Hugo community forums.

**Last Updated:** February 8, 2026
