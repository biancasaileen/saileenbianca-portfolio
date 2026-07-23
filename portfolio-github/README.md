# Saileen Bianca Baldonado — Portfolio

Personal portfolio for an **AI Automation & Workflow Specialist**. A single-page,
motion-rich site with an interactive AI assistant ("Sy"), an animated hero video,
3D flip service cards, an experience timeline, and a workflow/tech-stack pipeline.

## Live preview / local run

The site is fully static — no build step required. Serve the folder over HTTP
(so the video, fonts, and runtime load correctly):

```bash
# any static server works
npx serve .
# or
python3 -m http.server 8080
```

Then open `http://localhost:8080/`. Opening `index.html` via `file://` will not
play the video or load the runtime — always serve over HTTP(S).

`index.html` and `Portfolio.dc.html` are identical; `index.html` exists so GitHub
Pages / static hosts serve it as the default document.

## Project structure

```
index.html                 Entry document (served by static hosts)
Portfolio.dc.html          Same content, original authoring filename
support.js                 Client-side rendering runtime
image-slot.js              Drag-and-drop image placeholder web component
uploads/
  FORPORTFOLIO.mp4         Hero background video
  hero-poster.jpg          Hero poster (shown before video plays)
  Saileen_..._Portfolio.pdf  Résumé
deploy/                    Production security-header configs (see SECURITY.md)
```

## Third-party resources

- **Fonts:** Google Fonts (Space Grotesk, Manrope) via `fonts.googleapis.com`.
- **Runtime libs:** React 18.3.1 + ReactDOM (production builds), and Babel
  Standalone (only if a JSX component is imported) — loaded from `unpkg.com`
  with Subresource Integrity (SRI) hashes.

## Deploying

1. Push this folder to a GitHub repo.
2. **GitHub Pages:** Settings → Pages → deploy from branch root. (Note: Pages
   does not send custom response headers — use Netlify/Vercel/Nginx for the
   security headers in `deploy/`.)
3. **Vercel:** copy `deploy/vercel.json` to the repo root.
4. **Netlify:** copy `deploy/_headers` to the publish root.
5. **Nginx:** include `deploy/portfolio-security.conf` in your `server {}` block.

See **SECURITY.md** for the full hardening report and CSP rationale.

## License

MIT — see `LICENSE`.
