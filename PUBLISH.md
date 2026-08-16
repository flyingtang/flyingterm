# Publishing FlyingTerm to public mirrors

Public user-facing repos (docs + release staging live under `open/`):

- https://github.com/flyingtang/flyingterm
- https://gitee.com/flyingtang/flyingterm

The development tree stays in the private/monorepo FlyingTerm project.  
`open/` is what end users should see as the repository root.

---

## 中文：一条命令发布（推荐）

### 1. 本地配置文件（token **不要**提交到 git）

在 **你准备执行命令的目录**（一般是 FlyingTerm 开发仓根目录）:

```bash
./tools/publish-open.sh --init
# 生成: ./publish-open.local.env
```

或手动：

```bash
cp tools/publish-open.env.example publish-open.local.env
```

编辑 `publish-open.local.env`：

| 项 | 含义 |
|---|---|
| `OPEN_PUBLIC_DIR` | 公开仓本机克隆路径，**相对「执行命令时的当前目录」**；默认 `./flyingterm-public`，不存在会自动 `git clone` |
| `GITHUB_REPO_URL` / `GITEE_REPO_URL` | 公开源码仓库地址（可改） |
| `GITEE_TOKEN` 或 `GITEE_TOKEN_FILE` | Gitee API 令牌（上传 Release） |
| `GH_TOKEN` 或 `GH_TOKEN_FILE` | GitHub 令牌（上传 Release，需 `repo` 权限） |

`publish-open.local.env`、`secrets/`、`*.token` 已在 `.gitignore` 中。

也可把 token 放进单独文件（同样勿提交）:

```bash
mkdir -p secrets
echo '你的Gitee令牌' > secrets/gitee.token
# 在 publish-open.local.env 里写:
# GITEE_TOKEN_FILE=./secrets/gitee.token
```

### 2. Token 怎么拿？

**Gitee（`GITEE_TOKEN`）**

1. 打开：https://gitee.com/profile/personal_access_tokens  
2. 生成私人令牌，勾选能管理仓库 / Release / 附件的权限  
3. 复制令牌（只显示一次）写入 `publish-open.local.env` 的 `GITEE_TOKEN=`，或写入 `GITEE_TOKEN_FILE` 指向的文件  

这不是网页日常登录，而是给脚本调 API 用的。

**GitHub（`GH_TOKEN`，上传 Release 必填）**

1. https://github.com/settings/tokens 新建 classic token，勾选 **`repo`**
2. 写入 `publish-open.local.env` 的 `GH_TOKEN=`，或 `GH_TOKEN_FILE` 指向的文件

脚本走 GitHub API 上传，**不依赖**本机安装 `gh`（Windows 上 PATH 里经常没有 `gh`）。

### 3. 一条命令

```bash
# 在 FlyingTerm 开发仓根目录（或任意目录，只要配置里的相对路径按该目录解析）
./tools/publish-open.sh --all --version 0.1.0
```

等价于：当前系统签名打包 → 暂存 `open/releases/` → 同步 `open/` 到公开仓并 push GitHub+Gitee → 上传安装包到两边 Release。

注意：

- **安装包不会进 git**。`--sync` 只推文档 / `latest.json`；`.dmg` / `.exe` 靠 `--upload` 传到 GitHub/Gitee **Releases**。
- Mac 上 `--all` / `--build` 默认打 **universal**（x64+arm）。已打好包时用 `--skip-build` 避免重编。
- Windows 只能打 Windows 包；Linux 需在 Linux 上再编。

只要同步文档：

```bash
./tools/publish-open.sh --sync
```

只要补传安装包（Mac/Linux 产物已放进 `open/releases/`）：

```bash
./tools/publish-open.sh --upload --version 0.1.0
```

只预克隆公开仓：

```bash
./tools/publish-open.sh --ensure-clone
```

> **说明：** Windows 只能打 Windows 安装包。Mac/Linux 需在对应机器再跑 `--build --stage`，把产物放进开发仓 `open/releases/` 后执行 `--upload`。

`npm run open:publish -- --help` 可看全部参数。

---

## English: one-command publish

```bash
./tools/publish-open.sh --init
# edit ./publish-open.local.env (tokens, repo URLs, OPEN_PUBLIC_DIR relative to cwd)
./tools/publish-open.sh --all --version 0.1.0
```

- **Gitee token:** https://gitee.com/profile/personal_access_tokens → `GITEE_TOKEN` or `GITEE_TOKEN_FILE`
- **GitHub:** `gh auth login` **or** https://github.com/settings/tokens (`repo`) → `GH_TOKEN` / `GH_TOKEN_FILE`
- **OPEN_PUBLIC_DIR:** local clone of the *public* repo (not the same as nesting secrets in `open/` source tree). Default `./flyingterm-public` under the directory where you run the command; auto-cloned from `GITHUB_REPO_URL`.

Never commit `publish-open.local.env` / `secrets/` / `*.token`.

---

## One-time: push `open/` as the public repo root (manual)

```bash
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

Prefer `./tools/publish-open.sh --sync` after `--init` instead of hand-copying.

Legacy: `node tools/sync-open-remotes.mjs --push` (still needs `OPEN_PUBLIC_DIR`).

## Release checklist (maintainers)

1. Bump `package.json` + `src-tauri/tauri.conf.json` version (same number).
2. Build signed installers (Windows / macOS / Linux as available).
3. Stage: `node tools/stage-open-release.mjs --version X.Y.Z` (or included in `--all`).
4. Optionally copy `open/releases/latest.json` → mycap-server `file/flyingterm/latest.json`.
5. Sync public docs + `latest.json` (`--sync`).
6. Upload Release assets on Gitee **and** GitHub (`--upload`).
7. Smoke-test updater from a mainland network.

## Updater endpoint order (in the app)

1. Gitee raw `releases/latest.json` (CN-friendly, tiny JSON in git)
2. `https://admin.flyingtang.cn/api/v1/noauth/flyingterm/latest.json`
3. GitHub Releases `latest/download/latest.json` (global fallback)

Installers should **not** be hosted on the 600G cloud box — only the small JSON may be served there.
