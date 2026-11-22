# JoelFast 5.0.1 — Joel Desk Pro 下载源

> 仓库地址：<https://github.com/joelting02-sys/JoelFast>（原名 `TDM-Fast` 已改名为 `JoelFast`，旧链接会自动跳转）

本仓库是 **Joel Desk Pro** 桌面端高速下载区的下载源。应用内的「从 GitHub 下载 JoelFast」按钮会直接抓取本仓库 Release 的资产。

## Release

| Tag | 资产 | 大小 | SHA256 |
| --- | --- | --- | --- |
| `v5.0.1-joel` | `JoelFastDownloader_5.0.1_x64.exe` | 6,430,720 | `573f545766b7d227fb022045871969ab39e8932ebd80841bc5cc4d2fdc22a773` |

### 同一 Release 附带的解析内核

| 资产 | 大小 | SHA256 |
| --- | --- | --- |
| `yt-dlp_win.zip` | 18,127,019 | `a981b50fff6e437576d1c3c8df0680af572028ca12dff5e49435d9d33c98294b` |
| `ffmpeg-win64.zip` | 200,462,678 | `2353c41981c5c35e747a262b8d37fbc52d77a42669f754456a841ecd8eee2262` |
| `deno-win64.zip` | 42,917,472 | `cba84b26e0df5ae7f74fc66b86a95b866872a86292ebd84fa1e959927edd93bf` |

工具清单文件：仓库 `main` 分支的 [`ffmpeg-manifest.json`](https://raw.githubusercontent.com/joelting02-sys/JoelFast/main/ffmpeg-manifest.json)（应用内置的清单地址已指向这个链接，不再依赖任何第三方域名）。

## 直接下载

https://github.com/joelting02-sys/JoelFast/releases/download/v5.0.1-joel/JoelFastDownloader_5.0.1_x64.exe

## 应用内行为（4 步）

1. 用户在「高速下载区」点 **从 GitHub 下载 JoelFast**。
2. 应用调用 GitHub Releases API 解析 `v5.0.1-joel` 的资产直链。
3. 下载到 `~/Downloads/JoelFastDownloader_5.0.1_x64.exe`，先校验大小与 SHA256，再落盘。
4. 校验通过后自动写入 `tdm_exe_path` 并刷新状态，下载区随即切换为已就绪，可直接启动并嵌合。

## 校验

```
certutil -hashfile JoelFastDownloader_5.0.1_x64.exe SHA256
```

## 品牌改动清单

- 文件名 → `JoelFastDownloader_5.0.1_x64.exe`
- 版本资源（VS_VERSIONINFO）：CompanyName `Joel Ting`、ProductName / FileDescription `JoelFast Download Manager`、InternalName `JoelFast`、LegalCopyright `Copyright © 2026 Joel Ting`、OriginalFilename / 版本号 `5.0.1`
- 图标 → Joel Desk Pro 的「J」字标（6 个尺寸 PNG，16/32/48/64/128/256）
- 31 个前端资源（brotli 压缩）全部重写：窗口标题、关于页、菜单、i18n（zh-CN / en / zh-TW）、官网链接
- 应用标识 `com.freedidi.tdmfast` → `com.joelting.joelapp`
- 自定义协议 `tdmfast://` → `joelapp://`（注册表 `HKCU\Software\Classes\joelapp`）
- 托盘 id `tdm-tray` → `jfd-tray`
- 存储键 `tdmfast-cookies` / `tdmfast-info` → `joelapp-cookies` / `joelapp-info`
- Rust 模块路径 `tdmfast::` → `joelapp::`
- 构建路径 `C:\Users\LINGDU\.cargo` → `C:\Users\joel01\.cargo`
- HTTP 分段边界 `.tdmpart` → `.jfdpart`、硬件标识前缀 `tdmhw` → `jfdhw`、统计文件 `tdmstats.json` → `jfdstats.json`
- 激活码占位格式 `TDM-XXXX-XXXX-XXXX-XXXX` → `JFD-XXXX-XXXX-XXXX-XXXX`
- 在线激活地址与解析内核清单地址全部重定向到本仓库（不再出现任何第三方域名）

## 标识残留审计

| 检查项 | 出现次数 |
| --- | --- |
| `TDM Fast` / `tdmfast` / `tdm` | 0 |
| `LINGDU` / `freedidi` / `actcb` / `lingdu80` / `nexora` | 0 |
| `tdm-tray` / `.tdmpart` / `tdmhw` / `tdmstats.json` | 0 |
| `tdm_` / `tdm-` / `'TDM'`（前端资源内） | 0 |

唯一残留是 x86 指令字节与 PNG IDAT 数据中的随机字节组合（`tDM` / `TDm`），并非字符串；用 `strings` 扫描得到的也是代码段与压缩数据。

## 运行与隔离说明

| 项目 | 你本机的原版 | 本仓库的 JoelFast |
| --- | --- | --- |
| 数据目录 | `%LOCALAPPDATA%\com.freedidi.tdmfast` | `%LOCALAPPDATA%\com.joelting.joelapp` |
| 注册表协议 | `HKCU\Software\Classes\tdmfast`（`tdmfast://`） | `HKCU\Software\Classes\joelapp`（`joelapp://`） |
| 托盘 | `tdm-tray` | `jfd-tray` |

- 两个版本**不要同时运行**：它们都监听 `127.0.0.1:37651`。
- 本包未做 Authenticode 签名（重打包后原厂签名失效），Windows SmartScreen 可能提示未知发布者。
- 首次运行需要能创建 `%LOCALAPPDATA%\com.joelting.joelapp\EBWebView`（WebView2 用户数据目录）；若该目录创建被系统策略拒绝，会报 `Access is denied. (os error 5)`。

## 许可与来源

- 本构建由本机使用的 5.0.1 便携版重打包并整体重命名，商标、界面、图标、元数据均已替换为 JoelFast / Joel Ting。
- 解析内核为上游公开构建：yt-dlp（Unlicense）、FFmpeg（GPL）、Deno（MIT），其 SHA256 与上游发布一致，本仓库仅作镜像分发。
- 本仓库不提供任何账号、授权码或密钥；解析内核与安装包均以原样文件分发。
