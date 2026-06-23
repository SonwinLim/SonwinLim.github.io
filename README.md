# Ray Lim — Personal Profile Site

A single self-contained `index.html` (no build step, no dependencies). Aesthetic: an
aerospace-cockpit dossier — near-black ink, warm paper, signal-amber accent. Fonts: Fraunces
(display), Newsreader (body), JetBrains Mono (labels). All content is sourced from the approved
ResumeForge files and respects the Career Transition Narrative disclosure boundaries.

```
resume-site/
└── index.html      ← the entire site
```

To preview locally, just open the file in a browser, or run a static server (below).

---

## Hosting on a Mac mini behind a Cloudflare Tunnel

Two pieces:

1. **A static web server** on the Mac mini serving this folder on a local port.
2. **A Cloudflare Tunnel** (`cloudflared`) exposing that port at your domain — no port-forwarding,
   no public IP, automatic HTTPS.

> Target hostname: **`raylim.me`** (the apex — the natural home for a personal profile). To use a
> subdomain instead, swap `raylim.me` for e.g. `profile.raylim.me` in the `route dns` command and the
> config `hostname`. The domain must already be in your Cloudflare account (nameservers on Cloudflare).

### 1 · Serve the folder

**Option A — Caddy (recommended; runs forever, clean logs)**

```bash
brew install caddy
cd "/path/to/resume-site"
caddy file-server --listen :1235 --root .
```

**Option B — Python (already on macOS, zero install)**

```bash
cd "/path/to/resume-site"
python3 -m http.server 1235
```

Confirm it works: open `http://localhost:1235` on the Mac mini.

### 2 · Install and authenticate cloudflared

```bash
brew install cloudflared
cloudflared tunnel login          # opens a browser; pick raylim.me
```

### 3 · Create a named tunnel + DNS route

```bash
cloudflared tunnel create ray-profile
cloudflared tunnel route dns ray-profile raylim.me
```

The `create` step prints a **Tunnel ID** and writes a credentials JSON to
`~/.cloudflared/<TUNNEL_ID>.json`. Note both.

### 4 · Configuration file

Create `~/.cloudflared/config.yml`:

```yaml
tunnel: ray-profile
credentials-file: /Users/raylim/.cloudflared/<TUNNEL_ID>.json

ingress:
  - hostname: raylim.me
    service: http://localhost:1235
  - service: http_status:404
```

### 5 · Run it

```bash
cloudflared tunnel run ray-profile
```

Visit `https://raylim.me` — it should load over HTTPS.

### 6 · Keep both running across reboots

Install the tunnel as a launchd service:

```bash
sudo cloudflared service install
```

For the web server, the simplest durable option is Caddy as a brew service:

```bash
brew services start caddy
```

…with a `Caddyfile` in the folder instead of CLI flags:

```
:1235 {
    root * /path/to/resume-site
    file_server
}
```

(Or wrap the Python server in a launchd plist if you prefer.)

---

## Quick public test (no DNS setup)

To share a temporary `*.trycloudflare.com` link without configuring DNS:

```bash
cloudflared tunnel --url http://localhost:1235
```

Good for a quick preview; the URL changes each run, so use the named tunnel above for the real site.

---

## Editing content

Everything lives in `index.html`. Sections, in order:

| # | Section | Anchor |
|---|---------|--------|
| 01 | The hybrid profile + headline metrics | `#profile` |
| 02 | Signature results | `#results` |
| 03 | AI-enabled execution | `#ai` |
| 04 | Track record (timeline) | `#experience` |
| 05 | Capabilities (Commercial / Technical) | `#capabilities` |
| 06 | Foundations & honours | `#education` |
| 07 | Contact | `#contact` |

- **Colors / fonts:** the `:root` block at the top of the `<style>`.
- **Animated numbers:** any `<span data-count="N" data-suffix="…">`.
- **Content guardrails:** keep wording inside the "safe to say" set from
  `ResumeForge Career Transition Narrative.md`. Do not introduce anything from its "do not say" list.
