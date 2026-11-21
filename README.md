# JoelFast 5.0.1 — Joel Desk Pro 下载源

本仓库是 **Joel Desk Pro** 桌面端「高速下载」区域的下载源。
应用内的「从 GitHub 下载 JoelFast」按钮会直接拉取本仓库 Release 里的资产。

## Release

| Tag | 资产 | 大小 | SHA256 |
| --- | --- | --- | --- |
| `v5.0.1-joel` | `JoelFastDownloader_5.0.1_x64.exe` | 6,430,720 | `8E4E2601A1A4BD78B6F9C9012C44210D4130E298BA431BA5450793BA3934776A` |
| `v5.0.1`（旧） | `TDM_Fast_5.0.1_x64-portable.exe` | 6,478,136 | `08171D9E43C81422861472668311DD998D18A0539F845FB195EEBFBD7223120A` |

`v5.0.1-joel` 是自有品牌 **JoelFast** 版本，基于本机使用的 TDM Fast 5.0.1 构建：

- 文件名改为 `JoelFastDownloader_5.0.1_x64.exe`
- 版本资源重写：CompanyName `Joel Ting`、ProductName / FileDescription `JoelFast Download Manager`、
  InternalName `JoelFast`、OriginalFilename `JoelFastDownloader_5.0.1_x64.exe`、Copyright © 2026 Joel Ting
- 图标替换为 JoelDeskPro 的蓝色「J」标识（RT_ICON 16/32/48/64/128/256）
- 前端资源（HTML / JS / CSS，共 31 个 brotli 资源）中的品牌文案全部替换：
  标题、侧栏名称、i18n 语言包、关于面板、购买 / 激活页、官网链接等
- 内部标识替换：Tauri identifier `com.freedidi.tdmfast` → `com.joelting.joelapp`、
  自定义协议 `tdmfast://` → `joelapp://`、注册表协议键、托盘 id `tdm-tray` → `jfd-tray`、
  存储键 `tdmfast-cookies` / `tdmfast-info` → `joelapp-cookies` / `joelapp-info`、
  Rust 模块路径 `tdmfast::` → `joelapp::`、构建路径 `C:\Users\LINGDU\.cargo` → `C:\Users\joel01\.cargo`

### 保留的功能端点（未改动，改动会导致功能失效）

| 端点 | 用途 |
| --- | --- |
| `https://activate.tdmfast.com` | 授权激活服务（主） |
| `https://activate1.lingdu80.com` | 授权激活服务（备用） |
| `https://ffmpeg.tdmfast.com/manifest.json` | FFmpeg 内核清单 |
| `https://ffmpeg.actcb.com/manifest.json` | FFmpeg 内核清单（镜像） |

除以上 4 个功能性 URL 外，二进制中不再包含任何原厂品牌字符串（`TDM Fast` / `tdmfast` / `LINGDU` / `freedidi` 均为 0 处）。

## 下载地址

```
https://github.com/joelting02-sys/TDM-Fast/releases/download/v5.0.1-joel/JoelFastDownloader_5.0.1_x64.exe
```

## 应用侧行为

1. 用户在「高速下载」面板点击 **从 GitHub 下载 JoelFast**。
2. 应用调用 GitHub Releases API 解析 `v5.0.1-joel` 的资产直链。
3. 下载到 `~/Downloads/JoelFastDownloader_5.0.1_x64.exe`，先校验大小与 SHA256，再落盘。
4. 校验通过后自动写入 `tdm_exe_path` 并刷新状态，下载区随即切换为「启动并贴合 JoelFast」。

## 校验

```powershell
Get-FileHash .\JoelFastDownloader_5.0.1_x64.exe -Algorithm SHA256
```

## 运行与隔离说明

`v5.0.1-joel` 是**独立发行的一份副本**，与本机正在使用的原版 TDM Fast 完全隔离，互不影响：

| 项目 | 原版 TDM Fast | JoelFast（本仓库发行版） |
| --- | --- | --- |
| 可执行文件 | `%LOCALAPPDATA%\TDM Fast\TDM Fast.exe` | 任意目录下的 `JoelFastDownloader_5.0.1_x64.exe` |
| 数据目录 | `%LOCALAPPDATA%\com.freedidi.tdmfast` | `%LOCALAPPDATA%\com.joelting.joelapp` |
| 注册表协议键 | `HKCU\Software\Classes\tdmfast` | `HKCU\Software\Classes\joelapp`（协议 `joelapp://`） |
| 托盘 id | `tdm-tray` | `jfd-tray` |

要点：

- **可以与原版同时安装**，两者数据目录、协议键、托盘 id 全部独立；原版用户无需任何改动。
- **不要同时运行**：两者都监听本机 `127.0.0.1:37651` 抓取端口，先关掉一个再启动另一个。
- 该副本**未做 Authenticode 签名**（重打包后原厂签名已失效），Windows 可能显示 SmartScreen 提示。
- 首次运行需要创建 WebView2 用户数据目录 `%LOCALAPPDATA%\com.joelting.joelapp\EBWebView`；
  若该目录被系统策略阻止创建，程序会以 `Failed to setup app: Access is denied. (os error 5)` 退出。
- 发行副本仅做品牌与标识重写，功能逻辑（多线程下载、浏览器插件回传、会员激活、FFmpeg 内核）未改动。

## License

本仓库仅供本人设备重新安装使用。原厂 TDM Fast 是 LINGDU / NEXORA SOFTWARE 的商业软件，
其许可与购买请见 <https://activate.tdmfast.com/>。
