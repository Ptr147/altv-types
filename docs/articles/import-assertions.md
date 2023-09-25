# 导入断言

这只在客户端可用。

## 一般信息

你可能知道，你可以使用`import`关键字导入一个模块(JS模块、JSON文件等)。但是，如果您导入的文件扩展名与文件内容不相同，则可能会出现安全问题。因此，为即将到来的ECMA2022标准提出了导入断言。因为alt:V的客户端依赖于谷歌的V8引擎，所以导入断言是可用的，因为V8支持它们。如果无法根据导入断言的类型解析文件，则文件导入将无法防止安全问题。目前，alt:V支持以下类型的导入断言:`source`， `base64`和`json`。

## 实现导入断言

你可以使用导入断言来检查文件是否可以安全导入。下面的例子展示了如何使用不同类型的导入断言。

# [main.js](#tab/tab1-0)
```js
import "console.log('check source import')" assert { type: "source" };
import "Y29uc29sZS5sb2coJ2NoZWNrIHNvdXJjZSBpbXBvcnQnKQ==" assert { type: "base64" };

import json from "/data.json" assert { type: "json" };
console.log(json.name); // 返回 "John Doe"
```
# [data.json](#tab/tab1-1)
```json
{
    "name": "John Doe",
    "age": "42"
}
```
***

## 动态导入断言

对于动态导入，也可以使用import断言。

# [main.js](#tab/tab2-0)
```js
(async () => {
  await import("Y29uc29sZS5sb2coJ2R5bmFtaWMgaW1wb3J0IHdpdGggaW1wb3J0IGFzc2VydGlvbiBvZiB0eXBlIHNvdXJjZScp", { assert: { type: "base64" } });
  await import("console.log('使用导入源类型断言动态导入')", { assert: { type: "source" } });
});

const json = await import("./data.json", { assert: { type: "json" } });
console.log(json.default.age); // 返回 "42"
```
# [data.json](#tab/tab2-1)
```json
{
    "name": "John Doe",
    "age": "42"
}
```
