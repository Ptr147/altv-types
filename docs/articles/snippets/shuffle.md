# 数组洗牌

如果你发现自己需要打乱数组。

这是一个简单的导出函数来打乱数组。

```js
export function shuffle(array) {
    var currentIndex = array.length;
    var temporaryValue, randomIndex;
    while (0 !== currentIndex) {
        // 选择剩余的元素...
        randomIndex = Math.floor(Math.random() * currentIndex);
        currentIndex -= 1;

        // 并将其与当前元素交换。
        temporaryValue = array[currentIndex];
        array[currentIndex] = array[randomIndex];
        array[randomIndex] = temporaryValue;
    }

    return array;
}
```

## Example Usage

```js
const myArray = [1, 2, 3, 4, 5];
const result = shuffle(myArray);
console.log(result);
```