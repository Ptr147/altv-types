# 工人

## 什么是alt:V工人

alt:V中的workers实现与NodeJS非常相似，因为它允许你在另一个线程中并行运行JavaScript代码。Workers（线程）对于执行CPU密集型操作非常有用，但对于I/O密集型工作并没有太大帮助。V8引擎的异步I/O操作比Workers更高效。

### 重要提示

* 在Workers中无法访问Native函数。
* 创建一个worker线程并不会让你的代码运行更快，它只是将任务分配到另一个线程中

## 用法

> [!CAUTION]
> 为了能够使用alt:V Worker API，你必须通过 ``import * as alt from alt-worker`` 进行导入。你可以在[这里](https://docs.altv.mp/js/api/alt-client.Worker.html)找到整个Worker类的API。
### worker中可用的类

只有以下是线程安全的客户端alt API类可在Worker中使用：

* alt.File
* alt.RGBA
* alt.Vector2
* alt.Vector3

### worker中可用的函数

在alt:V环境中，诸如 ``setInterval`` 或 ``console.log`` 等基本功能是不存在的。这就是为什么我们会为它们暴露我们自己的函数，但为了更方便，如果你使用 ``setInterval`` 或 ``console.log``，它将调用相应的alt方法。

| 方法名     | 描述                                                                                                     |
| ----------------- | --------------------------------------------------------------------------------------------------------------- |
| alt.emit                 | 向客户端发送一个事件 side.                                                                        |
| alt.on                   | 接收从客户端发出的事件 side.                                                          |
| alt.once                 | 接收从客户端发出的事件。一旦触发，处理程序将被销毁。                 |
| alt.log                  | 使用alt:V日志记录器将指定的参数记录到控制台（会打印到客户端日志文件中）。 |
| alt.logWarning           | 使用alt:V日志记录器将指定的参数记录为控制台警告。                   |
| alt.logError             | 使用alt:V日志记录器将指定的参数记录为控制台错误。                              |
| alt.nextTick             | 安排在下一帧/下一tick上执行处理程序。                                                  |
| alt.setInterval          | 安排以指定间隔执行处理程序。                                                   |
| alt.setTimeout           | 安排处理程序在间隔到期后执行一次。                                   |
| alt.clearNextTick        | 清除使用nextTick函数设置的定时器。                                                           |
| alt.clearInterval        | 清除使用setInterval函数设置的定时器。                                                        |
| alt.clearTimeout         | 清除使用setTimeout函数设置的定时器。                                                         |
| alt.hash                 | 使用Jenkins one-at-a-time算法创建哈希。                                 |
| alt.getSharedArrayBuffer | 检索具有给定ID的共享数组缓冲实例。                                            |

### worker中可用的属性

| 属性名  | 描述                               |
| -------------- | ----------------------------------------- |
| alt.version    | 表示当前版本。          |
| alt.branch     | 表示当前分支。            |
| alt.sdkVersion | 表示当前SDK版本。      |
| alt.debug      | 返回资源是否处于调试模式。 |

### 如何创建worker

# [相对路径](#tab/tab1-0)
```js
import * as alt from 'alt-client';

const worker = new alt.Worker('./worker.js');
worker.start();
```
# [绝对路径](#tab/tab1-1)
```js
import * as alt from 'alt-client';

// 从资源根目录开始的路径
const worker = new alt.Worker('/client/worker.js');
worker.start();
```
***

## 内置worker事件

### 了解worker加载完成时机

```js
worker.on('load', () => {
    console.log('Worker loaded');
});
```

### 错误处理

```js
worker.on('error', (error) => {
    console.log(error);
});
```

### 从客户端向Worker进行通信

# [client](#tab/tab2-0)
```js
import * as alt from 'alt-client';

const worker = new alt.Worker('./worker.js');
worker.start();

worker.on('load', () => worker.emit('eventName', true, 0, [''], { p1: new Map() }));
```
# [worker](#tab/tab2-1)
```js
import * as alt from 'alt-worker';

alt.on('eventName', (...args) => {
    console.log(args);
});
```
***

### 从Worker向客户端进行通信

# [worker](#tab/tab3-0)
```js
import * as alt from 'alt-worker';

alt.emit('eventName', true, 0, [''], { p1: new Map() });
```
# [client](#tab/tab3-1)
```js
import * as alt from 'alt-client';

const worker = new alt.Worker('./worker.js');

worker.on('eventName', (...args) => {
    console.log(args);
});
```
***

### 使用共享数组缓冲区

[ArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/ArrayBuffer) 是用于表示通用、固定长度的原始二进制数据缓冲区的对象。
它是字节的数组，你不能直接操作其内容，但可以创建 [TypedArray](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/TypedArray) 对象或 [DataView](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/DataView) 来读取和写入缓冲区内容。

[SharedArrayBuffer](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/SharedArrayBuffer) 与数组缓冲区非常相似，不同之处在于它们可以用于在共享内存上创建视图。

> [!WARNING]
> 为了正确使用SharedArrayBuffers，你应该学习如何在alt:V环境之外使用它们。以下示例利用[Atomics](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Atomics) API来确保SharedArrayBuffer的[线程安全](https://en.wikipedia.org/wiki/Thread_safety)。

**示例用例**

# [client](#tab/tab4-0)
```js
import * as alt from 'alt-client';

const worker = new alt.Worker('./worker.js');
worker.start();

const buffer = createBuffer();
const sharedArray = new Int32Array(buffer);

worker.on('changedValue', () => {
     for(let i = 0; i < 10; i++){
       // 在"主线程"上输出更改后的值
       const value = Atomics.load(sharedArray,i);
       console.log(`[Main thread]: Value of index ${i} is ${value}`);
    }
});

function createBuffer() {
    // 根据数组类型计算缓冲区大小
    const length = 10;
    const size = Int32Array.BYTES_PER_ELEMENT * length;
    const sharedBuffer = new SharedArrayBuffer(size);
    const sharedArray = new Int32Array(sharedBuffer);
      
    // 向数组应用值
    for(let i = 0; i< 10; i++){
        sharedArray[i] = i*20;
    }
  
    const bufferIndex = alt.Worker.addSharedArrayBuffer(sharedBuffer);
    worker.emit("addedMyBuffer", bufferIndex);
    alt.log('emitted buffer to worker')

    return sharedBuffer;
}
```
# [worker](#tab/tab4-1)
```js
alt.on('addedMyBuffer', (index) => {
    //通过索引获取缓冲区并生成一个Int32Array
    const buffer = alt.getSharedArrayBuffer(index);
    const sharedArray = new Int32Array(buffer);

    for(let i = 0; i < 10; i++) {
        //读取值
        const arrayValue = Atomics.load(sharedArray, i);
        console.log(`[Worker] Value from main thread for [${i}] is ${arrayValue}`);

        // Atomics.exchange 返回索引 i 上的旧值。
        const oldValue = Atomics.exchange(sharedArray, i, 5)
        console.log(`[Worker] Value change to 5 on [${i}]`);

        // 读取新值
        const changedValue = Atomics.load(sharedArray, i);
        console.log(`[Worker] New value on ${i} is ${changedValue} (old value: ${oldValue})`);

        alt.emit('changedValue');
    }
});
```
