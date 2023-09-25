# 加密 / 密码散列

这需要引入一个名为`sjcl`的库。

这将允许您执行PBKDF2加密。

你可以在包中安装它。通过执行`npm install sjcl`命令获取Json

**Server Side**

```js
import sjcl from 'sjcl';

/**
 * 用pbkdf2散列密码
 * @param {string} password
 * @returns {string} 密码hash.
 */
export function encryptPassword(password) {
    const saltBits = sjcl.random.randomWords(2, 0);
    const salt = sjcl.codec.base64.fromBits(saltBits);
    const key = sjcl.codec.base64.fromBits(sjcl.misc.pbkdf2(password, saltBits, 2000, 256));
    return `${key}$${salt}`;
}

/**
 * 验证密码是否与pbkdf2匹配。
 * @param {string} password
 * @param {string} storedPasswordHash
 * @returns {bool} 如果密码正确，返回true
 */
export function verifyPassword(password, storedPasswordHash) {
    const [_key, _salt] = storedPasswordHash.split('$');
    const saltBits = sjcl.codec.base64.toBits(_salt);
    const derivedKey = sjcl.misc.pbkdf2(password, saltBits, 2000, 256);
    const derivedBaseKey = sjcl.codec.base64.fromBits(derivedKey);

    if (_key != derivedBaseKey) {
        return false;
    }

    return true;
}

/**
 * 基于字符串生成哈希。
 * @param {string} data
 */
export function generateHash(data) {
    let hashBytes = sjcl.hash.sha256.hash(data + Math.random(0, 900000000));
    return sjcl.codec.hex.fromBits(hashBytes);
}

/**
 * 基于string生成持久哈希。
 * @param {string} data
 */
export function persistentHash(data) {
    let hashBytes = sjcl.hash.sha256.hash(data);
    return sjcl.codec.hex.fromBits(hashBytes);
}
```

## Example Usage

**Server Side**

```js
const hash = encryptPassword('test');
const isCorrectPassword = verifyPassword('test', hash);

if (isCorrectPassword) {
    console.log(`That was a correct password.`);
}

const randomDataHash = generateHash(`whatever ${hash} ${isCorrectPassword}`);
console.log(randomDataHash);

const persistent = persistentHash(`whatever`);
const persistent2 = persistentHash(`whatever`);

if (persistent === persistent2) {
    console.log(`they match!`);
}
```