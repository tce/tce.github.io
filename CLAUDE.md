# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

Personal academic website for Thomas C. Eskridge (https://thomaseskridge.com), built with Jekyll 3.9 on the **al-folio** theme. It is a GitHub *user* page (`tce.github.io`), so:

- **`source`** is the branch holding the Jekyll source (work here).
- **`master`** holds the *built* static site and is overwritten by the deploy script. Never edit or commit source changes to `master`.

## Commands

```bash
bundle install
bundle exec jekyll serve      # local preview at http://localhost:4000
bundle exec jekyll build      # build into _site/ (this is all bin/cibuild does)
./bin/deploy --user           # build `source` and force-push the result to `master`
```

- There are no tests or linters; a successful `jekyll build` is the check. BibTeX syntax errors in `_bibliography/papers.bib` surface as jekyll-scholar build failures.
- `bin/deploy` prompts for confirmation, refuses to run with uncommitted tracked changes, deletes and recreates the local `master` branch, and force-pushes. It is interactive and outward-facing — let the user run it.
- Local Ruby is 3.4, but the locked gems predate Ruby 3: Jekyll 3.9 calls the removed `File.exists?` (`file-exists.rb` aliases it back, via `RUBYOPT="-r$PWD/file-exists.rb"`), and `bibtex-ruby` 4.4.7 (pinned `~> 4.0` by jekyll-scholar 5.x) fails on Ruby 3 (`tried to create Proc object without a block`). As of 2026-11 the site does not build locally on Ruby 3.4 for that reason.

## Content architecture

- **Publications** (the most frequently edited content): `_bibliography/papers.bib` (managed with BibDesk) is rendered by jekyll-scholar via `_pages/publications.md`, which loops over a hard-coded `years:` list in its front matter and queries `@*[year=Y]*` for each. **A paper whose year isn't in that list won't appear** — add new years there.
  - Each entry is rendered by `_layouts/bib.html`. Custom BibTeX fields it understands: `abbr`, `pdf`, `supp`, `poster`, `slides` (filenames resolved under `assets/pdf/`), `html`. Entries also carry `keywords` (e.g. `PEER`, `SUBMITTED`) and `type` (e.g. `CONF`) fields by convention.
  - Co-author links come from `_data/coauthors.yml`, keyed by last name.
  - `_config.yml` → `scholar.last_name` is still the template's `Einstein`, so the owner's name is not emphasized in author lists.
  - Other `.bib` files (`readinglist.bib`, `toread.bib`, `papers-site.bib`, `*.original`) are not referenced by any page.
- **News**: `_news/*.md` collection, shown on the home page (`_pages/about.md`, `news: true`) via `_includes/news.html`, limited by `news_limit` in `_config.yml`. Use `layout: post`, a full `date:`, and `inline: true` for short announcements that don't get their own page. `oldnews/` holds retired items.
- **Projects**: `_projects/*.markdown`, listed by `_pages/projects.md`; front-matter `img:` sets the card background.
- **Pages**: `_pages/` (about = site index at `/`, publications, projects, teaching). `_pages/about.md` builds to `index.html`; don't add a root `index` page.
- **CV**: `assets/pdf/CV.pdf`, replaced in place when updated.
- Site-wide settings (social IDs, feature toggles like dark mode/math/masonry, CDN library versions with SRI hashes) are in `_config.yml`; theme color and SASS variables in `_sass/`.
