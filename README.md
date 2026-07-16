# disable-ms-gamebar-popup

## 这是什么？

这是一个用于**彻底屏蔽 Windows 系统gamebar烦人弹窗**的小工具。当你连接手柄或某些游戏操作触发 `ms-gamebar://` 链接时，系统会提示“无法打开此 ms-gamebar 链接，你的设备需要一个新应用”。本脚本通过修改注册表，将 `ms-gamebar` 协议的处理程序指向一个**静默退出命令**，让系统不再弹出任何窗口或询问框。

## 特点

- ✅ **彻底屏蔽** – 不再出现“需要新应用”的蓝色弹窗。
- ✅ **无黑框闪现** – 使用 PowerShell 隐藏窗口模式，执行过程完全无感。
- ✅ **零副作用** – 不删除任何系统文件，不影响其他软件。
- ✅ **一键执行** – 以管理员身份运行，复制粘贴即可。
- ✅ **可逆操作** – 如需恢复，只需删除注册表项 `HKEY_CLASSES_ROOT\ms-gamebar` 即可。

## 使用方法

1. 右键点击“开始”菜单，选择 **Windows PowerShell (管理员)** 或 **终端 (管理员)**。
2. 复制以下代码，粘贴到 PowerShell 窗口中，按回车执行：

   ```powershell
   $k="Registry::HKEY_CLASSES_ROOT\ms-gamebar"
   If (-not (Test-Path $k)) { New-Item -Path $k -Force | Out-Null }
   Set-ItemProperty -Path $k -Name '(Default)' -Value 'URL:Fake Gamebar Protocol' -Force
   New-ItemProperty -Path $k -Name 'URL Protocol' -PropertyType String -Value '' -Force
   $ck="$k\shell\open\command"
   New-Item -Path $ck -Force | Out-Null
   Set-ItemProperty -Path $ck -Name '(Default)' -Value "powershell.exe -WindowStyle Hidden -Command `"exit`"" -Force
