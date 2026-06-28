# Beneath the Text — handoff document

**Live at:** https://beneaththetext.org
**Deployed via:** Netlify (drag-and-drop or connected to a Git repo)

---

## Directory structure

```
beneath-the-text/
├── index.html          ← home page
├── about.html          ← "About the voice"
├── method.html         ← sourcing and method
├── timeline.html       ← the global timeline (stub — see below)
├── library.html        ← primary sources library (stub — see below)
├── 404.html
├── robots.txt
├── sitemap.xml         ← update when you add pillars
├── netlify.toml        ← Netlify config, redirects, headers
├── content/            ← ALL CONTENT LIVES HERE — edit these, not the HTML
│   ├── hell.json
│   ├── heaven-resurrection.json
│   ├── rapture.json
│   ├── atonement.json
│   └── original-sin.json
└── pillars/            ← HTML files for each pillar (copies of pillar.html)
    ├── pillar.html     ← the canonical template — never edit the copies
    ├── hell.html
    ├── heaven-resurrection.html
    ├── rapture.html
    ├── atonement.html
    └── original-sin.html
```

---

## How to add a new pillar (pillars 6–11)

**Step 1 — create the content JSON**
Copy an existing file from `/content/` (e.g. `hell.json`) and rename it to the pillar's slug (e.g. `free-will.json`). Fill in every field. The schema is documented in the JSON itself — every key has an example value you can follow.

**Step 2 — create the HTML file**
```bash
cp pillars/pillar.html pillars/free-will.html
```
That's it. The template reads the slug from the URL and fetches the matching JSON.

**Step 3 — add a redirect in netlify.toml**
```toml
[[redirects]]
  from = "/pillars/free-will"
  to = "/pillars/pillar.html"
  status = 200
```

**Step 4 — update the home page index**
In `index.html`, find the `<div class="pillar-card coming">` for that pillar and change it to `<a class="pillar-card live" href="/pillars/free-will.html">`. Remove the `coming` class and update the status line to `Read →`.

**Step 5 — add to sitemap.xml**
Add a new `<url>` entry for the new pillar.

**Step 6 — deploy**
Push to your Git repo (if connected) or drag the folder onto the Netlify dashboard.

---

## How to edit existing pillar content

Open the relevant file in `/content/` (e.g. `content/hell.json`) and edit the text fields. The JSON is structured as follows:

- `question` — the page's opening question
- `handed` — array of paragraphs for "What you were handed"
- `threads` — array of reading threads (each with `name`, `became`, `became_label`, `detail`)
- `witnesses` — array of historical figures (each with `name`, `dates`, `standing`, `note`, `source`, `link`)
- `ascendancy` — array of paragraphs for "How one reading rose"
- `cards` — array of rewrite cards (each with `ref`, `title`, `traditional`, `charitable`, `lever`, `flag`)
- `moveon` — array of paragraphs for "Before we move on"
- `matters` — single string for "Why this matters even if you're not religious"
- `contested` — array of paragraphs for "Where this is contested"
- `close` — array of paragraphs for "Who this touches — an open door"

HTML is allowed inside string values (e.g. `<strong>`, `<em>`, `<br>`).

---

## How to edit the template (design changes)

**Never edit the individual pillar HTML files** (`hell.html`, `rapture.html`, etc.) — they are copies of `pillar.html` and any changes you make will be overwritten if you regenerate them.

**Always edit `pillars/pillar.html`**, then re-copy it to the pillar files:
```bash
cd pillars
for f in hell heaven-resurrection rapture atonement original-sin free-will bible sex-gender wealth war-empire never-healed; do
  cp pillar.html "${f}.html"
done
```

---

## Palette (WCAG-corrected)

All values pass WCAG 2.2 AA minimum contrast.

| Token | Hex | Ratio on parchment |
|-------|-----|--------------------|
| `--ink` | `#221F18` | 12.6:1 (AAA) |
| `--ink-faded` | `#565041` | 6.1:1 (AA) |
| `--madder` | `#9F3A30` | 5.1:1 (AA) |
| `--madder-deep` | `#7F2E27` | 6.9:1 (AA) |
| `--ochre` | `#7A5C1A` | 4.8:1 (AA) |
| `--parchment` | `#E5E1D4` | — (background) |

---

## Netlify deployment

**First deploy:**
1. Go to netlify.com → Add new site → Deploy manually
2. Drag the `beneath-the-text` folder onto the upload area
3. Set the custom domain to `beneaththetext.org` in Site settings → Domain management
4. Enable HTTPS (automatic with Netlify)

**Subsequent deploys:**
Drag the folder again, or connect to a GitHub repo for automatic deploys on push.

**For a "submit a question" form:**
Add this HTML anywhere in a page:
```html
<form name="contact" method="POST" data-netlify="true">
  <input type="text" name="question" placeholder="Your question">
  <button type="submit">Send</button>
</form>
```
Netlify Forms handles it automatically. Responses go to the email configured in the Netlify dashboard under Forms → Notifications. Route to `hello@beneaththetext.org` or whichever address you set up.

---

## Timeline and Library pages (stubs)

`timeline.html` and `library.html` are listed in the nav but not yet built. Create them by:
1. Copying `about.html` as a starting shell
2. Populating the timeline from the existing pillar markdown files in `/mnt/user-data/outputs/`
3. Populating the library from the `witnesses[].source + witnesses[].link` fields across all pillar JSONs

Both are straightforward once the remaining six pillars are drafted into JSON.

---

## Primary source links — verify before launch

Every `witnesses[].link` and `cards[].lever` reference should be checked against the live URL before launch. The canonical archives used are:

- **New Advent** (newadvent.org/fathers) — patristic texts in English
- **CCEL** (ccel.org) — additional patristic and Reformation texts
- **Sefaria** (sefaria.org) — rabbinic texts
- **Blue Letter Bible** (blueletterbible.org) — Scripture with original languages

---

*Last updated: initial build. Update this date whenever the template or content schema changes.*
