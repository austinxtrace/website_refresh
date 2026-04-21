# XTrace site — source

Two self-contained static HTML files. No build step, no dependencies. Open either file in a browser and it just works.

## Structure

```
site/
├── index.html   ← homepage (xtrace.ai)
├── xmem.html    ← developer subpage (xtrace.ai/xmem)
└── README.md    ← this file
```

Each page is fully self-contained: HTML + inline `<style>` + inline `<script>`. Tailwind is loaded from a CDN, fonts from Google Fonts. Nothing else external.

## Preview locally

Just double-click either `index.html` or `xmem.html`. No server required.

If you want hot-reload while editing, any static server works:

```sh
# VS Code / Cursor: install the "Live Server" extension, right-click the file, "Open with Live Server"

# or, from the terminal:
npx serve site       # node
python3 -m http.server -d site 8000   # python
```

## Section map

Each page uses HTML comment markers for every section, e.g. `<!-- ─────────────── HERO ─────────────── -->`. Use your IDE's outline / minimap to jump between them.

### index.html (homepage)

1. **NAV** — top bar, Pinecone-style Product dropdown
2. **HERO** — "Access was the easy part. Understanding is the work."
3. **PROOF BAND** — the 50% / 92.3% / 30% / 8+ stats row
4. **PROBLEM** — "What does it mean for your company to be understood?" + five failure modes
5. **MEMORY HUB** — how it works (3 steps), before/after XTrace, why different, use cases
6. **SHIFT** — the Hyperspell-derived "access vs understanding" narrative
7. **COMPOUNDING** — "The understanding has to accumulate"
8. **BENCHMARK** — LongMemEval chart (links to full benchmarks on xmem.html)
9. **PRICING** — Personal / Team / Enterprise
10. **FAQ** — seven questions
11. **FINAL CTA** — "Your company is already being understood by somebody..."
12. **FOOTER**

### xmem.html (developer page)

1. **NAV** — same as homepage (XMem row highlighted as active)
2. **HERO** — "Drop-in memory that reasons" + Python/TS/Go code sample tabs
3. **TRUSTED BAR** — agent-type pills
4. **PROBLEM FOR DEVS** — "Your agent remembers everything. That's the bug."
5. **BENCHMARKS** — four metric cards + LOCOMO chart
6. **WHY NOT VECTOR DB** — side-by-side comparison
7. **ARCHITECTURE** — flow diagram + live `curl` example
8. **PRIMITIVES** — six core objects (Fact, Episode, Artifact, Supersede, Policy, Audit)
9. **USE CASES** — six agent types
10. **FOR CTOs** — compliance & reassurance block
11. **PRICING** — Hacker / Startup / Scale / Enterprise
12. **MEMORY HUB BRIDGE** — "Same engine, two surfaces" upsell card
13. **FAQ** — seven developer-facing questions
14. **FINAL CTA** — "Your agent's memory problem is already solved."
15. **FOOTER**

## Brand tokens

Both files define the same Tailwind config inline. If you change a color, change it in **both** files (see the `tailwind.config` block near the top of each).

| Token | Value | Usage |
|---|---|---|
| `plum.600` | `#6B3FA0` | primary brand, buttons, accents |
| `plum.700` | `#573285` | headline italics, hover states |
| `paper.50` | `#fbfaf7` | page background |
| `ink.900` | `#1a1717` | body text |
| Newsreader | serif | headlines & italics |
| Manrope | sans | body & UI |
| JetBrains Mono | mono | code, chips, benchmark numbers |

## Common edits (fast find-and-replace)

| You want to change… | Search for… |
|---|---|
| Primary headline on homepage | `Access was the easy part` |
| XMem headline | `Drop-in memory that reasons` |
| Pricing on XMem page | `$49` / `$199` (current Startup / Scale prices) |
| LOCOMO score | `92.3%` (multiple places — metric card + bar chart) |
| Scale plan price | `$199` |
| Brand plum color | `#6B3FA0` (or use Tailwind class `plum-600`) |
| Nav dropdown copy | Section marked `<!-- NAV -->`, look for `nav-card-desc` |
| Footer links | Section marked `<!-- FOOTER -->` |

## Placeholders to verify before you ship

These are best-guess numbers and copy I filled in. Confirm with your team:

- [ ] **LongMemEval score (92.3%)** on `xmem.html` metric card — Mem0 claims 93.4% publicly; use your real score or remove the card if we don't have one
- [ ] **OpenAI Memory on LOCOMO (~52.9%)** — estimated from Mem0's comparative figure; swap for a real source if you have one
- [ ] **Scale plan limits** (5M adds / 50M recalls) — sanity-check these pencil out at $199/mo given your infra costs
- [ ] **Memory Hub bridge card** mentions *"starts at team-size of 5"* — align with actual Memory Hub pricing
- [ ] **p50 < 80ms latency claim** — confirm current p50 on Scale tier
- [ ] **"50% faster onboarding"** stat on homepage proof band — needs a customer or internal study to back it
- [ ] **Draper Associates logo** — the hero references "Backed by Draper Associates" in text; if you want the logo visible, add it to the proof band
- [ ] **Customer pullquote on xmem.html** ("Infra lead, YC W26 voice-agent startup") — replace with a real named customer when you have one
- [ ] **Zep LOCOMO number (75.1%)** — we used Zep's own corrected number; watch for pushback from Mem0's team who claim 58.4%

## Deployment

Drag-and-drop works with all three of these:

- **Vercel** — `vercel deploy site` from the parent folder, or drag the `site` folder onto [vercel.com/new](https://vercel.com/new)
- **Netlify** — drag the `site` folder onto [app.netlify.com](https://app.netlify.com)
- **Cloudflare Pages** — connect the git repo, set output directory to `site`

Default routing just works: `/` serves `index.html`, `/xmem` serves `xmem.html` on most hosts. If your host requires explicit routing, add a `_redirects` file (Netlify/Cloudflare) or `vercel.json` (Vercel) mapping `/xmem` → `/xmem.html`.

## Handing to a designer or frontend dev

If you eventually rebuild this in Framer, Webflow, Next.js, or similar:

- Copy the messaging and hierarchy straight from the HTML — the structure maps 1:1 to sections in any CMS
- Brand tokens above are the complete palette
- Tailwind classes in the markup are only hints — they don't survive the port. The copy and the section flow are what matters.
