debounce: 简单的函数防抖 throttle: 简单的函数节流
```js
function debounceFn(delay) {
        let flag = true;
        let time = null;

        function fn() {
            if (time) {
                clearTimeout(time);
            }
            if (flag) {
                time = setTimeout(function () {
                    console.log(2);
                }, delay)
            }
        }

        return fn;
    }

    function throttleFn(delay) {
        let flag = true;

        function fn() {
            if (flag) {
                flag = false;
                setTimeout(function () {
                    flag = true;
                    console.log(1);
                }, delay)
            }
        }
        return fn;
    }
    ```
