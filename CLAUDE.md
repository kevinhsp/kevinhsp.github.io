# supenghe.com — Supeng He's academic personal website

## Stack and deployment (decided 2026-10-06)
- Quarto website (this repo) → GitHub Pages (branch `gh-pages`) → custom domain supenghe.com.
- GitHub repo: `kevinhsp/kevinhsp.github.io`, public. Source branch `main`.
- Publishing: GitHub Actions `.github/workflows/publish.yml` runs on every push to `main`:
  `quarto-dev/quarto-actions/setup@v2` + `quarto-dev/quarto-actions/publish@v2` with
  `target: gh-pages` and `permissions: contents: write`. The very first publish was done locally on 2026-10-06 with
  `quarto publish gh-pages --no-prompt --no-browser`, after creating an empty `gh-pages` branch on
  origin via `git commit-tree` (Quarto's own branch creation does an orphan checkout in the working
  tree). The gh-pages target creates no `_publish.yml`; none is needed.
- Computations: `execute: freeze: auto` in `_quarto.yml`. Python cells run locally only; `_freeze/`
  is committed so CI never needs Python/Jupyter. After editing any page with code cells, run
  `quarto render` and commit the updated `_freeze/`.
- Custom domain: `CNAME` in the repo root contains `supenghe.com` and is copied into the site via
  `project: resources: [CNAME]`.
- DNS lives on Cloudflare and is managed by hand by Supeng (never via tooling). Required records:
  A `@` → 185.199.108.153, 185.199.109.153, 185.199.110.153, 185.199.111.153;
  CNAME `www` → kevinhsp.github.io. All five must be "DNS only" (grey cloud, not proxied),
  otherwise GitHub cannot issue the TLS certificate. GitHub Settings → Pages: custom domain
  supenghe.com, then "Enforce HTTPS" once the DNS check passes.

## Local tooling (Windows 11)
- Python: project virtualenv `.venv` (Python 3.13, created by PyCharm) with `jupyter` and
  `matplotlib`. Quarto does NOT pick up `.venv` by itself: `_environment.local` (gitignored, machine-specific)
  sets `QUARTO_PYTHON=E:/-personal_web/.venv/Scripts/python.exe`. On a new machine, recreate that file
  or activate `.venv` before running quarto.
- Quarto and GitHub CLI are installed with winget (user scope). This is Windows: no brew.
- Git pushes use gh as the credential helper, configured repo-locally only
  (`git config --local credential.helper`), so the global git config is untouched.
- Line endings: `core.autocrlf=true` is fine; Quarto normalizes newlines before hashing
  frozen inputs, so CRLF/LF differences never invalidate `_freeze/`.
- Commands:
  - `quarto preview --no-browser` — local preview
  - `quarto render` — full render into `_site/`, refreshes `_freeze/`
  - `quarto check` — verify Quarto and Jupyter detection
  - `quarto publish gh-pages --no-prompt --no-browser` — manual publish (CI normally does this)

## Site structure
- `index.qmd` — about page (template `trestles`): `photo.jpg`, name, position line,
  research interests (TODO), email/GitHub/LinkedIn links
- `research.qmd` — Working Papers and Conference Abstracts; every entry has a status line
- `projects.qmd` — listing of `projects/`; each project is `projects/<slug>/index.qmd`
  (may contain Python cells)
- `blog.qmd` — listing of `posts/`; each post is `posts/<slug>/index.qmd`
- `cv.qmd` — links to `cv.pdf` (placeholder PDF until the real CV replaces it)
- `404.qmd`
- `_quarto.yml` — navbar Research · Projects · Blog · CV; footer with email/GitHub/LinkedIn;
  theme flatly + `custom.scss`; `execute: freeze: auto`; `resources: [CNAME]`
- `.gitignore` — `/.quarto/`, `/_site/`, plus local-only `/.venv/`, `/.idea/`, `/_environment.local`
- `custom.scss` — minimal overrides on flatly (deep-blue links and navbar highlight instead of
  flatly's teal). Keep it small; no further SCSS unless needed.
- Look and feel (decided 2026-10-06): conservative business-school style — navy navbar (flatly),
  deep-blue accents, white background, no gimmicks.

## About Supeng (source of truth for placeholders)
- Name: Supeng He
- Position line: Research Assistant with Kan Xu, W. P. Carey School of Business,
  Arizona State University · Applying to PhD programs, Fall 2027
- Education: MS Computer Science, Rice University; MS Business Analytics and Project Management,
  University of Connecticut; BA Business, Drew University (minors in Data Science and Mathematics)
- Email: TODO (not provided yet) · GitHub: kevinhsp · LinkedIn: TODO (not provided yet)
- Research-interest text and paper descriptions come from Supeng. Until provided, use clearly
  marked `TODO` placeholders. Never invent content.
  `grep -rn TODO --include=*.qmd --include=*.yml .` lists everything still to fill in.

## Content rules
- No empty sections, no "coming soon", no skills list, no news feed.
- Placeholders are literal `TODO` strings.

## Working rules
- Talk to Supeng in Chinese (中文). Code, file names and commit messages stay in English.
- Ask before any destructive git operation (reset --hard, force push, branch deletion,
  rewriting history, deleting tracked files).
- Keep explanations short.

## Setup status
- [x] Tooling installed (quarto, gh, python + jupyter + matplotlib)
- [x] Site scaffolded and rendered locally
- [x] GitHub repo created and `main` pushed (https://github.com/kevinhsp/kevinhsp.github.io)
- [x] First `quarto publish gh-pages` done
- [ ] GitHub Actions publish workflow green
- [ ] Pages source set to `gh-pages`
- [ ] Cloudflare DNS records added (Supeng, by hand)
- [ ] Custom domain verified and Enforce HTTPS on (Supeng, by hand)
