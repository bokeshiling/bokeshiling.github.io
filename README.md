# My Hexo Blog

Source for my personal blog, built with [Hexo](https://hexo.io) 7.3 and the
default `landscape` theme. The published site lives at
**https://bokeshiling.github.io**.

## Repository layout

```
source/_posts/          Markdown posts (the only content directory in use)
scaffolds/              Templates used by `hexo new`
_config.yml             Site-wide Hexo configuration
.github/workflows/      GitHub Actions deployment pipeline
themes/                 Empty — theme is installed via npm, not vendored here
```

Generated output (`public/`, `db.json`, `.deploy_git/`) is gitignored. It
gets rebuilt from source on every push, so there is nothing to commit by hand.

## Writing a new post

```bash
npx hexo new post "My post title"
```

This creates `source/_posts/My-post-title.md` from `scaffolds/post.md`.
Existing posts use Chinese titles directly as filenames — that works fine.

Each post needs Hexo front-matter at the top:

```yaml
---
title: 参观寺庙见女师傅诵经有感
date: 2026-05-21 10:00:00
tags:
---
```

The site permalink format is `:year/:month/:day/:title/`, so the `date`
field determines the URL. Changing `date` on an already-published post
will change its URL and break inbound links.

## Local preview

```bash
npm install         # first time only
npm run server      # serves at http://localhost:4000 with live reload
npm run build       # one-off static build into public/
npm run clean       # clear public/ and db.json if the build gets stuck
```

There is no `npm run deploy` — deployment is automatic (see below). Do not
add `hexo-deployer-git` back; the old `hexo deploy` flow would push generated
output directly to `main` and overwrite the source.

## Deployment

Every push to `main` triggers `.github/workflows/deploy.yml`:

1. Checkout the repo
2. `npm install`
3. `npx hexo generate` → produces `public/`
4. Upload `public/` as a GitHub Pages artifact
5. `actions/deploy-pages` publishes it

The repo's **Settings → Pages → Source must be set to "GitHub Actions"**
(not "Deploy from a branch"). If that setting drifts back to a branch, the
workflow will succeed but the live site will stop updating.

## Notes

- The repo previously used a two-branch layout (`source` for Markdown,
  `main` for generated HTML) with `hexo deploy` pushing to `main`. That
  flow has been retired. The old `main` was deleted and `source` was
  renamed to `main`, so the current `main` is the source branch.
- Windows downloads sometimes leave `*:Zone.Identifier` files next to
  posts in `source/_posts/`. They are zero-byte NTFS alt-streams and
  should be deleted; Hexo will warn about them otherwise.
