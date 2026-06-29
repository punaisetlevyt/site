# Site

Source for my personal website, built with [Hugo](https://gohugo.io/) and deployed automatically to GitHub Pages via GitHub Actions.

## Stack

- **Static site generator:** Hugo (Extended)
- **Hosting:** GitHub Pages
- **CI/CD:** GitHub Actions (`.github/workflows/hugo.yml`)
- **Custom domain:** configured via `CNAME` file + DNS (see below)

## Repository structure

```
.
├── .github/workflows/hugo.yml   # Build & deploy workflow
├── content/                     # Markdown content
├── layouts/                     # Custom layout overrides (if any)
├── static/                      # Static assets (images, etc.)
├── themes/                      # Theme (may be a git submodule — check .gitmodules)
├── hugo.toml                    # Site configuration
└── CNAME                        # Custom domain config for GitHub Pages
```

## How deployment works

Every push to `main` triggers the GitHub Actions workflow, which:

1. Installs the pinned Hugo version (see `HUGO_VERSION` in the workflow file)
2. Checks out the repo, including submodules (for the theme, if applicable)
3. Builds the site with `hugo --minify`
4. Publishes the `public/` output to GitHub Pages

No manual build or upload step is needed — just push to `main` and the live site updates automatically within a couple of minutes. You can watch progress under the repo's **Actions** tab.

## Local development

Requires [Hugo Extended](https://gohugo.io/installation/) installed locally, matching the version pinned in the workflow file.

```bash
# Check your local Hugo version matches the workflow's HUGO_VERSION
hugo version

# Run the local dev server with live reload
hugo server -D

# Build locally to verify before pushing (optional — CI does this too)
hugo --minify
```

If the theme is a git submodule, clone with:

```bash
git clone --recurse-submodules <repo-url>
```

or, if already cloned without it:

```bash
git submodule update --init --recursive
```

## Updating the site

```bash
# 1. Make changes to content/, static/, layouts/, or config as needed

# 2. Preview locally
hugo server -D

# 3. Commit and push
git add .
git commit -m "Describe your change"
git push origin main
```

That's it — the GitHub Actions workflow handles the rest.

### Adding a new post

```bash
hugo new content/posts/my-new-post.md
```

Edit the generated file, set `draft: false` when ready to publish, then commit and push as above.

### Updating the Hugo version

1. Update `HUGO_VERSION` in `.github/workflows/hugo.yml`
2. Update your local Hugo install to match
3. Test locally with `hugo server -D` before pushing

### Updating the theme (if submodule)

```bash
cd themes/<theme-name>
git pull origin main   # or the theme's default branch
cd ../..
git add themes/<theme-name>
git commit -m "Update theme"
git push origin main
```

## Custom domain

The site is served at the domain configured in the `CNAME` file in the repo root and in the **Settings → Pages** section of this repository. DNS is configured at the domain registrar/DNS provider with records pointing to GitHub Pages — see [GitHub's documentation](https://docs.github.com/en/pages/configuring-a-custom-domain-for-your-github-pages-site) for the specific record types required (apex vs. subdomain).

HTTPS is provided automatically by GitHub Pages once DNS is correctly configured and verified.

## Notes

- This repository is public, since GitHub Pages on the free tier requires a public repo (or a paid plan for private repo Pages).
- Build status and history are visible under the **Actions** tab.
