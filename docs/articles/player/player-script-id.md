# 理解 ScriptID

这是我们在**客户端**识别其他玩家或自己的一种方式。

概述

- scriptID仅对客户端是唯一的。
- scriptID将为我们提供一种修改玩家原生行为的方法。
    -   使用player实例优先于使用scriptID。
- 他们通常与当地人一起natives。
- 它们对于每个客户和每个玩家都是独一无二的。
    -   不要尝试与其他玩家共享scriptID。这是行不通的。

## 如何获取

可以通过这种方式获取玩家的scriptID。

```js
alt.Player.local.scriptID;
```

这相当于FiveM中的`local playerPed = PlayerPedId()`。

然而，对于单个玩家来说，这取决于你如何接收他们的玩家实例。

### Server Side

```js
import * as alt from 'alt-server';

alt.on('playerConnect', player => {
    // 向所有玩家发送带有player实例的` joined `事件
    alt.emitAllClients('joined', player);
});
```

### Client Side

```js
import * as alt from 'alt-client';
import * as native from 'natives';

alt.onServer('joined', otherPlayer => {
    // 检查otherPlayer是否为本地玩家
    if (otherPlayer === alt.Player.local) {
        // 让我们冻结自己。
        // 不要真的这么做。这就是大多数当地人使用scriptID的方式。
        alt.log(`You have frozen.`);
        native.freezeEntityPosition(alt.Player.local.scriptID, true);

        // 我们会在五秒钟内解冻自己。
        alt.setTimeout(() => {
            alt.log(`You are unfrozen.`);
            // 你可以使用player实例本身，这将包括一些内部检查
            native.freezeEntityPosition(alt.Player.local, false);
        }, 5000);
        return;
    }

    // 将scriptID打印到` F8 `控制台。
    alt.log(`Their scriptID is: ${otherPlayer.scriptID}`);
});
```
