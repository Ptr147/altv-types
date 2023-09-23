# 玩家服务器函数

这个页面将作为使用各种玩家函数的一个表示。

这些应该在服务器端中使用。

## addWeaponComponent

用于根据武器的哈希值为其添加武器组件。

```js
// weaponHash, componentHash
player.addWeaponComponent(0xDFE37640, 0x5B3E7DB6)
```

## deleteMeta

用于删除跨资源可用的**服务器端**元数据。

```js
// 设置玩家的元数据。
player.setMeta('cash', 500); // key, value

// 检查元数据是否存在
if (player.hasMeta('cash')) { // key
    // 获取玩家身上的元数据
	const currentCash = player.getMeta('cash'); // key

	// 删除元数据
    player.deleteMeta('cash'); // <--- Usage
}
```

## deleteStreamSyncedMeta

用于删除存储在**服务器端**和**客户端**可访问的玩家实体上的数据。

这种类型的同步元仅可由该玩家的流范围内的玩家访问。

```js
// 设置玩家的元数据。
player.setStreamSyncedMeta('ping', player.ip); // key, value

// 检查元数据是否存在
if (player.hasStreamSyncedMeta('ping')) { // key
    // 获取玩家身上的元数据
	const currentCash = player.getStreamSyncedMeta('ping'); // key
    
	// 删除元数据
    player.deleteStreamSyncedMeta('ping'); // <--- Usage
}
```

## deleteSyncedMeta

用于删除存储在**服务器端**和**客户端**可访问的玩家实体上的数据。

这种类型的同步元数据在流范围内和**外部**都可以访问。

```js
// 设置玩家的元数据。
player.setSyncedMeta('ping', player.ip); // key, value

// 检查元数据是否存在
if (player.hasSyncedMeta('ping')) { // key
    // 获取玩家身上的元数据
	const currentCash = player.getSyncedMeta('ping'); // key
    
	// 删除元数据
    player.deleteSyncedMeta('ping'); // <--- Usage
}
```

## destroy（销毁）

你永远不应该使用它。它不会做任何事情，可能会让你的服务器崩溃。

## getMeta（获取元数据）

用于获取跨资源可用的**服务器端**元数据。

```js
// 设置玩家的元数据。
player.setMeta('cash', 500); // key, value

// 检查元数据是否存在
if (player.hasMeta('cash')) { // key
    // 获取玩家身上的元数据
	const currentCash = player.getMeta('cash'); // <--- Usage

	// 删除元数据
    player.deleteMeta('cash'); // key
}
```

## getStreamSyncedMeta

用于获取存储在玩家实体上的数据，可以在**服务器端**和**客户端**访问。

这种类型的同步元仅可由该玩家**流范围内**的玩家访问。

```js
// 设置玩家的元数据。
player.setStreamSyncedMeta('ping', player.ip); // key, value

// 检查元数据是否存在
if (player.hasStreamSyncedMeta('ping')) { // key
    // 获取玩家身上的元数据
	const currentCash = player.getStreamSyncedMeta('ping'); // <--- Usage
    
	// 删除元数据
    player.deleteStreamSyncedMeta('ping'); // key
}
```

## getSyncedMeta

用于删除存储在**服务器端**和**客户端**可访问的玩家实体上的数据。

这种类型的同步元数据在流范围**内部**和**外部**都可以访问。

```js
// 设置玩家的元数据。
player.setSyncedMeta('ping', player.ip); // key, value

// 检查元数据是否存在
if (player.hasSyncedMeta('ping')) { // key
    // 获取玩家身上的元数据
	const currentCash = player.getSyncedMeta('ping'); // <--- Usage
    
	// 删除元数据
    player.deleteSyncedMeta('ping'); // key
}
```

## giveWeapon

用于根据哈希或适当的名称给玩家一种武器。

```js
// weaponHash, ammoCount, equipThisWeaponNow?
player.giveWeapon(1141786504, 1, true); // Get a golf club.
```

## hasMeta

用于检查**服务端** 元数据是否可用。这可以跨资源工作。

```js
// 设置玩家的元数据。
player.setMeta('cash', 500); // key, value

// 检查元数据是否存在
if (player.hasMeta('cash')) { // <---- Usage
    // 获取玩家身上的元数据
	const currentCash = player.getMeta('cash'); // key

	// 删除元数据
    player.deleteMeta('cash'); // key
}
```

## hasStreamSyncedMeta

用于检查存储在玩家实体上的数据是否可用。这可以在**服务器端**和**客户端**访问。

这种类型的同步元仅可由该玩家的流范围内的玩家访问。

```js
// 设置玩家的元数据。
player.setStreamSyncedMeta('ping', player.ip); // key, value

// 检查元数据是否存在
if (player.hasStreamSyncedMeta('ping')) { // <--- Usage
    // 获取玩家身上的元数据
	const currentCash = player.getStreamSyncedMeta('ping'); // key
    
	// 删除元数据
    player.deleteStreamSyncedMeta('ping'); // key
}
```

## hasSyncedMeta

用于检查存储在玩家实体上的数据是否可用。这可以在**服务器端**和**客户端**访问。

这种类型的同步元数据在流范围**内部**和外部都可以访问。

```js
// 设置玩家的元数据。
player.setSyncedMeta('ping', player.ip); // key, value

// 检查元数据是否存在
if (player.hasSyncedMeta('ping')) {  // <--- Usage
    // 获取玩家身上的元数据
	const currentCash = player.getSyncedMeta('ping'); // key
    
	// 删除元数据
    player.deleteSyncedMeta('ping'); // key
}
```

## kick

用来从服务器踢出玩家。提供给kick函数的**reason**目前不会显示给玩家。

你可能想创建一个解决方案，在5秒后断开与用户的连接。

```js
alt.on('playerConnect', handleConnect);

// 监听玩家连接。
function handleConnect(player) {
	player.kick('Bye'); // 立即踢出他们。
    
	// Kick them eventually.
    alt.setTimeout(() => {
        console.log(`${player.name} will be kicked in 5 seconds.`);
    }, 5000); // 5 seconds.
}
```

## removeAllWeapons

用于移除玩家身上的所有武器。

```js
player.removeAllWeapons(); // 除此之外就没什么了。
```

## removeWeapon

用于根据武器哈希值移除武器。

```js
player.removeWeapon(1141786504); // Remove a golf club.
```

## removeWeaponComponent

用于从玩家的武器中移除武器组件。

```js
// weaponHash, componentHash
player.removeWeaponComponent(0xDFE37640, 0x5B3E7DB6)
```

## resetNetOwner

恢复netOwner的默认功能。一般的开发人员可能不会使用它。

## setDateTime

用于设置玩家在游戏中的当前时间。重要的是要理解，无论玩家当前的时间尺度是什么，时间仍然会继续。

如果您希望它停留在某个时间，建议冻结时间。

这可以在**客户端**使用原生的`native.pauseclock(true);`来实现。

```js
const currentServerTime = new Date(Date.now());
// day, month, year, hour, minute, second
player.setDateTime(date.getDay(), date.getMonth(), date.getFullYear(), date.getHour(), date.getMinute(), date.getSecond())
```

## setMeta

Used to set **server-side** meta on the player. This works cross-resource.

```js
// 设置玩家的元数据。
player.setMeta('cash', 500); // <--- Usage (key, value)

// 检查元数据是否存在
if (player.hasMeta('cash')) { // key
    // 获取玩家身上的元数据
	const currentCash = player.getMeta('cash'); // key

	// 删除元数据
    player.deleteMeta('cash'); // key
}
```

## setNetOwner

用于更改此实体的网络所有者。一般的开发人员可能不会使用它。

## setStreamSyncedMeta

用于设置存储在玩家实体上的数据。这可以在**服务器端**和**客户端**访问。

这种类型的同步元仅可由该玩家的流范围内的玩家访问。

```js
// 设置玩家的元数据。
player.setStreamSyncedMeta('ping', player.ip); // key, value

// 检查元数据是否存在
if (player.hasStreamSyncedMeta('ping')) { // <--- Usage
    // 获取玩家身上的元数据
	const currentCash = player.getStreamSyncedMeta('ping'); // key
    
	// 删除元数据
    player.deleteStreamSyncedMeta('ping'); // key
}
```

## setSyncedMeta

用于设置存储在**服务器端**和**客户端**可访问的玩家实体上的数据。

这种类型的同步元数据在流范围**内部**和**外部**都可以访问。

```js
// 设置玩家的元数据。
player.setSyncedMeta('ping', player.ip); // key, value

// 检查元数据是否存在
if (player.hasSyncedMeta('ping')) {  // <--- Usage
    // 获取玩家身上的元数据
	const currentCash = player.getSyncedMeta('ping'); // key
    
	// 删除元数据
    player.deleteSyncedMeta('ping'); // key
}
```

## setWeaponTintIndex

用于根据武器的哈希值设置武器的着色索引。

```js
// weaponHash, componentHash
player.setWeaponTintIndex(0xDFE37640, 1)
```

## spawn

用于在玩家死后根据Vector3位置重生。最后一个参数用于延迟刷出。

```js
// x, y, z, delay
player.spawn(0, 0, 0, 0); // 在地图下0,0,0处生成玩家
```


