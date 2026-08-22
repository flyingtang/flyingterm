# Release artifacts (installers)

Put signed build outputs here before uploading to **Gitee** / **GitHub** Releases.

## Layout

```
open/releases/
  latest.json                    # slim updater (committed; small)
  latest-full.json               # full WebView2 updater (Windows installer URL → GitHub)
  latest.json.example
  latest-full.json.example
  FlyingTerm_*_x64-setup.exe              # slim (Gitee)
  FlyingTerm_*_x64-setup-webview2.exe     # full (GitHub; ≥100MB)
  *.sig / *.dmg / *.AppImage / *.deb / *.rpm
```

Large binaries are **gitignored**. Only `latest.json` / `latest-full.json` (and docs) should be committed to the public repo so clones stay small. Upload installers as **Release assets**: slim → Gitee + GitHub; full WebView2 → GitHub only (Gitee 100MB cap).

## `latest.json` rules

1. Build with `npm run build:nsis` (or mac/linux scripts) so artifacts are signed.
2. Copy Tauri-generated `latest.json` from `src-tauri/target/release/bundle/` (or platform subfolder).
3. Slim `latest.json` Windows URL → **Gitee**. Full `latest-full.json` Windows URL → **GitHub** (`*-setup-webview2.exe`).
4. Commit/push both JSON files to the public repo (`master`):

   `https://gitee.com/flyingtang/flyingterm/raw/master/releases/latest.json`
   `https://gitee.com/flyingtang/flyingterm/raw/master/releases/latest-full.json`

5. Last-resort copies are proxied by cloudnote at `/api/v1/noauth/flyingterm/latest.json` and `latest-full.json`.
6. Create a Release tag `vX.Y.Z` on **both** Gitee and GitHub. Upload slim installers to both; upload the full WebView2 exe to GitHub only. Also attach `latest.json` + `latest-full.json` as GitHub Release assets (`releases/latest/download/…`).

Helper: from the FlyingTerm repo root:

```bash
node tools/stage-open-release.mjs --version 0.1.0
```

See `../PUBLISH.md`.
