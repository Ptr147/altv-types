# 原型

原型设计允许你扩展玩家、车辆等的默认类功能。

有一点很重要，所有的原型文件都应该在数据库初始化后立即加载。

假设你的原型具有数据库功能。

它们**不能**在多个资源中工作。只有一个资源。

这意味着原型的数据在资源之间是**不可转移**的。

### JavaScript Server Side Example

```js
alt.Player.prototype.emitMeta = function emitMeta(key, value) {
    if (typeof value === 'function') {
        throw new Error(`Value cannot be a function. emitMeta.prototype`);
    }

    this.setMeta(key, value);
    alt.emitClient(this, 'meta:Emit', key, value);
};

alt.Player.prototype.init = function init() {
    this.data = {};
};

alt.Player.prototype.syncCash = function syncCash() {
    this.emitMeta('cash', this.data.cash);
};

alt.Player.prototype.setCash = function setCash(value) {
    if (isNaN(value)) {
        throw new Error(`Value is not a number. setCash.prototype`);
    }

    if (!this.data) {
        this.data = {};
    }

    this.data.cash = value;
    this.syncCash();
};

alt.Player.prototype.subCash = function subCash(value) {
    if (isNaN(value)) {
        throw new Error(`Value is not a number. subCash.prototype`);
    }

    if (!this.data) {
        this.data = {};
    }

    const absValue = Math.abs(parseFloat(value)) * 1;
    if (this.data.cash < absValue) {
        return false;
    }

    this.data.cash -= absValue;
    this.data.cash = Number.parseFloat(this.data.cash).toFixed(2) * 1;
    this.syncCash();
    return true;
};

alt.Player.prototype.addCash = function addCash(value) {
    if (isNaN(value)) {
        throw new Error(`Value is not a number. addCash.prototype`);
    }

    if (!this.data) {
        this.data = {};
    }

    const absValue = Math.abs(parseFloat(value));
    if (this.data.cash + absValue > 92233720368547757) {
        absValue = 92233720368547757;
    }

    this.data.cash += absValue;
    this.data.cash = Number.parseFloat(this.data.cash).toFixed(2) * 1;
    this.syncCash();
    return true;
};
```

### Typescript Server Side Example

```ts
// 使用`alt-client`在客户端进行原型设计。
declare module 'alt-server' {
    export interface Player {
        cash?: number;
        bank?: number;

        // 必须声明函数接口。
        addToCash(value: number): boolean;
        addToBank(value: number): boolean;
    }
}

// 然后你可以定义原型函数。
alt.Player.prototype.addToCash = function addToCash(value: number) {
    if (!this.cash) {
        this.cash = value;
    } else {
        this.cash += value;
    }

    return true;
};

alt.Player.prototype.addToBank = function addToBank(value: number) {
    if (!this.bank) {
        this.bank = value;
    } else {
        this.bank += value;
    }

    return true;
};
```

## Example Usage

**Server Side**

```js
player.init();
player.emitMeta('test', true);
player.setCash(25);
player.subCash(5);
player.addCash(2);
```