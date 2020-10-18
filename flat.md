---
array: array
num: number
---

Flatten the `array`

```js
const flat = (array, num) => {
    let arr = [];
    function _flat(args, n) {
        if (Array.isArray(args) && n === 0) {
            return args;
        }
        n--;
        args.flatMap(v => {
            if (Array.isArray(v)) {
                if (Array.isArray(_flat(v, n))) {
                    arr.push(v);
                }
            } else {
                arr.push(v);
            }
        });
    }
    _flat(array, num + 1);
    return arr;
};
```
