# 介绍本地存储

## 概述

- 本地存储顾名思义 **本地/仅限客户端**.
- LocalStorage是一个持久的客户端缓存存储。
- 如果玩家删除了他的缓存文件夹，LocalStorage数据将会丢失!

**永远不要对敏感数据使用LocalStorage。内容以纯文本形式保存在磁盘上。**

| 函数名         | 描述                                                        |
| ---------------------- | ------------------------------------------------------------------- |
| alt.LocalStorage.get       | 从本地存储中的指定键获取值。      |
| alt.LocalStorage.set       | 将指定的键设置为本地存储中的指定值。 |
| alt.LocalStorage.delete    | 从本地存储中删除指定的键。                   |
| alt.LocalStorage.deleteAll | 从本地存储中删除所有密钥。                           |
| alt.LocalStorage.save      | 将更改保存到磁盘。                                    |

## 设置一个键 `.set(key: string, value: any)`

```js
import * as alt from "alt-client";

alt.LocalStorage.set("testKey", "test"); // 将指定的键设置为本地存储中的指定值。
alt.LocalStorage.save(); // 将更改保存到磁盘。
```

## Get a key `.get(key: string)`

```js
import * as alt from "alt-client";

const testKey = alt.LocalStorage.get('testKey'); // 从本地存储中的指定键获取值。
alt.log('testKey:', testKey)
```

## Delete a key `.delete(key: string)`

```js
import * as alt from "alt-client";

alt.LocalStorage.delete("testKey"); // 从本地存储中删除指定的键。
alt.LocalStorage.save(); // 将更改保存到磁盘。
```

## Delete all keys `.deleteAll()`

```js
import * as alt from "alt-client";

alt.LocalStorage.deleteAll(); // 从本地存储中删除所有密钥。
alt.LocalStorage.save(); // 将更改保存到磁盘。
```

## 示例用例：

- 存储暗/亮模式偏好
- 存储玩家自定义键绑定
- 存储其他小型玩家设置
- 上一次的步行方式
