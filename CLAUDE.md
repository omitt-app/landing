# omitt landing - Working Notes for AI Assistants

> ⚠️ **This file is in a PUBLIC repo.** Anything written here is visible on GitHub forever. Do NOT add: pricing strategy, persona rankings, competitor analysis, customer emails, internal roadmap details, API keys, or secrets.
>
> If you're working in the parent `omitt-app/` directory, read `../CLAUDE.md` first - it has the full project knowledge base. This file is a public-safe subset focused on the landing page only.

---

## What this repo is

The marketing landing page for **omitt** - a privacy-first macOS app that detects and redacts sensitive data before users paste it into AI tools. Live at https://omitt.app.

Auto-deployed to Vercel from the `main` branch on every push.

---

## ⚠️ Hard rules (non-negotiable)

These are written into the brand DNA. Violating them breaks the product identity.

1. **Brand is `omitt` lowercase.** Never "Omitt" except at the start of a sentence. Never caps. Never "Omit" (single t).
2. **NO em-dash character (Unicode U+2014).** Always use a classic hyphen `-` instead. Search-and-replace before any commit, including in CSS comments.
3. **Voice: plain over clever, small claims, no fear copy.**
   - Words to use: hide, redact, omit, scan, on-device, careful, confident
   - Words BANNED: unlock, leverage, revolutionary, AI-powered, military-grade, breach, unhackable, supercharge, seamless, next-gen, cutting-edge, enterprise-grade
4. **Two colors only: Ink `#0D0D0C` and Cream `#F9F2ED`.** No accent colors. Ever. ~70/30 ratio.
5. **One font: Nunito Variable (self-hosted, Latin subset).** Don't add other fonts.
6. **On-device promise is sacred.** No analytics, no tracking, no telemetry on the landing page. Even Plausible/GA breaks the trust narrative.

---

## Repo layout

```
landing/
├── CLAUDE.md          # ← you are here
├── .gitignore         # comprehensive
├── index.html         # the single-page landing site
├── site.webmanifest
├── robots.txt
├── sitemap.xml
└── assets/
    ├── fonts/         # Nunito-Variable.woff2 (Latin subset, 39 KB)
    ├── icons/         # favicons, app icons
    ├── png/           # PNG exports
    └── svg/           # SVG sources (icon.svg, sprite.svg for 8 detection icons)
```

The whole site is **one HTML file** with inline CSS and JS. No build step, no bundler, no framework. Edit `index.html` directly.

---

## Pre-commit security checklist (MANDATORY)

Before every push to `main`, scan the diff for:

### Forbidden strings
- `sk_live_...`, `sk_test_...` (Stripe)
- `sk-...` (OpenAI)
- `Bearer <token>`
- `ghp_...`, `gho_...`, `ghu_...` (GitHub tokens)
- `AKIA...` (AWS access keys)
- `eyJ...` (JWT tokens)
- `xoxb-...` (Slack)
- `.env`, `secret`, `password`, `credentials.json`

### Forbidden files
- Anything matching `.env*`, `*.env`, `secrets/`, `*.pem`, `*.key`, `*.cert`
- Customer emails or lists
- Large binary blobs (>1MB usually a mistake)

### What IS safe to commit
- HTML, CSS, JS for the page
- Brand assets in `assets/`
- The Loops.so form endpoint URL (designed to be public; bot protection via honeypot field)
- README, LICENSE, this CLAUDE.md

If anything in the diff looks suspicious, abort the commit and investigate.

---

## Section structure of index.html (locked, don't reorder without reason)

1. Nav
2. Hero (icon + h1 + lede + form + demo mockup)
3. Problem (3 stat cards + callout)
4. Solution (3 steps)
5. Use Cases (6 bento cards)
6. Detect Grid (8 categories with icons)
7. Comparison Table (3 columns)
8. Privacy Promise (3 stats)
9. Pricing (3 tiers)
10. FAQ (10 questions)
11. Final CTA (with second form)
12. Footer

---

## Design system (in `index.html` inline CSS)

### Container
- Single `.wrap` class for ALL sections (max-width 1180px)
- Typography hierarchy via `max-width` on text elements, NOT via nested narrower wrappers
- Example: `.lede { max-width: 620px }`, `.signup { max-width: 480px }`

### Typography (from Nunito Variable)
| Use | Weight | Letter-spacing |
|---|---|---|
| Wordmark / Display | 800 | -0.035em |
| H1 | 800 | -0.025em |
| H2 | 800 | -0.020em |
| Subhead | 700 | -0.010em |
| Body | 400 | 0 |
| Meta | 500 | 0 |

### Colors (CSS variables defined at top of `<style>`)
```css
--ink: #0D0D0C
--cream: #F9F2ED
--cream-soft: rgba(249, 242, 237, 0.70)
--cream-mute: rgba(249, 242, 237, 0.55)
--cream-faint: rgba(249, 242, 237, 0.45)
```

Never use any other color. If you think you need an accent, you don't.

---

## Forms

Two waitlist forms on the page:
- Hero form: `id="error-hero"`, submits with `userGroup=hero`
- Final CTA form: `id="error-bottom"`, submits with `userGroup=bottom`

Both submit to the same Loops.so form endpoint (urlencoded POST: `email` + `userGroup`). The `userGroup` tag lets us measure where signups come from. A 409 response means "already on the list" and is treated as success.

### Custom validation (NOT native HTML5)
- `<form novalidate>` with custom inline JS
- Friendly error messages match brand voice:
  - "Please add your email so we can let you in when the beta opens."
  - "That email looks a bit off. Mind double-checking it?"
- Error containers use `role="alert"` + `aria-live="polite"` for accessibility
- Both forms include a hidden honeypot field for bot protection

### Don't
- Don't switch back to native HTML5 validation messages (robotic, breaks voice)
- Don't remove honeypot fields (bots would contaminate the list)
- Don't remove either form without first reviewing the userGroup ratio in Loops

---

## Verified statistics (only ones safe to cite)

Every numeric claim on the site has been web-verified. ONLY use these:

| Claim | Source |
|---|---|
| 75% of employees use AI for work tasks | Microsoft Work Trend Index 2024 |
| 858,440 AI-related DLP events | Verizon DBIR 2026 |
| U.S. v. Heppner (no privilege for AI chats) | SDNY Feb 2026, Judge Rakoff |
| Samsung 2023 source code leak | Bloomberg |

**Don't add any new stat without web-searching first.** If a source can't be confirmed, cut the claim. Compliance-buying audiences will check, and one fabricated stat destroys credibility forever.

---

## Workflow (current setup)

- User is solo founder, non-developer
- Assistant runs checks, commits, fetches, confirms the target branch, and pushes through CLI
- Founder uses GitHub Desktop only when a visual review checkpoint is explicitly requested
- Don't ask the founder to run CLI commands that the assistant can safely run
- Never force-push or bypass failed checks
- Single-purpose commits with descriptive messages

### Commit message format
```
<imperative verb> <what changed>

- <bullet of what + why>
- <bullet of what + why>
```

---

## When in doubt

If you don't have access to the parent `omitt-app/` directory, the most common questions you might have:

- **"Can I add an accent color?"** No. Two colors only.
- **"Should I add Google Analytics?"** No. On-device promise extends to the landing page.
- **"Should I remove this section?"** Probably not without a specific metric reason.
- **"Should I use em-dash here?"** Never. Use `-`.
- **"Should I 'modernize' the voice?"** No. Plain over clever is the rule.
- **"Should I add more form fields?"** No. Email-only collects the highest-conversion signal.

When unsure, preserve existing structure and change copy only.

---

## License

Code in this repo is under the LICENSE file. Brand assets (icons, wordmark) and brand identity are NOT open-source - they belong to the omitt project.
