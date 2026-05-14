# Haystack — Personal Dashboard

A standalone HTML productivity dashboard, opened daily, hosted as a static site on Netlify.

---

## What it is

Single-page dashboard with **eight widgets**: Calendar, Affirmations, Inspo, WILT, Goals, People, Resources, Projects. All state persists in browser localStorage. No backend, no build step, no dependencies beyond a single HTML file.

---

## File structure

```
/
├── haystack.html          ← the entire app, single file
└── icons/
    ├── logo.png           ← Haystack lockup (masthead)
    ├── wilt.webp          ← hand with pencil
    ├── goals.webp         ← signpost
    ├── people.webp        ← birds with crest
    ├── resources.webp     ← carabiner with keys
    └── projects.webp      ← folder with sticker
```

**Still missing:** `calendar.webp`, `affirmations.webp`, `inspo.webp` — those three widget icon slots are empty pending illustrations.

---

## Tech stack

- **Single HTML file** with inline CSS + JS
- **localStorage** for persistence — storage key: `adam-dashboard-v1`
- **Open-Meteo API** for weather (no API key needed)
- **Google Calendar embed** for the calendar widget (user pastes their own embed URL via Settings)
- **Hosted on**: GitHub + Netlify

---

## Brand

### Logo
Hand-drawn haystack + "Haystack" wordmark lockup. Renders at 78px height on desktop, 56px on mobile. Sits in the masthead with an italic tagline below: *"~ a place to think with yourself ~"*.

### Typography
- **Font**: Iowan Old Style (built-in macOS system font, no Typekit needed)
- **Fallback stack**: `"Iowan Old Style", "Iowan Old Style BT", Charter, "Charter BT", Georgia, serif`
- **Single font for everything** — display, body, UI. No sans-serif.
- Body size: 15px. Widget titles: 24px bold. Page masthead: image-based.

### Color palette (CSS variables in `:root`)

**Brights** — for accents, icons, brand details:
```
--hs-pink:   #FF76B8
--hs-yellow: #FFCA33
--hs-green:  #34B36B
--hs-blue:   #4D68FF
--hs-orange: #FF8A4C
```

**Softs** — for widget backgrounds:
```
--hs-sky:    #D6ECFF
--hs-mint:   #D9F7E6
--hs-cream:  #FFF7EB
```

**Neutrals**:
```
Background: #FAF6EE (warm cream page bg)
Paper:      #FFFCF5 (slightly warmer for cards)
Ink:        #111111 (Haystack black)
```

### Style guide principles
- Hand-drawn, uneven linework
- Sparse, purposeful color (never overuse)
- Optimistic, human, never stiff or corporate
- Leave space to breathe

---

## Widget inventory

| Widget | Background | Icon | Function |
|---|---|---|---|
| **Calendar** | Cream/paper | *(needs icon)* | Embedded Google Calendar with Month/Week/Schedule toggle. Paste embed URL via Settings button. |
| **Affirmations** | Cream `#FFF7EB` | *(needs icon)* | Sticky-note style notes. Click "+ Add" to create, click into a note to edit, hover for delete. |
| **Inspo** | Cream/paper | *(needs icon)* | Image rotates hourly. Manage via "Manage" modal — paste URLs or upload images (stored as base64). |
| **WILT** | Sky `#D6ECFF` | hand + pencil | "What I Learned Today" — one entry per date. Today's input is at the top, history below. |
| **Goals** | Soft yellow `#FFF1CC` | signpost | Tabs: Day / Week / Month / Bucket. Checkbox + inline-edit + delete on hover. |
| **People** | Soft pink `#FFE3EF` | birds + crest | Quick name list in widget; click "manage" or a name to open full editor with notes per person. |
| **Resources** | Mint `#D9F7E6` | carabiner + keys | Title + URL link pairs, opens in new tab. |
| **Projects** | Cream/paper | folder + sticker | Click a project → fullscreen board of note cards. Each note has rich text + cover + tag. |

### Project notes — rich text editor

Each note supports:
- **Formatting**: bold, italic, strikethrough
- **Blocks**: H1, H2, paragraph
- **Lists**: bullet, numbered, checklist (checkable, state persists)
- **Media**: link, inline image (paste a screenshot directly — auto-embeds as base64)
- **Cover image**: URL or upload
- **Tag**: `#today` / `#tomorrow` / `#someday`

---

## Data persistence

- **Everything** lives in browser localStorage under key `adam-dashboard-v1`
- **Per browser, per domain** — does NOT sync across devices
- Clearing browser cache = losing dashboard data
- **No export/import yet** (see TODOs)

---

## Deployment workflow

1. Drop `haystack.html` and the `icons/` folder into the GitHub repo
2. Netlify auto-deploys on push
3. Visit `https://yourdomain.com/haystack.html` (or rename to `index.html` if it's the root)

---

## Open TODOs / deferred decisions

### Icons still needed
- **Calendar** — style guide shows calendar with pink date marker
- **Affirmations** — style guide shows speech bubble with heart
- **Inspo** — style guide shows light bulb (idea)

When icons arrive, drop them in `icons/` and update the matching `<span class="icon-slot">` in the HTML — they're tagged with `data-icon="calendar"` etc. for easy find.

### Features deferred
1. **Subprojects** — original spec mentioned them; UX not yet decided (nested folders? tabs within a project? tag-based grouping?)
2. **Goal recurrence** — daily goals don't auto-reset at midnight. Need to decide: roll over, archive, or stay as manual checklist?
3. **Cross-project "today" view** — `#today` tag works per-note but there's no top-level filter to see *all* notes tagged today
4. **Backup / export JSON** — protect against localStorage loss; should be a one-click export and matching import

### Style / copy
- Tagline *"~ a place to think with yourself ~"* could be removed or made dynamic (rotating quotes, daily message, etc.)

---

## Quirks worth knowing

- **Calendar embed** accepts either the raw URL or a full `<iframe>` tag — JS extracts `src=` automatically
- **Inspo uploads** are stored as base64 strings in localStorage; keep individual file sizes under ~1MB to avoid hitting the ~5MB localStorage cap
- **Image paste in notes** works — paste from clipboard, gets embedded as base64
- **Mobile layout** stacks all widgets full-width below 700px viewport
- **Hourly inspo rotation** uses timestamp comparison, not a background refresh — the image changes when you reload the page after an hour has passed
- **Colored widgets have no borders** (Affirmations, WILT, Goals, People, Resources) — they're flat color blocks. Don't add borders back.
- **Icon size in widget headers**: 38px (large enough to read the illustration detail)

---

## Design decisions made (so we don't relitigate them)

- Iowan Old Style chosen over Instrument Serif — warmer, more scholarly, matches the Opennote/Haystack aesthetic
- All widgets pastel colored except Calendar / Inspo / Projects (those stay neutral cream so they don't fight for attention)
- Bordersless on colored widgets — Adam preference, do not change
- Real haystack logo replaces text masthead and the pink hand-drawn underline (no longer needed once the logo image is in place)
- Project name: "Haystack" (renamed from "Dashboard")

---

## How Adam works on this

- Standard Claude chat (not Claude Code — Claude Code has a known image-message bug)
- Doesn't edit HTML/CSS himself; relies on Claude to produce complete, ready-to-deploy files he drops into his local GitHub folder
- Iterative visual workflow — small adjustments, hard-refresh review (`Cmd+Shift+R`), one change at a time
- Use uploaded live files as the base, not Claude's reconstructed/cached versions
