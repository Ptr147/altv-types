# 您的第一个资源

本文将解释如何创建您的第一个资源。

## 通用服务器结构

我们将继续解释通用服务器结构以及如何添加资源。
```
server/
├── cache/
├── data/
├── modules/
├── resources/
    ├── chat/
    ├── freeroam/
    ├── voice-chat/
    ├── example/
├── altv-server.exe
├── libnode.dll
├── server.toml
```

让我们创建一个示例资源。

```
这是资源的名称:
example/

以下是客户端文件:
example/client/

以下是服务端文件：
example/server/

以下是配置和信息:
example/resource.toml
```

## 配置 `resource.toml`

当您创建资源时。在toml中，它应该类似于下面这样:
```toml
type = "js"
main = "server/server.js"
client-main = "client/client.js"
client-files = [
    "client/*"
]
deps = [ 
    "chat"
]
```

**deps** 允许您请求其他资源，如果没有找到，您的资源将加载失败!

你可以通过在代码中添加以下代码来访问导出的函数及其他:
```js
import * as chat from 'chat';
```

**main** 是主服务器文件。

**client-main** 是客户端为播放器加载的第一个文件。

**client-files** 通常只包含 client/*， 它将加载 **所有** 文件.

## 编写服务器端代码 - `server.js`

创建服务器端代码 - server.js
我们将添加 **chat** 作为资源依赖。
然后我们要导入 'alt' 和 'chat'。

```js
// alt:V内置模块，提供服务器端API。
import * as alt from 'alt-server';
// 聊天资源模块。
import * as chat from 'chat';

console.log('==> Your Resource Has Loaded! Horray!');

// 在服务器端调用
alt.on('playerConnect', (player) => {
    // 记录日志到控制台。
    alt.log(`==> ${player.name} has connected.`);

    // 向所有玩家显示消息。
    chat.broadcast(`==> ${player.name} has joined.`);

    // 设置玩家模型。
    player.model = 'mp_m_freemode_01';

    // 在坐标x, y, z处生成玩家。
    player.spawn(813, -279, 66);

    // 发送给传入的玩家、事件名称、要发送的参数。
    alt.emitClient(player, 'Server:Log', 'hello', 'world');
});
```
**恭喜你编写了第一个服务端代码!**<br>
这将会打印到控制台中<br>
在玩家加入时广播到聊天<br>
这将使玩家拥有freeroam模型并生成它们<br>
向客户端发出事件

请参阅下面的客户端部分，了解如何在客户端拦截事件。

## 创建客户端代码 - `client.js`

我们将导入两个重要的alt:V依赖:**natives**和**alt**
```js
// alt:V内置模块，提供客户端API。
import * as alt from 'alt-client';
// alt:V内置模块，提供native API(函数来自GTA V中)。
import * as native from 'natives';

alt.log('Client-side has loaded!');

alt.onServer('Server:Log', (msg1, msg2) => {
    alt.log(`Message From Server: ${msg1}`);
    alt.log(`Message From Server: ${msg2}`);
});
```

## 使用 TypeScript

要利用TypeScript中的所有优势，需要三个步骤:
1) 设置 `package.json`
2) 安装类型
3) 创建一个tsconfig文件

### 设置 package.json

如果你的项目中还没有`package.json `。根据你的偏好，在resources文件夹中打开一个终端并执行以下命令 
软件包管理器:
```
# 使用npm安装
npm init
# 使用yarn安装
yarn init
```

按照下一个向导操作后，打开`package.json`。使用文本编辑器创建一个Json文件，并添加以下条目:

```not_json
{
    # 以下是您在向导中提供的值
    "name": "altv-client",
    ...
    # 在底部添加这一行
    "type": module
}
```

### 安装类型

| 类型                 | 用于    |
|----------------------|-----------------|
| @altv/types-shared   | Client & Server |
| @altv/types-client   | Client          |
| @altv/types-natives  | Client          |
| @altv/types-webview  | Client          |
| @altv/types-server   | Server          |

根据所需的类型，这些现在必须安装。如果项目包含服务器和客户端的脚本，则可以安装所有类型的脚本:

```yaml
# 仅限客户端
# 使用npm安装
npm i -D @altv/types-shared @altv/types-client @altv/types-natives @altv/types-webview
# 使用yarn安装
yarn add -D @altv/types-shared @altv/types-client @altv/types-natives @altv/types-webview

# 仅限服务器端
# 使用npm安装
npm i -D @altv/types-shared @altv/types-server
# 使用yarn安装
yarn add -D @altv/types-shared @altv/types-server

# 联合安装
# 使用npm安装
npm i -D @altv/types-shared @altv/types-client @altv/types-natives @altv/types-webview @altv/types-shared @altv/types-server
# 使用yarn安装
yarn add -D @altv/types-shared @altv/types-client @altv/types-natives @altv/types-webview @altv/types-shared @altv/types-server
```

### 创建一个tsconfig文件

现在在资源文件夹中创建一个新文件并将其命名为`tsconfig.json`。然后用你最喜欢的文本编辑器打开它。

插入以下内容，并根据需要进行调整。
有些选项在这里不讨论了，因为它们与alt:V没有直接关系，已经详细介绍过了[TypeScript 文档](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html).

```yaml
{
  "compilerOptions": {
    "lib": [ "es5", "es6", "esnext" ],
    "module": "esnext",
    "moduleResolution": "node",
    "target": "esnext",

    "resolveJsonModule": true,
    "esModuleInterop": true,
    "removeComments": true,
    "experimentalDecorators": true,
    "emitDecoratorMetadata": true,
    "preserveConstEnums": true,

    "noImplicitAny": true,
    "sourceMap": false,

    # 如果你有一个子文件夹，所有的ts文件都在其中，你可以添加下面这行来指定根文件夹
    "rootDir": "./Source/",
    # 在这里，你可以设置生成javascript文件的输出文件夹
    "outDir": "./Dist/",

    # 在这里指定alt:V类型所在的TypeScript (IDE)
    "typeRoots": ["./node_modules/@altv"]
  },
  "compileOnSave": true,
  "include": [
    "./**/*.ts"
  ],
  "exclude": [
    "node_modules"
  ]
}
```

## 最后的话

首先，恭喜你编写了你的第一个客户端代码，它打印到` F8 `控制台=>打开控制台查看它。
接下来设置一个具有基础知识的整个服务器!

## 忠告

请记住...
- alt:V 使用ES6模块
- 服务器不会自动更新，你可以使用类似 [altv-pkg](https://github.com/Stuyk/altv-pkg) (非官方)轻松更新服务器文件
