# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

- `npm run server` — start local dev server at http://localhost:4000 with live reload
- `npm run build` — generate static site into `public/`
- `npm run clean` — delete `public/` and `db.json` (run before `build` when troubleshooting stale output)
- `npx hexo new post "<title>"` — scaffold a new post in `source/_posts/` from `scaffolds/post.md`

There is no test or lint setup in this project.

## Architecture

This is a [Hexo](https://hexo.io) 7.3.0 static blog using the default `landscape` theme. Site behavior is driven entirely by `_config.yml` and the contents of `source/`.

**Deployment.** Despite `_config.yml` still declaring a `hexo-deployer-git` target, the live deployment path is **GitHub Actions → GitHub Pages artifact** (`.github/workflows/deploy.yml`): every push to `main` runs `npx hexo generate` and publishes `public/` directly. There is no `source` branch in the current workflow — the README is stale on this point. Do **not** run `npm run deploy` or `hexo deploy` locally; it would push generated output to a separate repo and bypass the Actions pipeline.

**Content layout.** Posts live in `source/_posts/*.md` with standard Hexo front-matter (`title`, `date`, `tags`). Filenames are often Chinese; keep them as-is. The `permalink` is `:year/:month/:day/:title/`, so the `date` field in front-matter determines the URL — changing it changes the published path.

**Generated artifacts.** `public/`, `db.json`, and `.deploy_git/` are all gitignored. The Actions workflow regenerates `public/` from source on every push and uploads it as a Pages artifact — nothing built locally needs to be committed. `.deploy_git/` is a leftover working directory from the old `hexo-deployer-git` flow and can be deleted safely.

## Writing posts

The site language in `_config.yml` is `en` but actual content is Chinese. New posts should follow the existing front-matter pattern (see `source/_posts/参观寺庙见女师傅诵经有感.md`). After adding a post, push to `main` — the Action handles build + deploy.
