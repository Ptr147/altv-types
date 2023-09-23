# 玩家连接前

## 概述

<a href="https://docs.altv.mp/js/api/alt-server.IServerEvent.html#_altmp_altv_types_alt_server_IServerEvent_beforePlayerConnect" target="_blank"> 你可以在这里找到关于类型的信息（点击跳转）。 </a>

这个事件允许服务器在实际连接之前踢出用户，例如，如果玩家被封禁。

## 示例

```js
import alt from "alt-server";

alt.on('beforePlayerConnect', (connectionInfo) => {
  alt.log(connectionInfo.branch); // 打印出当前分支。
  alt.log(connectionInfo.authToken); // 打印authToken.
  alt.log(connectionInfo.build); // 打印出当前构建编号。
  alt.log(connectionInfo.cdnUrl); // 打印CDN的URL.
  alt.log(connectionInfo.hwidExHash); // 打印出客户端硬件ID（HWID）的ExHash。
  alt.log(connectionInfo.hwidHash); // 打印出客户端硬件ID（HWID）的ExHash。
  alt.log(connectionInfo.isDebug); // 打印出客户端是否处于调试模式。
  alt.log(connectionInfo.passwordHash); // 打印出客户端是否处于调试模式。
  alt.log(connectionInfo.socialID); // 打印出客户端的social ID。
  
  if(connectionInfo.passwordHash !== alt.hashServerPassword('myPassword')) {
    return false; 
  }
});
```
