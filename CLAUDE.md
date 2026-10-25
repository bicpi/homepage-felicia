# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Static, German-language business card site for Felicia Rieber (interior designer), live at https://felicia-rieber.de. Plain HTML plus one hand-written CSS file. No build step, package manager, tests or linter.

The repo is **public**. Never put hosting details, credentials or infrastructure specifics into any committed file, including README, CLAUDE.md, commit messages and PR descriptions.

## Structure

`public/` is the entire website and the only thing ever published. Everything outside it (README, CLAUDE.md, `.github/`) stays out of production, so new site files must go into `public/`.

Preview locally with `python3 -m http.server -d public` (http://localhost:8000). The site expects to be served from a domain root, since the back/home links use `/`.

## Site conventions

- `styles.css` looks like Tailwind but is **not**: there is no Tailwind and no build. The utility-style classes in the HTML (`text-mute`, `max-w-lg`, ...) are hand-defined in `styles.css`. When you use a class in HTML, make sure it exists there, and add it if not. Colors come from the CSS variables in `:root`.
- `index.html` and `rechtliches.html` each carry their own `<head>` (title, description, canonical, Open Graph/Twitter tags; JSON-LD only in `index.html`). There is no shared template, so shared head changes must be made in both files.
- URLs in canonical tags, `og:url`, JSON-LD, `sitemap.xml` and `robots.txt` are absolute. Update `sitemap.xml` when adding or removing a page.
- All copy is German (`<html lang="de">`).
- `rechtliches.html` is the legal page (Impressum, Datenschutz). Do not change its wording unless asked.

## Workflow and deployment

- `main` is protected: no direct pushes. Work on a branch, open a PR, and squash-merge it (the only enabled merge method).
- Merging to `main` deploys `public/` to production through `.github/workflows/deploy.yml`, mirroring it exactly (files removed from `public/` disappear from the live site).
- The workflow appends an invisible `<!-- deploy <sha> -->` marker to `index.html` on the CI runner to verify the deploy. Never commit that marker.
- The workflow intentionally depends only on `public/` and `public/index.html`. Keep it free of references to specific pages or files, and if `public/` is ever renamed, change the workflow in the same PR.
- Actions in workflows are pinned to commit SHAs with a version comment. Pin any new action the same way.
