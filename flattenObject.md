flatten Object
```js
const myFlattenObject = targetObj => {
    let tempObj = {};
    let str = "";
    let flag = false;
    let temp = null;

    function fn(obj) {
        Object.keys(obj).forEach(v => {
            if (typeof obj[v] === "object") {
                flag = true;
                str = fn(obj[v]);
                str += v;
            } else if (flag) {
                flag = false;
                str += v;
                temp = obj[v];
            } else {
                tempObj[v] = obj[v];
            }
        });
        return str;
    }

    fn(targetObj);
    tempObj[[...str].reverse().join(".")] = temp;
    return tempObj;
};

const flattenObject = (obj, prefix = '') =>
    Object.keys(obj).reduce((acc, k) => {
        const pre = prefix.length ? `${prefix}.` : '';
        if (typeof obj[k] === 'object' && obj[k] !== null && Object.keys(obj[k]).length > 0) {
            Object.assign(acc, flattenObject(obj[k], pre + k));
        } else {
            acc[pre + k] = obj[k];
        }
        return acc;
    }, {});
```
