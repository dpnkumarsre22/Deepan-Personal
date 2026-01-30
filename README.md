# My Site (Hugo + PaperMod)

A Hugo site using the PaperMod theme, with CI/CD via GitHub Actions deploying to GitHub Pages.

## Getting Started

- Prereqs: Install Hugo (extended) on macOS.

```bash
brew install hugo
hugo version
```

- Run locally:

```bash
hugo server -D
```

- Build production:

```bash
hugo --minify
```

## Configuration

Main config is in [hugo.toml](hugo.toml). Key highlights:
- Theme: `hugo-PaperMod`
- Outputs: home includes `HTML`, `RSS`, `JSON` (enables search)
- UI: reading time, breadcrumbs, TOC, code copy, last modified
- Permalinks: posts under `/posts/:slug/`
- Markup: syntax highlight (`dracula`), TOC, Goldmark renderer unsafe=true
- Privacy: YouTube privacy-enhanced, Twitter DNT

Customize:
- `baseURL`: set your real domain or let CI set it during Pages deploy
- `params.author`, social links, icons under `static/images/`
- Analytics: set `services.googleAnalytics.id`

## CI/CD: GitHub Actions → Pages

Workflow: [.github/workflows/hugo.yml](.github/workflows/hugo.yml)

What it does:
- Checks out repo (with theme submodules if used)
- Sets up Hugo extended (`0.124.1`)
- Configures GitHub Pages (`actions/configure-pages`)
- Builds site with `--minify` and auto `baseURL` from Pages
- Uploads `public/` as artifact and deploys to Pages

Caching for speed:
- Sets `HUGO_CACHEDIR` to `.hugo_cache`
- Caches `.hugo_cache` and `resources/` so subsequent builds are faster
- Enables Go module caching (for Hugo Modules)

Trigger:
- Runs on `push` to `main` and manual `workflow_dispatch`

## Enable GitHub Pages

1. Push code to GitHub:

```bash
git init
git add .
git commit -m "Initialize Hugo site with PaperMod and CI"
git branch -M main
git remote add origin https://github.com/<your-username>/<your-repo>.git
git push -u origin main
```

2. In GitHub → Settings → Pages: Source = "GitHub Actions".
3. Check Actions tab for deployment status; your site URL appears on successful deploy.

### Custom Domain (optional)
- Add your domain in Settings → Pages → Custom domain, or
- Create `static/CNAME` containing your domain name.
- Update DNS to point to GitHub Pages per GitHub docs.

## .gitignore

See [.gitignore](.gitignore):
- Ignores `public/`, `resources/`, `.hugo_build.lock`
- macOS `.DS_Store`, editor files, and optional Node modules

## Troubleshooting

- Build fails on Pages: confirm Hugo version in workflow and that theme assets exist under `static/`.
- Base URL issues: for project/user pages, the workflow sets `--baseURL` from Pages so you can leave `baseURL` generic in [hugo.toml](hugo.toml).
- 404s for assets: ensure referenced images/icons exist in `static/images/` and paths match.
- Slow builds: caching is enabled; speed improves from second run onward.

## Badges (optional)

Add a build status badge to your README (replace owner/repo):

```md
![Deploy Hugo](https://github.com/<owner>/<repo>/actions/workflows/hugo.yml/badge.svg)
```

## References

- Hugo: https://gohugo.io/
- PaperMod theme: https://github.com/adityatelange/hugo-PaperMod
- GitHub Pages via Actions: https://docs.github.com/en/pages

