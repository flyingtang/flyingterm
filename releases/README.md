# Release artifacts (installers)

Put signed build outputs here before uploading to **Gitee** / **GitHub** Releases.

## Layout

```
open/releases/
  latest.json          # updater manifest (committed; small)
  latest.json.example  # template
  FlyingTerm_*_x64-setup.exe
  FlyingTerm_*_x64-setup.exe.sig
  *.nsis.zip / *.sig   # if produced by Tauri
  *.dmg / *.AppImage / *.deb / *.rpm + matching .sig
```

Large binaries are **gitignored**. Only `latest.json` (and docs) should be committed to the public repo so clones stay small. Upload installers as **Release assets** on Gitee/GitHub.

## `latest.json` rules

1. Build with `npm run build:nsis` (or mac/linux scripts) so artifacts are signed.
2. Copy Tauri-generated `latest.json` from `src-tauri/target/release/bundle/` (or platform subfolder).
3. Rewrite each `platforms.*.url` to prefer **Gitee** download URLs, e.g.

   `https://gitee.com/flyingtang/flyingterm/releases/download/v0.1.0/<filename>`

   Optional GitHub mirror URL can stay as a second Release upload (same filename).
4. Commit/push `open/releases/latest.json` to the public repo (`master`), so this works:

   `https://gitee.com/flyingtang/flyingterm/raw/master/releases/latest.json`

5. Optional last-resort copy of the same JSON (not the installers) is proxied by cloudnote at:

   `https://note.flyingtang.cn/api/v1/noauth/flyingterm/latest.json`
   `https://flyingtang.cn/api/v1/noauth/flyingterm/latest.json`
6. Create a Release tag `vX.Y.Z` on **both** Gitee and GitHub; upload all installers + `.sig` files.

Helper: from the FlyingTerm repo root:

```bash
node tools/stage-open-release.mjs --version 0.1.0
```

See `../PUBLISH.md`.
