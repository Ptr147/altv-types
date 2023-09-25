# 玩家属性

此页面将作为使用各种玩家属性的表示。

这些应该在服务器端使用。

## player.armour

用于**设置或获取**玩家当前的盔甲等级。

```js
const currentArmour = player.armour;
player.armour = 5;
player.armour += 10;
player.armour -= 10;
```

## player.authToken

用于在早期的认证实现中**获取**一个authToken。大多数用户不会使用它。

```js
alt.on('playerConnect', handleConnect);

function handleConnect(player) {
    if (!player.authToken) {
        player.kick(`No Auth Token Found`);
        return;
    }

    alt.log(`${player.name}'s authToken is ${authToken}`);
}
```

## player.currentWeapon

用于**设置或获取**玩家当前武器的哈希值。

```js
player.currentWeapon = alt.hash('weapon_pistol'); // 0x1B06D571

if (player.currentWeapon === 0x1b06d571) {
    console.log(`Player has a pistol equipped.`);
}
```

如果你想在服务器端用简单的英语而不是散列来处理所有武器检查。你应该尝试使用原型去设置并提供给玩家武器。

```js
alt.Player.prototype.setWeapon = function(weaponName) {
    this.currentWeapon = alt.hash(weaponName);
    this.currentWeaponName = weaponName;
};
```

## player.currentWeaponComponents

用于**获取**玩家当前装备的武器上的所有武器组件。所有武器组件都被哈希。

```js
const currentWeaponComponents = player.currentWeaponComponents;

for (let i = 0; i < currentWeaponComponents.length; i++) {
    const weaponComponentHash = currentWeaponComponents[i];
}
```

## player.currentWeaponTintIndex

用于**获取**当前装备武器的着色指数。

```js
const currentTint = player.currentWeaponTintIndex;
```

## player.dimension

用于**获取**或**设置**玩家的当前尺寸。尺寸基于INT32。这意味着一个维度有一个最小值和最大值。从`-2147483648`到`2147483648`。

Heron对dimensions的描述如下。

```
X只能看到X
-X可以看到0和-X
0看不到-X和X
```

默认维度是**0**。

```js
const currentDimension = player.dimension;
player.dimension = 1;

// 如果你需要每个玩家加入一个唯一的实例。这是一种不错的处理方式。
// 只要记得在对唯一维度执行任何操作后将其设置为零即可。
player.dimension = player.id;
```

## player.entityAimOffset

用于**获取**玩家当前的瞄准偏移量。目前还没有这个特定功能的用例。

在撰写本文时，我们甚至不确定它是否正常运行。

## player.entityAimingAt

用来**获取**玩家当前的目标实体。目前的看法是，这个特定的属性不起作用。

在撰写本文时，我们甚至不确定它是否正常运行。

## player.flashlightActive

用于**获取**玩家的手电筒活动?

在撰写本文时，我们甚至不确定它是否正常运行。

## player.health

用于**获取或设置**玩家当前的生命值。

《GTA:V》中的每个模型都有自己的健康范围。

```js
const currentHealth = player.health;
player.health += 10;

// Kill the player.
player.health = 0;
```

## player.hwidExHash

这是获得属于玩家的唯一硬件哈希的两种方法之一。

这是一个禁止玩家的好方法;尤其是他们需要一套全新的硬件才能通过你的禁令。

目前没有人知道如何确定硬件信息。

```js
const currentHwidEx = player.hwidExHash;
```

## player.hwidHash

这是获得属于玩家的唯一硬件哈希的两种方法之一。

这是一个禁止玩家的好方法;尤其是他们需要一套全新的硬件才能通过你的禁令。

目前没有人知道如何确定硬件信息。


```js
const currentHwid = player.hwidHash;
```

## player.id

用于**获取**玩家的当前id。这些严格来说对于玩家的加入顺序是唯一的。如果玩家断开连接，它的id将被连接的新玩家或稍后创建的车辆重复使用。

ID的值可以使用到`65535`。

```js
alt.on('playerConnect', handleConnect);

function handleConnect(player) {
    alt.log(`${player.name} has joined with the ID of ${player.id}`);
}
```

## player.ip

在大多数情况下，这是一个从IPv4转换到IPv6的IP。

本地主机IP的一个示例:`::ffff:127.0.0.1`

```js
alt.on('playerConnect', handleConnect);

function handleConnect(player) {
    if (player.ip === `::ffff:127.0.0.1`) {
        alt.log(`${player.name} has joined from local host.`);
    }
}
```

## player.maxArmour

用于**设置或获取**玩家当前的最大护甲。最大值可以设置为`8191`。

设置完成后，可以将玩家的当前盔甲设置为最大盔甲值。

```js
const currentMaxArmour = player.maxArmour;
if (currentMaxArmour <= 100) {
    player.maxArmour = 8191;
    player.armour = 8191;
}

alt.log(`${player.name} is a chunker.`);
```

## player.maxHealth

用于**设置或获取**玩家当前的最大生命值。最大值可以设置为`8191`。

在此之后，你可以将玩家的当前生命值设置为最大生命值。

```js
const currentMaxHealth = player.maxHealth;
if (currentMaxHealth <= 100) {
    player.maxHealth = 8191;
    player.health = 8191;
}

alt.log(`${player.name} is a healthy chunker.`);
```

## player.model

用于设置**设置或获取**玩家的当前模型。

你可以在这里传递一个字符串值。

没有自动设置模型，应该在玩家连接或认证时设置。

```js
alt.on('playerConnect', handleConnect);

function handleConnect(player) {
    player.model = 'mp_m_freemode_01';
}
```

## player.name

用于**获取**玩家的当前名字。这是由玩家在他们的选项中设定的。

```js
alt.on('playerConnect', handleConnect);

function handleConnect(player) {
    if (player.name.toLowerCase() === 'stuyk') {
        player.kick(`Stupid name.`);
    }
}
```

## player.netOwner

用于**获取**负责将实体同步到服务器的当前网络所有者。继承自entity。

对于一般的开发人员来说，这并不是一个有用的属性。

```js
const currentOwner = player.netOwner;

if (!currentOwner) {
    console.log(`Looks like nobody owns this entity.`);
}
```

## player.ping

用于**获取**玩家当前的ping。

下面是一个片段，使玩家ping可由所有玩家查看。

```js
alt.setInterval(handlePingUpdate, 5000);

function handlePingUpdate() {
    const currentPlayers = [...alt.Player.all];

    for (let i = 0; i < currentPlayers.length; i++) {
        const player = currentPlayers[i];
        if (!player || !player.valid) {
            continue;
        }

        player.setSyncedMeta('ping', player.ping);
    }
}
```

## player.pos

用于**获取**或**设置**玩家的当前位置。

当获得玩家的当前位置时，你应该使用扩展操作符使其可修改。

```js
const currentPos = { ...player.pos };
currentPos.x += 1;

// Teleport them under the map.
player.pos = {
    x: 0,
    y: 0,
    z: 0
};

// Create a Vector3 with the alt function.
player.pos = new alt.Vector3(0, 0, 0);
```

## player.rot

Used to **get or set** the player's current rotation.

```js
// Get and log current player rotation.
alt.log(player.rot);

// Set player rotation to zero.
player.rot = new alt.Vector3(0, 0, 0);
```

## player.seat

用于**获得**玩家当前所在的位置。

```js
if (player.vehicle && player.seat === 1) {
    alt.log(`Player is in the driver's seat.`); // 驾驶位
}

if (player.vehicle && player.seat === 2) {
    alt.log(`Player is riding shotgun.`); // 副驾驶
}

if (player.vehicle && player.seat === 3) {
    alt.log(`Player is behind the driver's seat.`); // 左后方
}

if (player.vehicle && player.seat === 4) {
    alt.log(`Player is behind the player riding shotgun.`); // 右后方
}
```

## player.socialID

用于**获取**玩家当前social club身份的非安全版本，该身份与他们的Rockstar账户相关联。

请记住，这个值**不安全**，不要将其用于身份验证。**它可以被伪造**。

```js
const social = player.socialID;
```

## player.valid

用于**获取**玩家实体是否仍然有效。在使用超时、间隔和处理断开连接事件时，这很有用。

无效的玩家意味着它与服务器断开了连接。

```js
alt.on('playerDisconnect', handleDisconnect);

function handleDisconnect(player, reason) {
    if (!player || !player.valid) {
        console.log(`Looks like this player is already invalid. Can't save anything.`);
        return;
    }

    console.log(`${player.name} has disconnected.`);
}
```

## player.vehicle

用于**获取**玩家所在的当前车辆。如果玩家**不在**车辆中，则返回**null**。

```js
const { vehicle } = player
if (vehicle) {
    console.log(`Player is in a vehicle. Let's change its color to red!`);
    vehicle.customPrimaryColor = new alt.RGBA(255, 0, 0);
}

if (!player.vehicle) {
    console.log(`Player is not in a vehicle.`);
}
```
