# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Gregory Kondas' personal academic site (`gkondas.github.io`), built from the
[Academic Pages](https://github.com/academicpages/academicpages.github.io) Jekyll template
(itself a detached fork of Minimal Mistakes). GitHub Pages builds and deploys `master`
automatically — there is no CI workflow in this repo, and `_site/` is generated output that is
gitignored and never committed.

## Commands

```bash
bundle install                        # ruby deps (delete Gemfile.lock first if it errors)
./run.sh                              # == bundle exec jekyll serve -l -H localhost → localhost:4000
npm run build:js                      # re-bundle assets/js/main.min.js after editing assets/js/**
npm run watch:js                      # same, on change
```

No tests, no linter. Verification = the local server rendering the page.

## Content model

Everything user-facing is front-matter markdown, not code. Adding content means adding a file:

- `_pages/` — standalone pages (`about.md` is the homepage, `cv.md`, `publications.md`, …).
- `_publications/`, `_talks/`, `_teaching/`, `_portfolio/` — Jekyll collections, all declared in
  `_config.yml` with `permalink: /:collection/:path/`. Filenames are date-prefixed
  (`YYYY-MM-DD-slug.md`) and each file's front matter must set `collection:` and an explicit
  `permalink:` (note: `/publication/...` singular for publications, matching the archive pages).
- `_posts/` — blog posts, same date-prefix convention.
- `files/` — PDFs and downloads, served at `/files/<name>`. `images/` — site images.

The collection archive pages (`_pages/publications.md`, `talks.html`, `teaching.html`,
`portfolio.html`) just loop over the collection; they don't need editing when content is added.

Site-wide identity (author name, bio, avatar, social/academic links, analytics) lives in the
`author:` block of `_config.yml`; the nav bar in `_data/navigation.yml`.

## Theme layer

`_layouts/` → `_includes/` → `_sass/`, standard Minimal Mistakes structure. `_sass/_variables.scss`
holds the theme's colors/typography knobs; Jekyll compiles `assets/css/main.scss` itself.
JavaScript is the exception: `assets/js/main.min.js` is a checked-in uglify bundle of
`assets/js/plugins/*` + `assets/js/_main.js`, so edits to those sources only take effect after
`npm run build:js`.

Because this is a customized fork, upstream template changes cannot be merged cleanly — patch
manually rather than syncing.

## Ancillary

- `markdown_generator/` — optional notebooks/scripts that turn `publications.tsv` / `talks.tsv`
  into collection markdown files. Not part of the build; only run deliberately.
- `talkmap.py` / `talkmap.ipynb` / `_pages/talkmap.html` — geocodes talk locations into a Leaflet
  map under `talkmap/`. Also opt-in.
- `nemo/` — unrelated Python scratch work committed into this repo; not wired into the site.
