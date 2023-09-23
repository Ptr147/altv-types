# 事件

事件以非常特定的方式工作，了解它们的通信方式非常重要。

服务器可以与任何客户端通信。
客户端只能与WebViews和服务器通信。

客户端**不能**与另一个客户端通信。

| 方法名      | 描述                                                                                                                                       |
| --------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------- |
| alt.emit              | 在服务器端或客户端发出一个事件，只能在发出方接收到。                                                         |
| alt.on                | 接收事件。Server只接收服务器事件。Client只接收Client事件。                                                     |
| alt.off               | 停止监听自定义或内置事件。                                                                                                   |
| alt.onServer          | 在客户端接收服务器发出的事件。通过`alt.emitClient`触发。                                                     |
| alt.offServer         | 停止监听来自服务器的自定义事件。                                                                                               |
| alt.emitClient        | 通过`alt.onServer`向特定的客户端或一组客户端发送事件。                                               |
| alt.emitClientRaw     | 向指定的客户端或客户端数组发送事件。它比`alt.onServer`接收的`emitAllClients`工作得更快。        |
| alt.emitAllClients    | 通过`alt.onServer`向所有接收到的客户端发送一个事件。                                                                            |
| alt.emitAllClientsRaw | 向所有客户端发送事件。它比`alt.onServer`接收的`emitAllClients`工作得更快。                                     |
| alt.onClient          | 在服务器端接收客户端发出的事件。通过`alt.emitServer`触发。                                                     |
| alt.offClient         | 停止监听来自客户端的自定义事件。                                                                                               |
| alt.emitServer        | 通过`alt.onClient`向服务器发送一个事件。                                                                              |
| alt.emitServerRaw     | (只能正常工作，服务器正在使用javascript)向服务器发送事件。它比`alt.onClient`接收的`emitServer`更快 |

## 从服务器到客户端

服务器只能通过`emitClient`或`emitClientRaw`向客户端发送数据，这需要一个玩家。

`emitClientRaw`和`emitAllClientsRaw`用于大数据，比`emitClient`或`emitAllClients`更快。


# [Client-side](#tab/tab1-0)
```js
alt.onServer('sayHello', () => {
    alt.log('Hello from server.');
});
```
# [Server-side](#tab/tab1-1)
```js
alt.on('playerConnect', player => {
    alt.emitClient(player, 'sayHello'); // 向特定的玩家发送事件

    // 向所有玩家发送事件
    alt.emitAllClients('sayHello');
});
```
***

## 从客户端到服务器

客户端只能通过`emitServer`或`emitServerRaw`将数据发送到服务器端。
`emitServerRaw`用于处理大数据，比`emitServer`更快，并且只有在服务器使用javascript时，`emitServerRaw`才有效。

服务器端`onServer`事件处理程序将自动接收发送事件的播放器作为第一个参数。

# [Client-side](#tab/tab2-0)
```js
alt.on('connectionComplete', () => {
    alt.emitServer('sayHello');
});
```
# [Server-side](#tab/tab2-1)
```js
alt.onClient('sayHello', player => {
    alt.log(`${player.name} is saying hello`);
});
```
***

## 从服务器资源到另一个服务器资源

服务器和客户端只能通过`on`和`emit`函数与自己通信。
它们也是跨资源发送和接收的。

# [Server-side](#tab/tab3-0)
```js
alt.emit('hello', 'this is a message');

alt.on('hello', msg => {
    alt.log(msg);
});
```
***

## 从客户端资源到另一个客户端资源


# [Client-side](#tab/tab4-0)
```js
alt.emit('hello', 'this is a message');

alt.on('hello', msg => {
    alt.log(msg);
});
```
***

## Client to WebView and back

**Note:** `resource` 在WebView的URL中引用您正在为之编写代码的资源。

# [Client-side code](#tab/tab5-0)
```js
const webview = new alt.WebView('http://resource/client/html/index.html');
webview.on('test2', handleFromWebview);

function handleFromWebview(msg) {
    alt.log(msg);
}

alt.setTimeout(() => {
    webview.emit('test', 'Hello from Client');
}, 500);
```
# [Client-side HTML](#tab/tab5-1)
```html
<html>
    <head>
        <title>Hello World</title>
    </head>
    <body>
        <p>Words</p>
        <script type="text/javascript">
            if ('alt' in window) {
                alt.on('test', msg => {
                    console.log(msg);
                    alt.emit('test2', 'hello from webview');
                });
            }
        </script>
    </body>
</html>
```
***

## 取消订阅事件

# [alt.off(Server-side/Client-side)](#tab/tab6-0)
```js
alt.on('customEventName', customEventFunction);
function customEventFunction() {
    alt.off('customEventName', customEventFunction);
}
```
# [alt.offClient(Server-side)](#tab/tab6-1)
```js
alt.onClient('customEventName', customEventFunction);
function customEventFunction() {
    alt.offClient('customEventName', customEventFunction);
}
```
# [alt.offServer(Client-side)](#tab/tab6-2)
```js
alt.onServer('customEventName', customEventFunction);
function customEventFunction() {
    alt.offServer('customEventName', customEventFunction);
}
```
***

## 原始事件

原始事件将值序列化为原始字节缓冲区表示，而不是将它们转换为中间值，因此原始事件可以比普通事件更快地序列化和发送大对象/数组，但缺点是这仅在双方都使用JavaScript时有效。

可以把它们看作是“原生”的JavaScript事件，它们可以发送几乎任何JS内置的数据结构，例如`Set`， `Map`， `Date`等，并且没有限制(例如，任何大小的BigInt)。

> [!CAUTION]
> `alt.emitServerRaw` 仅当服务器使用javascript时有效。

# [Server-side(emitClientRaw or emitAllClientsRaw)](#tab/tab7-0)
```js
alt.on('playerConnect', player => {
    const array = [
        { id: 0, name: "test1" },
        { id: 1, name: "test2" }
    ]
    alt.emitClientRaw(player, 'sayHello', array); // 向特定的玩家发送事件

    // 向所有玩家发送事件
    alt.emitAllClientsRaw('sayHello', array);
});
```

# [Client-side(emitClientRaw or emitAllClientsRaw listener)](#tab/tab7-1)
```js
alt.onServer('sayHello', (array) => {
    alt.log(array);
});
```

# [Client-side(emitServerRaw)](#tab/tab7-2)
```js
alt.on('connectionComplete', () => {
    const array = [
        { id: 0, name: "test1" },
        { id: 1, name: "test2" }
    ]
    alt.emitServerRaw(player, 'sayHello', array);
});
```

# [Server-side(emitServerRaw listener)](#tab/tab7-3)
```js
alt.onClient('sayHello', (player, array) => {
    alt.log(`${player.name} is sended ${array}`);
});
```
