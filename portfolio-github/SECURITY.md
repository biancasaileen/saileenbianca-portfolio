# Security

This is a static front-end. The primary defenses are HTTP response headers set
at the host/CDN layer — ready-to-use configs are in `deploy/` (`vercel.json`,
`_headers`, `portfolio-security.conf`).

## Code audit summary

- No `eval`, `new Function`, `innerHTML`, `document.write`, or
  `dangerouslySetInnerHTML` in the application code.
- All dynamic text (including the Sy assistant's free-text input) renders as
  React text nodes and is auto-escaped — no XSS sink.
- Authored `style="…"` attributes are applied via CSSOM (`element.style.*`),
  which is not governed by CSP.
- External links use `target="_blank"` + `rel="noopener noreferrer"`
  (no reverse-tabnabbing).
- No secrets, API keys, `.env`, debug endpoints, console logs, source maps, or
  test files ship to the client.
- `<html lang="en">`, icon-only buttons have `aria-label`s, decorative graphics
  are `aria-hidden`, and SEO/social meta + favicon are present.

## Runtime dependencies (pinned + SRI)

- `react@18.3.1`, `react-dom@18.3.1` — production builds, from unpkg with SRI.
- `@babel/standalone@7.29.0` — loaded lazily, only when a JSX component is
  imported (this site imports none, so it typically never loads).

## Content Security Policy

Strictest policy that preserves the design/runtime:

```
default-src 'self';
script-src 'self' 'unsafe-eval' https://unpkg.com;
style-src 'self' 'unsafe-inline' https://fonts.googleapis.com;
font-src 'self' https://fonts.gstatic.com;
img-src 'self' data: blob:;
media-src 'self';
connect-src 'self' https://unpkg.com;
worker-src 'self' blob:;
frame-ancestors 'none'; base-uri 'self'; form-action 'self' mailto:;
object-src 'none'; upgrade-insecure-requests
```

### Why two exceptions remain

- **`script-src 'unsafe-eval'`** — the client-side rendering runtime
  (`support.js`) evaluates the component logic with `new Function` and uses
  Babel for any JSX imports. Required to boot.
- **`style-src 'unsafe-inline'`** — the runtime injects a few `<style>` blocks
  via `.textContent` (keyframes/resets/atomics). Cannot be nonce/hash-scoped in
  this runtime.

`script-src 'unsafe-inline'` is **not** required and is intentionally omitted
(no inline `<script>` executes; the logic block is `type="text/x-dc"`).

### Removing both exceptions (long-term)

Precompile to a static bundle (Vite/Next): compile JSX at build time (drops
`unsafe-eval`), self-host React (drops the `unpkg.com` allowance), and extract
styles to a hashed `.css` file or add a per-response nonce (drops
`style-src 'unsafe-inline'`). Target: `script-src 'self'; style-src 'self'`.

## Reporting

Report vulnerabilities privately to: biancasaileen@gmail.com
