---
title: 「草稿」 Windows 开关机脚本
abbrlink: 43cd8705
date: 2023-07-28 21:34:42
categories: Windows
tags:
  - Windows
  - GroupPolicy
  - Scripts
  - PowerShell
banner:
  bannerText: Windows GroupPolicy Scripts
---

## Introduction

## 文件位置

```text
C:\Windows\System32\GroupPolicy\
├── Machine\
│   └── Scripts\
│       ├── psscripts.ini
│       ├── scripts.ini
│       ├── Shutdown\
│       └── Startup\
└── User\
    └── Scripts\
        ├── Logoff\
        └── Logon\
```

## 文件格式

### `scripts.ini`

`bat` 脚本描述文件

```ini
# 关机运行
[Shutdown]
0CmdLine=script_1.bat
0Parameters=
1CmdLine=script_2.bat
1Parameters=

# 开机运行
[Startup]
0CmdLine=script_3.bat
0Parameters=
```

### `psscripts.ini`

`PowerShell` 脚本描述文件

```ini
# 优先运行
[ScriptsConfig]
EndExecutePSFirst=true

# 关机运行
[Shutdown]
0CmdLine=script_1.ps1
0Parameters=
1CmdLine=script_2.ps1
1Parameters=

# 关机运行
[Startup]
0CmdLine=script_3.ps1
0Parameters=
```

## Other

### `Bootstrap.bat`

```batch
@echo off
for /f %%i in ('dir /s /b *.ps1') do (
    powershell -noprofile -nologo -executionpolicy bypass -File %%i
)
```

### `EnableProxy.ps1`

```powershell
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings" ProxyEnable -Value 1
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings" ProxyServer -Value "127.0.0.1:8080"
```

### `DisableProxy.ps1`

```powershell
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings" ProxyEnable -Value 0
Set-ItemProperty -Path "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings" ProxyServer -Value ""
```