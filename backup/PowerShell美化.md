# 一，安装Nerd Font字体

避免出现字符“▯”，Nerd Font字体下载地址https://www.nerdfonts.com/font-downloads

<!--more-->
# 二，安装适用于 PowerShell 的 Oh My Posh
## 1.使用winget安装
```sh
winget install oh-my-posh
```
## 2.使用Microsoft Store安装

![image.png](/static/img/385bf3647827a7f72c8af583c6571c97.image.webp)
## 3.选择并应用 PowerShell 主题
```
notepad $PROFILE
```
### 填写内容
```
oh-my-posh init pwsh --config "$env:POSH_THEMES_PATH\paradox.omp.json" | Invoke-Expression
```

:::note{title="注"}
paradox为主题名称
:::
主题官网：https://ohmyposh.dev/docs/themes
## 三，可能的问题
### 1.notepad $PROFILE可能会提示系统找不到指定的路径，参考Microsoft文档https://learn.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-7.3 中的Profile types and locations部分自行创建，然后重启Windows Terminal即可。<br>
### 2.无法加载文件 ***\WindowsPowerShell\profile.ps1,因为在此系统上禁止运行脚本
查看现用执行策略
```
>> get-executionpolicy
Restricted
```
更改执行策略，以管理员身份打开 PowerShell
```
>> set-executionpolicy remotesigned
```
选择a或者y选项，再次重启Windows Terminal。
### 本文连接源自https://learn.microsoft.com/en-us/windows/terminal/tutorials/custom-prompt-setup


