# FlyingTerm

[English](README.md) | [中文](README.zh-CN.md)

Cross-platform **SSH / SFTP / tunnel** workbench for Windows, macOS, and Linux.  
Personal and educational use — see [LICENSE](LICENSE).

| Download | Docs |
|---|---|
| [Gitee Releases](https://gitee.com/flyingtang/flyingterm/releases) (recommended in mainland China) | This README |
| [GitHub Releases](https://github.com/flyingtang/flyingterm/releases) | [中文说明](README.zh-CN.md) |

---

## Feature list

### Sessions
- Session tree with folders, tags, and search — click to connect
- Import `~/.ssh/config`
- Password / OpenSSH private key / Agent (Windows: OpenSSH named pipe + Pageant)
- Jump host (ProxyJump)
- Keepalive, optional terminal log, TOFU host-key pinning
- Quick connect: `user@host:22` in the sidebar
- Encrypted session export / import (password-protected AES-GCM)

### Terminal
- Multi-tab; split left-right / top-bottom / four panes
- Copy-on-select, Ctrl+Shift+C/V, Ctrl+F
- Dual fonts (ASCII + CJK)
- Themes and status-bar latency
- **MultiExec** broadcast + multi-line compose (Ctrl+Enter)
- Quick-commands bar (snippets)
- Local shell (Windows: prefers Git Bash — bundled MinGit or system Git)
- Terminal recording / playback

### Script engine (macros)
- Record actions on a live shell, or replay saved steps
- Step types: `send` · `wait` · `expect` (wait for substring in output)
- Open **Macros** in the side rail or bottom panel

### Files & network
- Dual-pane SFTP (browse, upload/download, drag-drop, chmod)
- Local / Remote / Dynamic SOCKS5 tunnels (default bind `127.0.0.1`)
- Global tunnel panel

### Extra protocols
- Serial · Telnet · **embedded RDP (clipboard + audio)** · VNC / MOSH (system clients)
- X11 forwarding (Windows: VcXsrv / Xming)
- System tray · optional app lock · **auto-update**

### Not included (by design)
Full desktop-product RDP extras (drive/printer redirect, GPU), full X.org bundle, Cygwin/MSYS toolbox — use dedicated tools. Embedded RDP covers connect + video + keyboard/mouse + **text clipboard + audio**; “Open with system client” remains available.

---

## Install

### Windows
1. Download `FlyingTerm_*_x64-setup.exe` from [Gitee](https://gitee.com/flyingtang/flyingterm/releases) or [GitHub](https://github.com/flyingtang/flyingterm/releases).
2. Run the installer (Windows 10 1809+ / 11; setup embeds the WebView2 **offline** installer — no Microsoft CDN required).
   - If WebView2 still fails: install [WebView2 Runtime](https://go.microsoft.com/fwlink/p/?LinkId=2124703) manually, then re-run setup.
3. Start **FlyingTerm** from the Start menu.

### macOS
1. Download the `.dmg` (universal preferred).
2. Open and drag FlyingTerm to Applications (macOS 10.15+).

### Linux
1. Prefer **AppImage** for broad distro support, or install `.deb` / `.rpm` on Ubuntu 22.04+ / Debian 12+ / Fedora 37+ (needs WebKitGTK 4.1).

---

## Quick start

1. **Quick connect** — type `user@host:22` in the sidebar and press Enter.
2. **New session** — toolbar / context menu → fill host, auth, Save & Connect.
3. **SFTP** — connect SSH/SFTP, open the Files panel or bottom **SFTP** tab.
4. **Macros** — connect a shell → Macros → Record → type in terminal → Stop → Play.
5. **Settings** — fonts, theme, language, tray, telemetry, encrypted backup, updates.

Data directory:
- Windows: `%APPDATA%\flyingterm\`
- macOS: `~/Library/Application Support/flyingterm/`
- Linux: `~/.local/share/flyingterm/`

---

## Auto-update (for users)

1. Keep network access to **Gitee** (preferred), or the update API / GitHub fallback.
2. By default the app checks on startup (Settings → “Check for updates on start”).
3. Or open **Settings → Check now**.
4. If a newer signed build exists → **Update and restart**.
5. Mainland China: updates normally resolve via **Gitee** first so traffic does not hit the vendor cloud for large installers.
6. After the vendor rotates signing keys, install the new package **once manually**; then auto-update works again.

---

## Script engine tips

| Step | Meaning |
|---|---|
| `send` | Write text to the terminal |
| `wait` | Delay (`delayMs` or numeric `value`) |
| `expect` | Wait until output contains `value` (timeout = `delayMs`, default 10s) |

Requires a **shell** tab (SSH / local / telnet / serial). Pure SFTP / RDP / VNC / MOSH cannot run macros.

---

## Support & mirrors

| Channel | URL |
|---|---|
| Gitee (CN) | https://gitee.com/flyingtang/flyingterm |
| GitHub | https://github.com/flyingtang/flyingterm |

Issues and release notes are published on both mirrors when possible.

**Maintainers:** how to sync `open/`, get Gitee/GitHub tokens, and one-command release → [PUBLISH.md](PUBLISH.md).

---

## License

Non-commercial — see [LICENSE](LICENSE). Commercial use requires a separate license.
