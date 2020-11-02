doubleArr is converted to an object
```js
let doubleArr = [["name", "wee"], ["ewr", 123]];
let obj = doubleArr.reduce((acc, [key, value]) => ({...acc, [key]: value}), {});
```
