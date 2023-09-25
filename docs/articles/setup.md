# 使用JS模块设置alt:V服务器

本文将介绍如何使用JS模块设置alt:V服务器。

## 要求

- [NodeJS的最新版本](https://nodejs.org/en/)
- 代码编辑器(Visual Studio Code、Sublime Text等)
- 命令提示符的基本知识
- 安装了VC Redist的最新版本
- Windows 10+、Linux操作系统(服务器使用)

## 总结

如果你有兴趣了解服务器设置的所有细节，请完全遵循本教程。

如果您对在几分钟内运行服务器更感兴趣，请查看[alt:V Quickstart repository](https://github.com/Stuyk/altv-quickstart)并按照这些说明操作。

## 获取服务器文件

[下载](https://altv.mp/#/downloads) 这里是最新版本。
在下拉菜单中导航到:**Release**、**Release candidate**或**Development**\*<br>
**对于开发，大部分时间使用release candidate版本。**

勾选以下选项:
```
[x] Data Files
[x] JS Module
[ ] C# Module
[x] Sample Config File
[x] Example Resource Pack
```

点击**DOWNLOAD**，完成后=>用你喜欢的程序(7-Zip, Winrar等)解压缩

*不要放在桌面的文件夹中！

## 运行服务器

根据你选择的代码编辑器，你可以考虑以下选项:

### Visual Studio Code
```md
- 在Visual Studio Code中，打开服务器根目录(文件>打开文件夹)
- 创建一个新终端(终端>新终端)

一旦你有了一个终端，就看你的了:

PowerShell:
- 选择PowerShell终端
- Type > ./altv-server.exe

命令提示符:
- 选择命令提示符
- Type > altv-server.exe

恭喜，你的服务器现在正在运行

- 按CTRL + C停止服务器。
- 按向上箭头并按enter重启服务器。
```

### PowerShell
```md
- 导航到altv-server.exe的根文件夹
- 转到文件资源管理器的地址栏
- Type > powershell
- Type > ./altv-server.exe

恭喜，你的服务器现在正在运行

- 按CTRL + C停止服务器。
- 按向上箭头并按enter重启服务器。
```

### 命令提示符
```md
只需要运行 altv-server.exe
```

## 配置客户端进行开发

- 切换到alt:V安装的根目录
- 在你的编辑器中打开`altv.toml`

现在应该会出现类似下面的内容:
```toml
branch = 'rc'
build = 1182
debug = true
gtapath = 'D\\Program Files (x86)\\Steam\\steamapps\\common\\Grand Theft Auto V'
ignoreLowPriorityProps = false
lang = 'en'
lastip = ''
name = 'YourWonderfulName'
netgraph = false
streamerMode = false
voiceActivationEnabled = false
voiceActivationKey = 78
voiceEnabled = true
voiceInputSensitivity = 20
voiceVolume = 100
```

### 使用分支

根据你在[获取服务器文件](#obtaining-the-server-files)中选择的内容来设置你的分支。
分支名称:
- release
- release candidate
- dev

```toml
# Release (最多使用):
branch = 'release'

# Release candidate:
branch = 'rc'

# Development:
branch = 'dev'
```

### 使用debug模式（alt:V客户端）

在你的alt:V安装目录下编辑`altv.toml`文件，添加如下代码:
```toml
debug = true
```
这允许使用一些调试功能，如重新连接等与你的F8控制台。

## 配置 `server.toml`

一旦你完成了上述更改，就该再次前往服务器了。
这就是 `server.toml` 神奇的地方，在这里你可以改变最重要的东西。

打开后，应该出现类似下面的内容:
```toml
name = 'alt:V Server'
host = '0.0.0.0'
port = 7788
players = 128
#password = ultra-password
announce = false
#token = YOUR_TOKEN
gamemode = 'Freeroam'
website = 'example.com'
language = 'en'
description = 'alt:V Sample Server'
modules = [ 'js-module' ]
resources = [
  
]
```

## 使用debug模式 (服务器端)

在开发过程中，请确保在`server.toml`中**启用**以下内容。
把这个添加到你的配置中:
```toml
debug = true
```

启用后，你现在可以使用如下调试命令:

`restart [资源名]` (服务器端) <br> 
`stop [资源名]` (服务器端) <br> 
`start [资源名]` (服务器端) <br> 
`reconnect` (客户端)<br> 
*Reconnect可能会导致bug和问题，这是一个实验性的功能，只打算在开发中使用。

请参阅[服务器命令](https://docs.altv.mp/articles/commandlineargs.html#server-commands)了解所有服务器控制台命令，[客户端控制台](https://docs.altv.mp/articles/console.html)了解所有客户端控制台(F8)命令。

## 添加资源

添加资源很简单，只需将文件夹名称放在resources数组中即可。

现在，资源数组是空的。
请记住，当您选择示例资源包时，您将获得三个示例资源。

通过简单地添加文件夹名称来加载它们<br>

*名称区分大小写！—同样的模板[资源名]不会工作，不像其他多人模式修改那样有效。

```toml
resources = [
    'freeroam',
    'chat',
    'newResource'
]
```

## Typescript视频教程

如果你想学习如何使用TypeScript设置alt:V资源，请观看Stuyk创建的视频教程:

<iframe width="1280" height="720" src="https://www.youtube-nocookie.com/embed/UTeNEiyrrIA" frameborder="0" allow="accelerometer; autoplay; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
