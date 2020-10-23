Filter out values that are not unique

```js
const myFilterNonUnique = arr => {
    let a = [];
    let k = 0;
    for (let i = 0; i < arr.length - 1; i++) {
        for (let j = i + 1; j < arr.length; j++) {
            if (arr[i] === arr[j]) {
                a[k] = arr[i];
                k++;
                break;
            }
        }
    }
    return arr.filter(v => !a.some(k => v === k));
};
const filterNonUnique = arr => arr.filter(i => arr.indexOf(i) === arr.lastIndexOf(i));
console.log(filterNonUnique([1, 2, 2, 3, 6, 4, 6, 4, 5]));
```