# FlyingTerm

[English](README.md) | [中文](README.zh-CN.md)

跨平台 **SSH / SFTP / 隧道** 工作台，支持 Windows、macOS、Linux。  
个人与学习用途 — 详见 [LICENSE](LICENSE)。

| 下载 | 文档 |
|---|---|
| [Gitee Releases](https://gitee.com/flyingtang/flyingterm/releases)（国内推荐） | 本页 |
| [GitHub Releases](https://github.com/flyingtang/flyingterm/releases) | [English](README.md) |

---

## 功能清单

### 会话
- 会话树：文件夹、标签、搜索 — 单击连接
- 导入 `~/.ssh/config`
- 密码 / OpenSSH 私钥 / Agent（Windows：OpenSSH 命名管道 + Pageant）
- 跳板机（ProxyJump）
- 保活、可选终端日志、主机密钥 TOFU
- 快速连接：侧栏输入 `user@host:22`
- 加密会话导出 / 导入（密码 AES-GCM）

### 终端
- 多标签；左右 / 上下 / 四分屏
- 选中复制、Ctrl+Shift+C/V、Ctrl+F
- 双字体（ASCII + 中文）
- 主题、状态栏延迟
- **MultiExec** 广播 + 多行编写（Ctrl+Enter）
- 快捷命令条（片段）
- 本地 Shell（Windows 优先 Git Bash：默认用系统 Git；可选安装包内置 PortableGit）
- 终端录制 / 回放

### 脚本引擎（宏）
- 在已连接 Shell 上录制操作，或回放已保存步骤
- 步骤类型：`send` · `wait` · `expect`（等到输出出现某段文字）
- 入口：左侧 **宏**，或底部面板 **宏**

### 文件与网络
- 双栏 SFTP（浏览、上传下载、拖拽、chmod）
- Local / Remote / Dynamic SOCKS5（默认绑定 `127.0.0.1`）
- 全局隧道面板

### 其它协议与增强
- 串口 · Telnet · **内嵌 RDP（文字剪贴板 + 音频）** · VNC / MOSH（系统客户端）
- X11 转发（Windows 请装 VcXsrv / Xming）
- 系统托盘 · 可选应用锁 · **自动升级**

### 有意不做
完整桌面级 RDP 附加能力（盘符/打印机、GPU）、完整 X.org、Cygwin/MSYS 工具箱 — 请用专用软件。内嵌 RDP 覆盖连接、画面、键鼠、**文字剪贴板与音频**；仍可「用系统客户端打开」。

---

## 安装

### Windows
1. 从 [Gitee](https://gitee.com/flyingtang/flyingterm/releases) 或 [GitHub](https://github.com/flyingtang/flyingterm/releases) 下载 `FlyingTerm_*_x64-setup.exe`。
2. 运行安装程序（需 Windows 10 1809+ / 11；安装包内嵌 WebView2 **离线**安装组件，不依赖微软 CDN）。
   - 若提示「安装被中止 / 无法写入文件」：先完全退出 FlyingTerm（含托盘），在任务管理器结束残留的 `FlyingTerm.exe`，再重跑安装。
   - 若仍提示 WebView2 失败：先手动安装 [WebView2 Runtime](https://go.microsoft.com/fwlink/p/?LinkId=2124703)，再重跑安装包。
3. 从开始菜单启动 **FlyingTerm**。

### macOS
1. 下载 `.dmg`（建议 universal）。
2. 拖到「应用程序」（需 macOS 10.15+）。

### Linux
1. 发行版兼容优先用 **AppImage**；Ubuntu 22.04+ / Debian 12+ / Fedora 37+ 也可装 `.deb` / `.rpm`（需 WebKitGTK 4.1）。

---

## 快速上手

1. **快速连接** — 侧栏输入 `user@host:22` 回车。
2. **新建会话** — 工具栏 / 右键 → 填主机与认证 → 保存并连接。
3. **SFTP** — 连上 SSH/SFTP 后打开文件面板或底部 **SFTP**。
4. **宏** — 连上 Shell → 宏 → 录制 → 在终端操作 → 停止 → 播放。
5. **设置** — 字体、主题、语言、托盘、统计、加密备份、检查更新。

数据目录：
- Windows：`%APPDATA%\flyingterm\`
- macOS：`~/Library/Application Support/flyingterm/`
- Linux：`~/.local/share/flyingterm/`

---

## 自动升级（给使用者）

1. 能访问 **Gitee**（推荐），或更新接口 / GitHub 备用源。
2. 默认启动时检查更新（设置 →「启动时检查更新」）。
3. 也可 **设置 → 立即检查**。
4. 有新版本时点 **更新并重启**。
5. 国内用户优先走 **Gitee** 拉取安装包，大文件不走厂商云主机流量。
6. 若厂商更换了签名密钥，需**手动安装一次**新包，之后可继续自动更新。

---

## 脚本引擎提示

| 步骤 | 含义 |
|---|---|
| `send` | 向终端写入文本 |
| `wait` | 延时（`delayMs` 或数字 `value`） |
| `expect` | 等到输出包含 `value`（超时 = `delayMs`，默认 10 秒） |

需要带 **Shell** 的标签（SSH / 本地 / Telnet / 串口）。纯 SFTP、RDP、VNC、MOSH 不能跑宏。

---

## 支持与镜像

| 渠道 | 地址 |
|---|---|
| Gitee（国内） | https://gitee.com/flyingtang/flyingterm |
| GitHub | https://github.com/flyingtang/flyingterm |

Issue 与发版说明会尽量在两边同步。

**维护者：** 同步 `open/`、申请 Gitee/GitHub Token、一条命令发版 → 见 [PUBLISH.md](PUBLISH.md)。

---

## 许可证

非商业用途 — 见 [LICENSE](LICENSE)。商业使用需另行授权。
