# vue

虚拟DOM: 提高性能, 减少重绘

MVVM: 一种模式

​	该模式通过ViewModel把Model(模型层, Model中的数据)传递给View(视图层, 页面), 也可以从View传给Model, 这样的交互是双向的.

​	只要Model发生改变View就发生改变这个过程称为数据绑定.

​	View发生改变, 数据通过ViewModel传递该Model, Model发生改变也通过ViewModel把数据传回该View这个过程称为双向数据绑定

插件: 库

指令: vue自定义的标签属性(v-model 双向数据绑定), 实现页面动态

插值语法: {{xxx}} 动态显示数据

全局配置: Vue.config.productionTip = false;  // 关闭没有使用生产环境模式的提示

```html
<div id="test">
    <input type="text" v-model="message">{{message}}  // input类型是checkbox时, 如果有v-model那么他本身的checked属性就没有效果
    <a v-bind:href="url">aa</a>  // 动态绑定数据(这样url就不是字符串, 而是真实的数据了)
    // 简写:<a :href="url">aa</a>
	<div class="box" v-on:click="test1">事件监听测试</div>  //需要传递事件对象参数要加$, 事件修饰符: .stop阻止冒泡, .prevent阻止默认行为, .....
    // 简写:<div class="box" @click="test1">事件监听测试</div>
    <div v-html="text"></div>  // 可识别标签
    <div v-text="text1"></div>  // 不可识别标签, 只看成文本
</div>

<!--<script src="https://cdn.jsdelivr.net/npm/vue"></script>-->
<script src="./js/vue.js"></script>
<script>
    const vm = new Vue({
        el: "#test",
        data: {
            message: "hello",
            url: "http://www.baidu.com",
            text: "<p>aa</p>",
            text1: "<p>aa</p>"
        },
        methods:{
            test1(event){
                console.log(event.target.innerHTML);
            }
        }
    })
    
    vm.$mount("#test");  // 在new实例时不写el, 另一种选择DOM元素作为vue实例的挂载目标
</script>
```

----

## 插值语法使用API

```html
<div class="box">
    <p>{{text.toUpperCase()}}</p>
</div>

<script src="./js/vue.js"></script>
<script>
    let vm = new Vue({
        el: ".box",
        data: {
            text: "aaaa"
        }
    })
</script>
```

----

## 计算属性

通过计算属性实现数据绑定, 计算属性有一个缓存机制, 绑定过数据如果需要再次使用会从缓存中拿去, 也就不需要再次调用get方法, 缓存是一个键值对对象

```html
<div class="box">
    <input type="text" v-model="text">
    <input type="text" v-model="text1">
    <p>{{text2}}</p>
</div>

<script src="./js/vue.js"></script>
<script>
    let vm = new Vue({
        el: ".box",
        data: {
            text: "",
            text1: ""
        },
        computed:{
            text2:{  // 如果第一次没有获取数据, 这里的get方法也会默认执行一次
                get(){  // 实现单向数据绑定.  view  -->  ViewModel  -->  model
                    return this.text + this.text1;
                }
                // 添加一个set()可实现双向数据绑定. view  -->  ViewModel  -->  model  -->  ViewModel  -->  view
            }
        }
    })
</script>
```

----

## 监视语法

监控data中的属性

```html
<div class="box">
    <input type="text" v-model="text">
    <input type="text" v-model="text1">
    <input type="text" v-model="text2">
</div>

<script src="./js/vue.js"></script>
<script>
    let vm = new Vue({
        el: ".box",
        data: {
            text: "",
            text1: ""
        },
        watch: {
            text(val) {  // 监控的函数有2个参数, 一个表示新值, 一个表示旧值
                this.text2 = val + this.text1;
            },
            // 另一种写法
            text: {
                handler(newVal, oldVal){
                    console.log(newVal, oldVal);	    			
                },
                immediate: true // 第一次监控的时候触发一次监控函数
                // deep: true  监控对象类型, 由于对象类型是引用地址, 如果对象中的元素发生改变监控函数不会触发, 需要该属性来深度监控
	    }
        }
    })
</script>
```

----

## class绑定 style绑定

```html
<!-- 动态的绑定class和style -->
<div class="box">
    <p class="text" :class="A" :style="{fontSize: mySize+'px'}">1111</p>
    // :class="{box:true}"  如果是对象属性值为true 该属性就会添加到class中
    <button @click="btn">更新</button>
</div>

<script src="./js/vue.js"></script>
<script>
    let vm = new Vue({
        el: ".box",
        data: {
            A: "",
            mySize: 20
        },
        methods:{
            btn(){
                this.A = 'fontColorA';  // fontColorA 样式名
                this.mySize = 40;
            }
        }
    })
</script>
```

----

## 条件渲染

```html
<div class="test">
    <p v-if="ok">1</p>
    <p v-else>2</p>

    <p v-show="ok">3</p>
    <p v-show="!ok">4</p>
    <button @click="toggle">切换</button>
</div>

<script src="./js/vue.js"></script>
<script>
    let vm = new Vue({
        el: ".test",
        data: {
            ok: true
        },
        methods:{
            toggle(){
                this.ok = !this.ok;
            }
        }
    });
</script>
```

----

## v-for循环创建li

```html
<div class="box">
    <ul >
        <li v-for="item in arr">{{item}}}</li>
    </ul>
</div>
<script src="../js/vue.js"></script>
<script>
    let vm = new Vue({
        el: ".box",
        data:{
            arr: [{name:"wer", age:23},{name:"ffd", age:6},{name:"zc", age:32}]
        }
    })
</script>
```

----

## input的type是单选和多选

```html
<div class="box">
    <input type="checkbox" value="ipt" v-model="ipt">ipt
	<!-- 多个单选的v-model设置成一个数据, 可实现单选效果, 就不用给每个单选添加同一name属性 -->
</div>

<script src="../js/vue.js"></script>
<script>
    let vm = new Vue({
        el: ".box",
        data:{
            ipt: ""
            // ipt: []  如果是数组, 数组中存的是input的value的值
        }
    });
</script>
```

## 过滤器

过滤器就是把数据按照指定的方法进行处理后过滤出来, 需要把值进行return才有值

```html
<div class="test">
    <p>{{"agc"|toUpper}}</p>
</div>

<script src="./js/vue.js"></script>
<script>
    let vm = new Vue({
        el: ".test",
        data: {},
        filters: {
            toUpper(target) {
                return target.toUpperCase();
            }
        }
    });
</script>
```

----

## 自定义指令

```html
<div class="box">
    <input type="text" v-test>
</div>

<script src="./js/vue.js"></script>
<script>
    let vm = new Vue({
        el: ".box",
        data: {
        },
        directives:{
            test(el){
                console.log(el);  // el 当前input元素
            }
        }
    })
</script>
```

----

## 生命周期

```html
<div class="box">
    <p>{{test}}</p>
</div>

<template id="temp">  // 提供一个专门为template属性操作的标签
    <p>123456</p>
</template>

<script src="./js/vue.js"></script>
<script>
    let vm = new Vue({
        el: ".box",
        // template: "<p>123</p>",  // 可以直接替换整个.box, 但是只能用一个父级元素替换
        template: "#temp",  // 生命周期来到这里: 检测是否有模板, 有就替换
        data: {
            test: "aaa"
        },
        beforeCreate(){  // 生命周期来到这里: 数据初始化之前, 无法获取data中的数据
            console.log(this.test);
        },
        created(){  // 生命周期来到这里: 数据初始化完成, 可以获取data中的数据
            console.log(this.test);
        },
        beforeMount(){  // 生命周期来到这里: 检测挂载是否正确 (挂载: 把生成的vue实例显示到那个dom元素上)
        },
        mounted(){  // 生命周期来到这里: 挂载完成了, 视图渲染完
        },
        beforeUpdate(){  // data中的数据发生改变之前(视图上数据也没有完成显示), 如果没有改变不会出现这两个生命周期
        },
        updated(){  // data中的数据发生改变完成(视图上数据完成显示), 数据发生改变一般用watch监听
        }
    })
</script>
```

----

## mounted

一般在mounted这个生命周期里操作dom元素, 

```html
<div class="box">
    <p ref="p1">{{test}}</p>  // ref一种获取元素的方式, 存储的是对象
</div>

<script src="./js/vue.js"></script>
<script>
    let vm = new Vue({
        el: ".box",
        data: {
            test: "aaa"
        },
        mounted(){
            console.log(this.$refs);  // {p1: p}
            console.log(this.$refs.p1);  // <p>aaa</p>
        }
    })
</script>
```

----

## 全局组件

组件都是独立的, 有自己的生命周期, 自己的数据

```html
<div class="box">
    <v-box></v-box>
</div>

<script>
    Vue.component("vBox", {  // 定义一个全局组件
        template: "<div><p>{{msg}}</p></div>",
        data(){  // 组件有自己独立的数据, 并且以函数的形式返回数据
            return {
                msg: "独立的数据"
            }
        }
    });
    let vm = new Vue({
        el: ".box",
        data: {
        }
    })
</script>
```

----

## 局部组件

```html
<div id="app">
    <!-- 3. 使用组件 -->
    <component1></component1>
</div>

<script src="../js/vue.js"></script>
<script>
    // 1. 定义组件
     let component1 = {
        template: "<div><p @click='testFn'>{{val}}</p></div>",
        data(){
            return {
                val: "局部组件1"
            }
        },
        methods: {
            testFn(){
                console.log(1);
            }
        }
    };
    let vm = new Vue({
        el: "#app",
        data: {
        },
        // 2. 注册组件, 嵌套组件, 需要在组件中注册组件
        components: {
            component1,
            component2: {template: "<div><p>{{val}}</p></div>", data(){return {val: "局部组件2"}}}
        }
    })
</script>
```



