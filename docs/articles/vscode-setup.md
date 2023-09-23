# 为你的项目配置VSCode

在本文中，我们将重点介绍简单节点的配置和创建内置的脚本。

> [!WARNING]
> 这篇文章仍在进行中。

## 下载 VSCode

在我们开始之前，我们需要下载和安装软件, 所以以防万一你还没有下载，
[这里](https://code.visualstudio.com/#alt-downloads)你可以找到不同平台的所有下载类型。在本文中，我使用的是Windows便携包。

## 选择语言和模型

现在，你需要决定是使用JavaScript还是TypeScript。我会为两者提供配置，但如果你不确定该选择哪个，并且愿意了解TypeScript实现的附加功能，你应该选择TypeScript。
另外，了解你是为客户端、服务器端模型还是两者都创建项目会很有帮助（稍后会提及）。

## Preparing folder for project

在VSCode中单击文件->打开文件夹，此选项将打开文件对话框。
在您的首选目录中创建文件夹，并选择您刚刚创建的文件夹
。所有项目文件都将存储在该文件夹中。

> [!CAUTION]
> 始终将项目作为文件夹打开，而不是单个文件。

## 创建node包

现在我们将介绍如何在项目文件夹中使用终端功能来执行shell脚本。要打开它，请单击"View" -> "Terminal"，这将在编辑器底部显示停靠窗口。终端将使用你系统中的默认shell（对于Windows 10，它是PowerShell）。我会提及可能存在的shell脚本差异。

现在在终端中执行这个脚本
```powershell
npm init --yes
```
它将创建默认的包配置文件，无需询问任何问题。你可以稍后填写它们。

现在你可以安装所选择模型的类型定义，这将被编辑器用于代码验证、自动补全等功能。请查看此[文档](https://github.com/altmp/altv-types/blob/master/README.md#altv-types)获取最新信息。


如果你选择了TypeScript语言作为项目的开发语言，你还需要安装TypeScript包。
```powershell
npm i -D typescript
```

## 创建配置

如果你选择了JavaScript项目，你的配置文件应该命名为 jsconfig.json；如果是TypeScript项目，则应为 tsconfig.json。两者都类似，所以如果你改变主意，稍后切换也不会很困难。

配置文件的内容应该如下所示
```json
{
    "compilerOptions": {
      "module": "es2015",
      "target": "es6",
      "checkJs": true,
      "strict": true,
      "typeRoots": [
        "./node_modules/@types",
        "./node_modules/@altv"
      ],
      "outDir": "out"
    },
    "exclude": ["node_modules"]
  }
```

> [!TIP]
> 注意`` typeoots ``条目，它是alt:V类型生效的必要条件。
> 无需手动定义每个包，如果你希望将使用的包最小化，
> 请改用 types 条目。

## 第一个项目脚本

现在我们可以创建我们的第一个脚本，应该命名为 index.js（如package.json中所描述）。
为了测试是否一切正常运作，你可以复制下面的这段代码。
```js
import * as alt from "alt-client";

alt.setInterval(() => {
    alt.log("This is a self repeating notification every 5 seconds.");
}, 5000);
```

> [!WARNING]
> 如果你正在创建服务器端脚本，可能会出现错误，提示找不到模块 `alt-client`。在这种情况下，只需将其更改为 `alt-server`。

如果一切顺利，你的脚本不应出现任何错误，同时在输入 ``alt.`` 时，你应该能够获得该模块中所有可用类型的自动补全。

## 创建资源配置

要使该项目被语言模块识别，我们需要创建资源配置文件。你可以在[这里](https://docs.altv.mp/js/articles/create-your-first-resource.html#configuring-the-resourcecfg)查看如何操作。
