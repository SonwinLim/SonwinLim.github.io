# Ray Lim — Personal Profile Site

A single self-contained `index.html` (no build step, no dependencies). Aesthetic: an
aerospace cockpit at night — blue-grey panel, warm paper, avionics-amber accent, and the track
record set as buff ATC flight-progress strips. Fonts: Newsreader (the commercial voice: headings
and body) and B612 / B612 Mono (the builder voice: labels, figures, dates — Airbus's cockpit
display face). `projects/index.html` shares the same tokens. Positioning: "Commercial operator
who builds" — settled, not job hunting; the current employer is never named. All content is
sourced from the approved ResumeForge files and respects the Career Transition Narrative
disclosure boundaries.

```
resume-site/
├── index.html      ← the entire site (HTML + CSS + JS, no build)
├── CNAME           ← raylim.me (required for GitHub Pages to claim the apex)
└── .gitignore      ← excludes docs/ and .DS_Store
```

To preview locally, just open the file in a browser, or run a static server (below).

---

## Deployment

Hosted on **GitHub Pages**, with Cloudflare in front as a DNS proxy (orange cloud).

- **Repo:** https://github.com/SonwinLim/SonwinLim.github.io
- **Apex domain:** `raylim.me` (claimed via the `CNAME` file in repo root)
- **SSL/TLS:** Cloudflare mode = **Full** (visitors always HTTPS)

### Deploy a change

```bash
cd "/Users/raylim/Bespoke Software/resume-site"
git add . && git commit -m "..."
git push origin main
```

Site rebuilds in ~30s. Cloudflare edge picks up the new content automatically.

### Cloudflare DNS records (for reference)

| Type | Name | Target | Proxy |
|------|------|--------|-------|
| A | `@` | `185.199.108.153` | Proxied |
| A | `@` | `185.199.109.153` | Proxied |
| A | `@` | `185.199.110.153` | Proxied |
| A | `@` | `185.199.111.153` | Proxied |
| AAAA | `@` | `2606:50c0:8000::153` | Proxied |
| AAAA | `@` | `2606:50c0:8001::153` | Proxied |
| AAAA | `@` | `2606:50c0:8002::153` | Proxied |
| AAAA | `@` | `2606:50c0:8003::153` | Proxied |
| CNAME | `www` | `SonwinLim.github.io` | Proxied |

SSL/TLS encryption mode: **Full** (under SSL/TLS tab, not Records).

---

## Editing content

Everything lives in `index.html`. Sections, in order:

| Section | Anchor |
|---------|--------|
| Hero: identity + portrait | `#top` |
| Two fluencies + readouts | `#profile` |
| Results ledger | `#results` |
| How I build: rules, systems map, pipeline, personal builds | `#build` |
| Track record (flight strips; `.strip.now` marks the current entry) | `#record` |
| What I work with (Commercial / Technical) | `#capabilities` |
| Education and honours | `#education` |
| Writing (newsletters) | `#writing` |
| Contact | `#contact` |

- **Colors / fonts:** the `:root` block at the top of the `<style>`.
- **Motion:** one hero load sequence (`.rise`); off under `prefers-reduced-motion`.
- **Content guardrails:** keep wording inside the "safe to say" set from
  `ResumeForge Career Transition Narrative.md`. Do not introduce anything from its "do not say" list.
