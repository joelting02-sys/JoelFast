# TDM Fast 5.0.1 — Joel Desk Pro 下载源

本仓库是 **Joel Desk Pro** 桌面端「高速下载」区域的下载源。
应用内的「从 GitHub 下载 TDM Fast」按钮会直接拉取本仓库 Release 里的资产。

## Release

| Tag | 资产 | 大小 | SHA256 |
| --- | --- | --- | --- |
| `v5.0.1` | `TDM_Fast_5.0.1_x64-portable.exe` | 6,478,136 | `08171D9E43C81422861472668311DD998D18A0539F845FB195EEBFBD7223120A` |

该文件即本机正在使用的 TDM Fast 5.0.1 主程序（`C:\Users\<user>\AppData\Local\TDM Fast\TDM Fast.exe`），
版本与 SHA256 完全一致，属于原样镜像，未作任何改动。

## 下载地址

```
https://github.com/joelting02-sys/TDM-Fast/releases/download/v5.0.1/TDM_Fast_5.0.1_x64-portable.exe
```

## 应用侧行为

1. 用户在「高速下载」面板点击 **从 GitHub 下载 TDM Fast 官方安装包**。
2. 应用调用 GitHub Releases API 解析 `v5.0.1` 的资产直链。
3. 下载到 `~/Downloads/TDM_Fast_5.0.1_x64-portable.exe`，先校验大小与 SHA256，再落盘。
4. 校验通过后自动写入 `tdm_exe_path` 并刷新状态，下载区随即切换为「启动并贴合 TDM」。

## 校验

```powershell
Get-FileHash .\TDM_Fast_5.0.1_x64-portable.exe -Algorithm SHA256
```

## License

TDM Fast 是 LINGDU / NEXORA SOFTWARE 的商业软件，本仓库仅供本人设备重新安装使用。
软件许可与购买请见 <https://activate.tdmfast.com/>。
