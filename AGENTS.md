# Repository Guidelines

## Project Structure & Module Organization

- `source/_posts/` — Markdown posts (the only content directory in use)
- `scaffolds/` — templates used by `npx hexo new`
- `_config.yml` — site-wide Hexo configuration (title, permalink, theme)
- `themes/` — empty; the `landscape` theme is installed via npm
- `.github/workflows/deploy.yml` — CI pipeline that builds and publishes to GitHub Pages
- `public/`, `db.json`, `.deploy_git/` — gitignored generated artifacts; never commit them

## Build, Test, and Development Commands

- `npm install` — install dependencies
- `npm run server` — local server at `http://localhost:4000` with live reload
- `npm run build` — static build into `public/`
- `npm run clean` — clear `public/` and `db.json` when the build gets stuck
- `npx hexo new post "Title"` — scaffold a post in `source/_posts/`

No test or lint tooling exists; verify changes with a clean build and a local preview.

## Coding Style & Naming Conventions

- Posts use the front-matter pattern `title`, `date`, `tags` from `scaffolds/post.md`.
- Keep Chinese filenames as-is, e.g. `source/_posts/参观寺庙见女师傅诵经有感.md`.
- The `date` field sets the permalink (`:year/:month/:day/:title/`); never change it on a published post — it breaks inbound links.
- Use 2-space indentation in YAML files and match the surrounding style.

## Testing Guidelines

No automated tests exist. After changes, run `npm run build` (it must finish without unrendered-file warnings), then `npm run server` and confirm the post renders at its permalink.

## Commit & Pull Request Guidelines

Commit messages follow two patterns: `Publish: <title>` for new posts (e.g. `Publish: 测试文章-你好世界`), and short imperative summaries for infra/config changes (e.g. `Update deploy.yml`).

Every push to `main` deploys, so keep PRs small and focused. Describe what changed and why, confirm the build passes, and flag anything that alters URLs or site behavior. Screenshots are only expected for theme or layout changes.

## Security & Configuration Tips

- Never run `hexo deploy` or re-add `hexo-deployer-git`; it would overwrite the source branch.
- In Settings → Pages, keep the source set to "GitHub Actions", or deploys silently stop updating the live site.
- Delete stray Windows `*:Zone.Identifier` files next to posts; Hexo warns about them.

## Agent-Specific Instructions

- Communicate with the user in Chinese (中文) unless they ask otherwise.
