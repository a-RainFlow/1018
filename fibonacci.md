Returns a Fibonacci array
```js
const fibonacci1 = (num) => {
    let arr = [];
    let len = num;
    const reNum = n => n === 1 ? 0 : n === 2 ? 1 : reNum(n - 1) + reNum(n - 2);
    for (let i = 0; i < len; i++) {
        arr.push(reNum(num--));
    }
    return arr.sort((a, b) => a - b);
};

```