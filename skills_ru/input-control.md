---
name: input-control
description: 操控键盘鼠标 — 移动、点击、拖拽、滚轮、输入文本、组合键、截图
---

# 调用

所有操作通过 `run_command` 执行：

```
powershell -ep bypass .\ic.ps1 <函数> <参数...>
```

> `-ep` = `-ExecutionPolicy`，确保脚本执行不受策略限制。

无参获取帮助：`powershell -ep bypass .\ic.ps1 Help` — 列出所有函数签名。  
查键名：`powershell -ep bypass .\ic.ps1 VK` — 列出所有可用键名。

连续操作用 `;` 串联（PowerShell 链）：`powershell -ep bypass ". .\ic.ps1; Move 100 200; Wait 200; Click Left"`  
> 链式调用时 dot-source（`. .\ic.ps1`）一次，后续函数共享同一进程，更快且状态连续。  
> 单次操作用 `-File` 模式最省 token。

## SendKeys 特殊字符（Send 函数内）

`^`=Ctrl `+`=Shift `%`=Alt `~`=Enter · `^c`=Ctrl+C `%{F4}`=Alt+F4 `^+{ESC}`=Ctrl+Shift+Esc  
字面字符需包裹：`{^}` `{+}` `{%}` `{~}` `{{}` `{}}` `{(}` `{)}`

## 示例

```
powershell -ep bypass .\ic.ps1 Move 500 300
powershell -ep bypass .\ic.ps1 Click Right
powershell -ep bypass .\ic.ps1 Combo Ctrl Shift Escape
powershell -ep bypass .\ic.ps1 Send "Hello World~"
powershell -ep bypass .\ic.ps1 Drag 100 200 400 500 Left
powershell -ep bypass .\ic.ps1 Shot $env:USERPROFILE\Desktop\screen.png
powershell -ep bypass .\ic.ps1 Pixel 500 300
powershell -ep bypass .\ic.ps1 Scroll -360
powershell -ep bypass ". .\ic.ps1; Move 200 200; Wait 100; DblClick; Wait 200; Send 'done~'"
```
