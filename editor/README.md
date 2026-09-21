# chemgraf.eu/editor/ — the browser editor

The ChemGraf web build, wrapped in a public page in the site's styling. Static
files; GitHub Pages serves them as they are. No build step.

## Files

| File | What it is |
|---|---|
| `index.html` | The public page: header, toolbar, the editor in an iframe |
| `notices.html` | Third-party licences (ImGui, CoordGen, SDL, stb, IBM Plex, Emscripten) |
| `app/index.html` | The editor page + window API — **copied verbatim from the handoff bundle, do not edit here** |
| `app/chemgraf.js` | Emscripten loader (same) |
| `app/chemgraf.wasm` | The editor (same) |
| `app/chemgraf.data` | Preloaded fonts (IBM Plex), templates, tool-strip icons (same) |

The wrapper talks to `app/index.html` through the same window API the ELN uses
(`eln-handoff/API.md`): `setMolRxnfile` / `getMolRxnfile` / `getSmiles` /
`setCdxml` / `getCdxml` / `getSvg` / `getLastError`, plus the `chemgraf-ready`
event. Nothing web-specific was added to the bundle for this page — the
toolbar's Open and Save buttons are the host page doing what the ELN's host
page does, because the browser build has no file dialogs of its own.

## Refreshing the bundle after a rebuild

```
tools\build_web.bat            (repo)
tools\publish_web.bat          → ChemGraf\eln-handoff\chemgraf-web\
copy the four files            → website\manual\editor\app\
```

Keep the two copies identical. If the page ever needs behaviour the bundle does
not have, add it to the wrapper, not to `app/index.html`.

## Fonts — settled

The bundle ships **IBM Plex Sans / Serif / Mono** (SIL OFL 1.1, `cpp/fonts-web/`),
not the Aptos the desktop uses: Aptos, Arial and Verdana are licensed with
Windows and Microsoft 365, and a public download is a redistribution. `CG_FONT`
(`src/app/gui_fonts.h`) picks the file per build; the family names the UI and the
file formats use are unchanged, so documents are unaffected. Measured: the label
face matches Aptos-SemiBold to +0.5 % ink and .001 in cap-height ratio, so
drawings weigh and sit the same. One thing does not carry over — bold free text
steps up only +12.3 % over the label face (Aptos-Black gave +50.8 %), because
Plex's Bold is its heaviest weight. `cpp/fonts-web/README.md` has the numbers
and the two ways out if that matters.

The **desktop** release still ships `fonts/` wholesale, Arial and Verdana
included. That question is open and is larger than this one.

## Before this goes live — open items

1. **HiDPI.** The build renders at 1x, so on a retina or scaled display the
   canvas looks soft. `status.md` §0w names the `FontGlobalScale` route.
2. **`mode=rxn` is unverified in the browser.** The page is pinned to
   `?mode=mol`; do not add a reaction tab until it has been tested.
3. **Settings do not persist** across reloads (IDBFS is not wired up).
4. **Touch — done 2026-09-21.** Below 640 px the editor uses its own compact
   layout (tool tiles along the bottom, one finger draws, two fingers zoom and
   pan, a hidden field raises the soft keyboard), and the page no longer gates
   phones: the toolbar tightens, the footer prose goes, `100dvh` keeps the
   canvas clear of the address bar. Verified only with Chrome's touch
   emulation (`cpp/tools/web_touch_harness.py`, 390×800), not on a physical
   phone yet. Still missing on touch: long-press as right-click; the
   periodic-table popup is wider than a phone screen (the quick row is fine).

## Checks worth running after the first deploy

```
curl -I https://chemgraf.eu/editor/app/chemgraf.wasm          → 200, application/wasm
curl -I -H 'Accept-Encoding: gzip, br' .../chemgraf.wasm      → is it compressed?
```

Cold load is ~6.9 MB uncompressed. GitHub Pages' soft bandwidth limit is 100 GB a
month; that is roughly 14 000 cold loads before compression and caching, which
is a long way from a problem, but it is the number to watch if the page is ever
linked somewhere large.

No analytics, no cookies, no network calls from the editor itself — the only
third-party request on the page is the Google Fonts stylesheet the rest of the
site already loads.
