# chemgraf.eu — landing page

One self-contained `index.html`. No build step, no dependencies except Google Fonts.

## Files

| File | What it is |
|---|---|
| `index.html` | The whole page |
| `impressum.html` | Impressum (ECG §5 / MedienG §25) — **placeholders, fill in before publishing** |
| `mark.png` | Logo mark, cropped from your logo (header) |
| `icon.png` | 512×512 square, used as favicon |
| `og.png` | 1200×630 link-preview card for Reddit / Slack / X |
| `logo.png` | Your original, kept for reference |

## Add your screenshots

Drop four PNGs next to `index.html` with these exact names:

- `shot-editor.png`
- `shot-nmr.png`
- `shot-stoich.png`
- `shot-naming.png`

Until a file exists, its slot shows a hatched placeholder with the filename on it, so
the page never looks broken. Roughly 16:10, ~1600 px wide is plenty. Captions are in
`index.html` under `<section id="look">`.

## Things to change before you post

- `mailto:hello@chemgraf.eu` appears twice (hero button, footer). Point it somewhere real,
  or swap the hero button for a Buttondown / Formspree form if you'd rather collect addresses.
- The free / paid feature lists live in `<section id="versions">`. **No price is stated** — the
  Pro card says "One payment · lifetime licence". Put a number in when you have decided one.
- `impressum.html` is required for an Austrian commercial website (ECG §5: name, real street
  address, e-mail; MedienG §25 disclosure). It is all `[placeholders]` right now — fill in
  your name and address before the site goes live, and delete the UID row if you have none.
- REMOVED on purpose (2026-08-18), don't put back: the ChemSketch / ChemDraw names and the
  trademark fine print (competitors stay implicit); the "status" section with the release
  estimate and working/not-yet lists (the screenshots carry that); the "one chemist in
  Vienna" line. The pitch is now `<section id="sketch">` — keep it short.
- The internal ChemDraw parity scorecard was REMOVED on purpose (2026-08-18). Those
  percentages come from `literature/notes/chemdraw-parity-eval.md`, which is an internal
  instrument scored against the ChemDraw user guide chapter by chapter — it is for steering
  the work, not for the public. Publishing "Query & database 5%" invites an argument about
  our own numbers instead of the point. Keep the eval; don't put it back on the page.
- `og:url` and `og:image` assume the site is served at `https://chemgraf.eu/`.

## Deploy to GitHub Pages on chemgraf.eu

1. Push these files to a repo (root, or a `docs/` folder).
2. Repo → **Settings → Pages** → set source, then set **Custom domain** to `chemgraf.eu`.
   That writes a `CNAME` file into the repo. Don't delete it.
3. In the Openprovider DNS zone for `chemgraf.eu`:
   - **Delete any URL-forwarding / redirect entry.** That's what rewrites the address bar.
   - `@` → four **A** records: `185.199.108.153`, `185.199.109.153`, `185.199.110.153`, `185.199.111.153`
   - `@` → four **AAAA** records: `2606:50c0:8000::153`, `2606:50c0:8001::153`, `2606:50c0:8002::153`, `2606:50c0:8003::153`
   - `www` → **CNAME** → `<your-username>.github.io.`
4. Wait for DNS, then tick **Enforce HTTPS** in the Pages settings.

Check it worked: `dig chemgraf.eu +short` should list the four GitHub IPs, and
`curl -I https://chemgraf.eu` should return `200`, not `301`.
