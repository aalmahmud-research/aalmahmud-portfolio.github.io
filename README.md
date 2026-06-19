# Research Portfolio — "Through-line"

A distinctive, editorial single-page academic portfolio. All content lives in **JSON
files** — you edit data and swap images; you rarely touch `index.html`.

## Design at a glance

- **Palette:** porcelain background, pine-ink text, a refined **teal** primary, and a
  sparing **brass** accent.
- **Type:** *Fraunces* (serif display) for headings + *Plus Jakarta Sans* (body) +
  *JetBrains Mono* (dates/labels).
- **Signature:** a continuous teal "spine" thread — section eyebrows, card hover accents,
  and the news timeline all share it.
- **Motion:** nav underline draws in on hover, sections fade up on scroll, hero rises in
  on load. All disabled automatically for visitors who prefer reduced motion.
- **Pagination:** News and Publications split across numbered pages.

## File structure

```
portfolio/
├── index.html              ← template + styles + logic (rarely edited)
├── config.json             ← per-page counts, font sizes, news highlight colors
├── about.json              ← name, bio, photo, skills, socials, CV link
├── news.json               ← news items (paginated)
├── research_interest.json  ← research area cards
├── education.json          ← degrees
├── experience.json         ← positions
├── publications.json       ← papers (paginated, collapsible abstracts)
├── projects.json           ← project cards
└── media/                  ← images, logos, CV PDF
```

## Run locally

The page loads data with `fetch()`, which **does not work by double-clicking the file**
(`file://`). Serve it over HTTP. From inside this folder:

```bash
python3 -m http.server 8000
```

Open <http://localhost:8000>. (Any static server works — `npx serve`, VS Code Live Server, etc.)

## Publish on GitHub Pages

Since this is your **own** long-term portfolio, create a **new repository** named
`your-username.github.io`, put these files at its root, push, then in the repo go to
**Settings → Pages → Deploy from a branch → main → /(root) → Save**. Your site appears at
`https://your-username.github.io` within a minute or two.

## The pagination (your "multiple pages")

News and Publications automatically break into pages. Control how many items per page in
`config.json`:

```json
"newsPerPage": 5,
"publicationsPerPage": 5
```

Add as many entries as you want to `news.json` / `publications.json` — the page numbers
(1, 2, 3 …, with Prev/Next) recalculate on their own. With many pages the control
collapses with an ellipsis (1 … 4 5 6 … 12). Changing pages smooth-scrolls back to the
section heading so you keep your place. Publication abstracts stay matched to the right
paper across pages.

## What to edit

### about.json
Name, title, bio, photo, location, email, CV link, skills, social links.
- **Advisor link:** set `advisor_name` to the exact name as written in your `bio`, and
  `advisor_link` to their URL — it becomes a link automatically.
- **Skill chips:** the `colors` field uses Tailwind classes. The theme set is
  `bg-teal-50 text-teal-800 border border-teal-200` (also try `emerald` and `amber`).

### config.json
- `profilePictureSize` — photo size in px.
- `newsPerPage`, `publicationsPerPage` — items per page.
- `fontSizes` — fine-tune publication card text sizes.
- `newsHighlights` — auto-highlight phrases in news text. Each maps a phrase to a `color`
  + `background`, e.g. `"NeurIPS 2025": { "color": "#0f5d55", "background": "#eaf4f1" }`.
  Teal `#0f5d55` / `#eaf4f1` and brass `#a8631a` / `#faf3e9` match the theme.

### publications.json
Per paper: `status` (badge text; `""` hides it), `status_colors`, `year`, `title`,
`authors`, `conference`, `thumbnail`, `description` (the abstract), `tags`, and optional
`paper_link`, `project_link`, `github_link`. Set any link to `null` to hide its button.

### projects.json
`name`, `image`, `stacks`, `github_link`, `details_page`. Empty `""` or `null` hides a link.

## Re-theming colors

Every color is a CSS variable at the top of `index.html` (the `:root` block). Change a few
values and the whole site updates:

```css
--teal-700: #0f5d55;   /* primary buttons, links, active nav */
--teal-600: #13786c;   /* accents, spine */
--brass:    #a8631a;   /* sparing secondary accent */
--ink:      #18292a;   /* main text + serif headings */
--porcelain:#f5f6f3;   /* page background */
```

## Images

Replace the placeholders in `media/` (keep the filenames, or update the paths in the JSON):
profile ~500×500 square; research images ~600×400; paper thumbnails ~400×520 portrait;
logos square PNG with transparent background; and your CV at `media/pdf/CV.pdf`.

## Dependencies

Loaded from CDN (nothing to install): Tailwind CSS (Play CDN), Lucide icons, and the three
Google Fonts. For a fully offline/production build you can vendor these locally and swap
the CDN tags; the Tailwind Play CDN also prints a harmless "production" console note that
vendoring removes.
