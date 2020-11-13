```js
const myGeometricProgression = function (end, start = 1, step = 2) {
    let arr = [];
    for (let i = 0; start * step ** i <= end; i++) {
        arr.push(start * step ** i);
    }
    if (arguments.length === 2) {
        return arr.filter(v => v < end);
    }
    return arr;
};

const geometricProgression = (end, start = 1, step = 2) =>
    Array.from({
        length: Math.floor(Math.log(end / start) / Math.log(step)) + 1,
    }).map((_, i) => start * step ** i);

console.log(myGeometricProgression(256));
console.log(myGeometricProgression(256, 3));
console.log(myGeometricProgression(256, 2, 4));
```
