# SkillForge ⚡

**Open-source, fully static marketplace for Grok & Claude skills.**

Browse, preview, and install real, copy-paste-ready skills into your `~/.grok/skills/` directory. No backend, no database, no accounts, no tracking — the entire site is a single `index.html` that works offline after download.

Built by **Phantom Capital** ([@PhantomCap_ai](https://x.com/PhantomCap_ai)).

## What it is

- **100% static** — one `index.html`, zero external dependencies (no CDNs, no build step, no runtime sugar). Works from `file://`, any static host, or GitHub Pages.
- **Real data only** — every skill listed in the marketplace has a matching folder under `/skills/<id>/` containing `skill.md` and `instructions.md`.
- **Dark/light theme**, live search, category filter, favorites (saved locally in your browser), and a skill preview modal with a one-click *Copy Install* action.

## Features

- Live search across title, description, author, and category
- Category filter (derived from the skill data, so it can never drift out of sync)
- Skill preview modal with one-click copy of install instructions (clipboard API + fallback)
- Favorites persisted to `localStorage`
- Full dark/light theme, keyboard-friendly modal (Esc closes, focus trap, focus restore)
- Accessible: semantic landmarks, labeled controls, visible focus, screen-reader live region, `prefers-reduced-motion` support
- Responsive single-file layout (no layout shifts on mobile)

## Quick start (local)

```bash
git clone https://github.com/PhantomCapAI/grok-skills-marketplace.git
cd grok-skills-marketplace
```

Then simply open `index.html` in any modern browser. No install, no build, no server required.

For a slightly nicer browser experience you can also serve it:

```bash
# Python 3
python -m http.server 8000
# or Node.js 18+
npx serve .
```

Then visit `http://localhost:8000`.

## Deploying (GitHub Pages)

The repo already ships a GitHub Actions workflow (`.github/workflows/deploy.yml`) that publishes the `main` branch to Pages on every push.

1. In your repo settings: **Settings → Pages → Source → "GitHub Actions"**.
2. Push to `main`. The `Deploy static content to GitHub Pages` workflow builds and publishes automatically.
3. The site lives at `https://<org>.github.io/grok-skills-marketplace/`.

Works identically on Netlify, Vercel, Cloudflare Pages, or any static host — upload the repo root and go.

## Installing a skill

In the modal for any skill, click **Copy Install**, then paste into your terminal. Each skill's instructions are generated from its real location in this repo:

```bash
git clone --depth 1 https://github.com/PhantomCapAI/grok-skills-marketplace.git
cp -r grok-skills-marketplace/skills/research-beast ~/.grok/skills/
```

Grok loads skills from its `~/.grok/skills/` directory (each skill is a folder containing `skill.md` and `instructions.md`).

## Data format

A skill is a folder under `/skills/<name>/` with two markdown files:

```
skills/<name>/
├── skill.md          # what the skill does (title, author, category, description)
└── instructions.md   # system prompt / behavior instructions
```

The marketplace registry lives inline in `index.html` as the `SKILLS` array (kept inline deliberately so the site remains a single offline-capable file). Each record maps to a real folder:

| Field      | Example                          | Notes                                  |
|------------|----------------------------------|----------------------------------------|
| `id`       | `phantom-finance-analyzer`       | Must match `/skills/<id>/` exactly     |
| `title`    | `Phantom Finance Analyzer`       | Display title                          |
| `author`   | `@PhantomCap_ai`                 | Display author                         |
| `category` | `finance`                        | Slug; label auto-derived for the filter dropdown |
| `icon`     | `📊`                             | Single emoji                           |
| `summary`  | `Advanced financial analysis…`   | Description shown in grid + modal      |
| `install`  | shell commands                   | `\n`-separated install instructions    |

**Marketplace rule:** no registry entry without a real skill folder, and no skill folder that is not listed. If they drift apart, the site still renders deterministically — it only lists what exists.

## Contributing

1. Fork the repo.
2. Copy `skills/_TEMPLATE/` to `skills/<my-skill>/` and fill in `skill.md` + `instructions.md`.
3. Add a matching entry to the `SKILLS` array in `index.html` (see **Data format** above).
4. Open a pull request. Verify the preview works locally by opening `index.html`.

## License

MIT © 2026 Phantom Capital. See [LICENSE](LICENSE).