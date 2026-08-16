# Publishing FlyingTerm to public mirrors

Public user-facing repos (docs + release staging live under `open/`):

- https://github.com/flyingtang/flyingterm
- https://gitee.com/flyingtang/flyingterm

The development tree stays in the private/monorepo FlyingTerm project.  
`open/` is what end users should see as the repository root.

## One-time: push `open/` as the public repo root

```bash
# From flyingterm (dev) repo
cd open
git init
git add .
git commit -m "docs: public FlyingTerm landing + license"
git remote add github git@github.com:flyingtang/flyingterm.git
git remote add gitee git@gitee.com:flyingtang/flyingterm.git
git branch -M master
git push -u github master
git push -u gitee master
```

Later updates: sync files from `open/` and push both remotes.

Or use:

```bash
node tools/sync-open-remotes.mjs
```

(requires `OPEN_GITHUB_DIR` / remotes configured — see script header).

## Release checklist (maintainers)

1. Bump `package.json` + `src-tauri/tauri.conf.json` version (same number).
2. `npm run build:nsis` (and mac/linux as needed) with signing key present.
3. `node tools/stage-open-release.mjs --version X.Y.Z`  
   - copies artifacts into `open/releases/`  
   - rewrites `latest.json` download URLs to **Gitee**  
4. Copy `open/releases/latest.json` → mycap-server `file/flyingterm/latest.json` and deploy.
5. Commit `open/releases/latest.json` (+ README changes) and push public `master` (Gitee + GitHub).
6. Create Release **vX.Y.Z** on Gitee **and** GitHub; upload installers + `.sig` (same filenames as in `latest.json`).
7. Smoke-test in mainland network: old build → update dialog → installs from Gitee URL.

## Updater endpoint order (in the app)

1. Gitee raw `releases/latest.json` (CN-friendly, tiny JSON in git)
2. `https://admin.flyingtang.cn/api/v1/noauth/flyingterm/latest.json` (config without waiting for git push; still points downloads at Gitee)
3. GitHub Releases `latest/download/latest.json` (global fallback)

Installers should **not** be hosted on the 600G cloud box — only the small JSON may be served there.
