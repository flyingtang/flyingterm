# Publishing FlyingTerm to public mirrors

Public user-facing repos (docs + release staging live under `open/`):

- https://github.com/flyingtang/flyingterm
- https://gitee.com/flyingtang/flyingterm

The development tree stays in the private/monorepo FlyingTerm project.  
`open/` is what end users should see as the repository root.

---

## 中文：一条命令发布（所有平台相同）

### 1. 首次配置（token **不要**提交）

```powershell
npm run release -- --init
```

编辑生成的 `publish-open.local.env`：

| 项 | 含义 |
|---|---|
| `OPEN_PUBLIC_DIR` | 公开仓本机克隆路径（相对当前目录；默认 `./flyingterm-public`，不存在会自动 clone） |
| `GITHUB_REPO_URL` / `GITEE_REPO_URL` | 公开仓库地址 |
| `GH_TOKEN` 或 `GH_TOKEN_FILE` | GitHub token（`repo` 权限，上传 Release） |
| `GITEE_TOKEN` 或 `GITEE_TOKEN_FILE` | Gitee token（上传 Release，可选） |

### 2. 日常发布（Windows / macOS / Linux 同一条）

```powershell
npm run release
```

可选指定版本（默认读 `package.json`）：

```powershell
npm run release -- --version 0.1.1
```

脚本会**自动识别当前系统**并完成：

| 本机系统 | 默认产物 |
|---------|---------|
| Windows | 默认打 **两份** NSIS：精简 `*_x64-setup.exe`（不内嵌 WebView2，走 Gitee）+ 完整 `*_x64-setup-webview2.exe`（内嵌离线包，走 GitHub）。只要精简：`--skip-full`。需要 MSI 时加 `--bundles nsis,msi`。默认不内置 Git Bash；需要时：`npm run build:nsis:gitbash` |
| macOS | universal `.app` / `.dmg`（含双架构远程桌面客户端） |
| Linux | deb + rpm + AppImage |

流程：签名打包 → 暂存 `open/releases/` → 同步公开仓文档 → 上传 GitHub / Gitee **Releases**。

注意：

- 安装包**不会**进 git；`--sync` 只推文档 / `latest.json` / `latest-full.json`。
- 精简版 updater 读 `latest.json`（安装包 URL 指向 Gitee）；完整版读 `latest-full.json`（安装包 URL 指向 GitHub）。Gitee Release 附件上限 100MB，完整版 exe 只上传 GitHub。
- 一条命令只打**当前系统**的包。三端完整发布需在 Win / Mac / Linux 各跑一次 `npm run release`（后跑的会合并进同一 Release tag）。
- 已打好包只需上传：`npm run release -- --skip-build`
- 只要同步文档：`npm run open:publish -- --sync`

### 3. Token

- GitHub：https://github.com/settings/tokens （classic，勾选 `repo`）
- Gitee：https://gitee.com/profile/personal_access_tokens  

---

## English: one command (all platforms)

```bash
npm run release -- --init   # once
# edit publish-open.local.env
npm run release             # or: npm run release -- --version 0.1.1
```

Same command on Windows / macOS / Linux — OS is detected automatically.

| Host OS | Default bundles |
|---------|-----------------|
| Windows | two NSIS builds: slim (no WebView2, Gitee) + full `*-webview2.exe` (GitHub). Use `--skip-full` for slim only. Add msi with `--bundles nsis,msi`. Git Bash is **not** bundled by default |
| macOS | universal app + dmg |
| Linux | deb, rpm, appimage |

Never commit `publish-open.local.env` / `secrets/` / `*.token`.

---

## Manual / advanced

```bash
npm run open:publish -- --help
npm run open:publish -- --build --stage --version 0.1.1
npm run open:publish -- --upload --version 0.1.1
```

Updater feeds (in order):

- Slim: Gitee raw `releases/latest.json` → GitHub `latest.json` → private domain fallback
- Full WebView2: same order on `latest-full.json`; installer URL always GitHub
