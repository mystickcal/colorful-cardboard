# Colorful Cardboard - Design Quick Reference Card

## 🎨 Color Palette

### Brand Colors
```
Electric Yellow   #FFD93D   ███  Beginner's Guide, CTAs
Deep Blue         #2B4C7E   ███  Primary Brand, Links
Cardinal Red      #DC2626   ███  Set Reviews, Alerts
Emerald Green     #059669   ███  Card Picks, Growth
Royal Purple      #8B5CF6   ███  Market Analysis, Premium
```

### Neutral Palette (Slate)
```
Slate 900   #0F172A   ███  Darkest Text
Slate 800   #1E293B   ███  Body Text
Slate 600   #475569   ███  Secondary Text
Slate 400   #94A3B8   ███  Muted Text
Slate 200   #E2E8F0   ███  Borders
Slate 100   #F1F5F9   ███  Light Backgrounds
Slate 50    #F8FAFC   ███  Page Background
```

---

## 📝 Typography

### Font Families
```css
Headings:  'Space Grotesk', sans-serif
Body:      'Inter', sans-serif
```

### Type Scale (Desktop)
```
h1: 2.5rem (40px) - font-weight: 800
h2: 2.0rem (32px) - font-weight: 700
h3: 1.5rem (24px) - font-weight: 600
Body: 17px - font-weight: 400, line-height: 1.7
Small: 0.875rem (14px)
```

### Type Scale (Mobile)
```
h1: 2.0rem (32px)
h2: 1.5rem (24px)
h3: 1.25rem (20px)
```

---

## 🏷️ Category System

| Category | Color | Hex | Badge | Tone |
|----------|-------|-----|-------|------|
| **Market Analysis** | Purple | #8B5CF6 | White text | Premium, Data-driven |
| **Card Picks** | Green | #059669 | White text | Actionable, Optimistic |
| **Set Reviews** | Red | #DC2626 | White text | Exciting, Timely |
| **Beginner's Guide** | Yellow | #FFD93D | Dark text | Friendly, Educational |

---

## 📐 Spacing & Layout

### Dimensions
```css
--gap: 28px (20px mobile)
--content-gap: 24px
--nav-width: 1200px
--main-width: 800px
--header-height: 72px
--footer-height: 80px
--radius: 12px
```

### Breakpoints
```
Desktop:  1024px+
Tablet:   768-1023px
Mobile:   <768px
```

---

## 🎯 Logo

```
◉ Colorful Cardboard
```

**Pokéball Icon (◉):**
- Top: Cardinal Red (#DC2626)
- Middle: Slate 800 (#1E293B)
- Bottom: White

**Wordmark:**
- Font: Space Grotesk Bold (700)
- Color: Deep Blue (#2B4C7E)
- Size: 1.5rem (24px)

---

## 🔗 CSS Variables (Key)

### Colors
```css
--accent: #2B4C7E           (Deep Blue)
--accent-hover: #1E3A5F     (Darker Blue)
--success: #059669          (Emerald Green)
--warning: #FFD93D          (Electric Yellow)
--danger: #DC2626           (Cardinal Red)
--info: #8B5CF6             (Royal Purple)
```

### PaperMod Overrides
```css
--primary: #1E293B          (Slate 800 - main text)
--secondary: #475569        (Slate 600 - secondary text)
--tertiary: #CBD5E1         (Slate 300 - tertiary)
--border: #E2E8F0           (Slate 200 - borders)
--theme: #FFFFFF            (White - page background)
--entry: #FFFFFF            (White - card background)
```

---

## 📦 Component Classes

### Buttons
```css
.button {
  background: var(--accent);
  color: white;
  padding: 12px 24px;
  border-radius: 8px;
  font-weight: 600;
  transition: all 0.2s ease;
}
```

### Post Cards
```css
.post-entry {
  background: var(--entry);
  border: 1px solid var(--border);
  border-radius: 12px;
  padding: 1.75rem;
  box-shadow: var(--shadow-sm);
}

.post-entry:hover {
  transform: translateY(-2px);
  border-color: var(--accent);
}
```

### Category Badges
```css
padding: 4px 12px;
border-radius: 6px;
font-size: 0.75rem;
font-weight: 600;
text-transform: uppercase;
```

---

## 🎨 Shadows

```css
--shadow-sm:   0 1px 2px rgba(0,0,0,0.05)
--shadow:      0 1px 3px rgba(0,0,0,0.1)
--shadow-md:   0 4px 6px rgba(0,0,0,0.1)
--shadow-lg:   0 10px 15px rgba(0,0,0,0.1)
--shadow-xl:   0 20px 25px rgba(0,0,0,0.1)
```

---

## 📝 Post Front Matter Template

### Market Analysis
```yaml
---
title: "Your Post Title"
date: 2026-02-08
categories: ["Market Analysis"]
tags: ["market trends", "vintage", "modern"]
description: "150-160 character description for SEO"
ShowToc: true
TocOpen: false
draft: false
---
```

### Card Picks
```yaml
---
title: "Your Post Title"
date: 2026-02-08
categories: ["Card Picks"]
tags: ["undervalued", "investment", "specific-card"]
description: "150-160 character description for SEO"
draft: false
---
```

### Set Reviews
```yaml
---
title: "Your Post Title"
date: 2026-02-08
categories: ["Set Reviews"]
tags: ["set-name", "set review", "chase cards"]
description: "150-160 character description for SEO"
draft: false
---
```

### Beginner's Guide
```yaml
---
title: "Your Post Title"
date: 2026-02-08
categories: ["Beginner's Guide"]
tags: ["beginner", "getting started", "basics"]
description: "150-160 character description for SEO"
ShowToc: true
TocOpen: true
draft: false
---
```

---

## 🔧 Hugo Commands

### Development
```bash
hugo server -D              # Run dev server with drafts
hugo server --bind 0.0.0.0  # Accessible on network
hugo server --disableFastRender  # Full rebuild each change
```

### Production
```bash
hugo --minify               # Build for production
hugo --gc                   # Clean cache
hugo --cleanDestinationDir  # Clean public/ before build
```

---

## ✅ Pre-Launch Checklist

### Design
- [ ] CSS loads correctly (check DevTools)
- [ ] Category badges appear on posts
- [ ] Left borders show on hover
- [ ] Logo Pokéball renders properly
- [ ] Dark mode toggle works
- [ ] All colors match spec

### Content
- [ ] At least 1 post per category
- [ ] All posts have front matter
- [ ] Images optimized (WebP + JPG)
- [ ] About page complete
- [ ] Disclaimer added

### SEO
- [ ] Meta descriptions (150-160 chars)
- [ ] Open Graph tags
- [ ] Twitter Card tags
- [ ] Sitemap.xml generated
- [ ] Robots.txt configured

### Performance
- [ ] Lighthouse score 90+
- [ ] Images lazy load
- [ ] Fonts preconnected
- [ ] CSS minified
- [ ] HTML minified

---

## 📞 Contact & Resources

**Design Files:**
- `assets/css/extended/custom.css` (main stylesheet)
- `DESIGN-SPEC.md` (full design documentation)
- `IMPLEMENTATION-GUIDE.md` (setup instructions)

**External Resources:**
- Hugo Docs: https://gohugo.io/documentation/
- PaperMod Wiki: https://github.com/adityatelange/hugo-PaperMod/wiki
- Google Fonts: https://fonts.google.com/
- Color Contrast: https://webaim.org/resources/contrastchecker/

**Support:**
- Hugo Community: https://discourse.gohugo.io/
- PaperMod Issues: https://github.com/adityatelange/hugo-PaperMod/issues

---

## 🎯 Quick Tips

### Adding a New Category Color
1. Define color variable in `:root`:
   ```css
   --brand-orange: #F97316;
   ```
2. Add category styling:
   ```css
   [data-category="new-category"] .entry-header::after {
     background: var(--brand-orange);
     color: white;
   }
   ```

### Changing Logo
Edit `hugo.toml`:
```toml
[params.label]
  text = "Your Site Name"
  icon = "◉"
```

### Custom Homepage Hero
Update `hugo.toml`:
```toml
[params.homeInfoParams]
  Title = "Your Hero Title"
  Content = "Your hero description..."
```

### Enable Search
```toml
[outputs]
  home = ["HTML", "RSS", "JSON"]
```
Then add search icon to `hugo.toml` menu.

---

**Version:** 1.0  
**Last Updated:** February 8, 2026  
**Print this page and keep it handy during development!**

---

## 📊 At-a-Glance Reference

```
LOGO:    ◉ Colorful Cardboard
FONTS:   Space Grotesk (headings) + Inter (body)
PRIMARY: #2B4C7E (Deep Blue)
RADIUS:  12px
GAP:     28px (20px mobile)
MAX:     800px content, 1200px nav

CATEGORIES:
Purple   → Market Analysis
Green    → Card Picks  
Red      → Set Reviews
Yellow   → Beginner's Guide
```

---

**Need help?** Reference `DESIGN-SPEC.md` for detailed explanations or `IMPLEMENTATION-GUIDE.md` for step-by-step instructions.
