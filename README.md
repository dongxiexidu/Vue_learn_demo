# Vue_learn_demo
 基础学习
 ### 1.hello
 ```vue
 <div id="app">
     {{message}}
     <span v-bind:title="message">hello world</span>
 </div>

 <div id="app2">
     {{message}}
 </div>

 <!--1.导入vue.js-->
 <script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>

 <script>
     var vm = new Vue({
         //el 属性 用来指示vue编辑器从什么地方开始解析Vue语法,可以说是一个占位符
         el: "#app",
         //data 属性 从Vue中抽象出来的属性,视图中的数据存放在data中
         data:{
             message: "hello, vue!"
         }

     });

     var vm2 = new Vue({
         el: "#app2",
         data:{
             message: "hello, vue2!"
         }

     });
 </script>
 ```
 
 ### 2.条件控制if
 ```vue
 <div id="app">

     <h1 v-if="type==='A'">A</h1>
     <h1 v-else-if="type==='B'">B</h1>
     <h1 v-else>C</h1>
 </div>

 <!--1.导入vue.js-->
 <script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>

 <script>
     var vm = new Vue({
         el: "#app",
         data:{
             ok: true,
             type: 'B'
         }
     });
 </script>
 ```
 ### 3.遍历数组for
 ```vue
 <div id="app">

     <li v-for="(item,index) in items">
         {{index}}-{{item.message}}
     </li>
 </div>

 <!--1.导入vue.js-->
 <script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>

 <script>
     var vm = new Vue({
         el: "#app",
         data:{
             items: [
                 {message: 'java'},
                 {message: 'html'},
                 {message: 'iOS'},
                 {message: 'css'}
             ]
         }
     });
 </script>
 ```

### 4.方法的定义及调用
```vue
<div id="app">

    <!-- 使用v-on 绑定click事件 -->
    <button v-on:click="sayHi">click me</button>
</div>

<!--1.导入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>

<script>
    var vm = new Vue({
        el: "#app",
        data:{
            message: "chuangzhnag"
        },
        methods: { // 方法必须定义在vue的methods对象中
            sayHi: function () {
                alert(this.message);
            }
        }
    });
</script>
```
### 5.输入绑定
```vue
<div id="app">

<!--    v-model 会忽略所有表单元素的value,checked,selected特性的初始值 -->
   输入的文本: <input type="text" v-model="message" > {{message}}

    <br>
    <textarea v-model="message"></textarea>
    <br>

    <!-- checked 默认选中-->
    性别: <input type="radio" name="gender" value="男"  v-model="gender">男
    <input type="radio" name="gender" value="女" v-model="gender">女

    <p>
        选中了: {{gender}}
    </p>

    <br>
    下拉框:
    <select name="" id="" v-model="selected">
        <option disabled value="请选择">请选择</option>
        <option>A</option>
        <option>B</option>
<!--  selected 默认选中      -->
        <option selected>C</option>
    </select>

    <span>{{selected}}</span>


</div>

<!--1.导入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.6.11"></script>

<script>
    var vm = new Vue({
        el: "#app",
        data: {
            message: "123",
            gender: '男',
            selected: '请选择'
        }
    });
</script>
```
### 6.自定义一个Vue组件component
```vue
<div id="app">

<!--遵循SoC关注度分离原则-->
<!--    组件: 传递给组件中的值: props -->
    <ldx v-for="item in items" v-bind:qin="item"></ldx>

</div>

<!--1.导入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21"></script>

<script>

//   自定义:定义一个Vue组件component
    Vue.component("ldx",{
        props: ['qin'],
        template: '<li>{{qin}}</li>'
    });

    let vm = new Vue({
        el: "#app",
        data: {
            items: ["java", "linux", "iOS"]
        },

    });
</script>
```

### 7.axios网络请求
axios 是一个基于Promise 用于浏览器和 nodejs 的 HTTP 客户端
```vue
<div id="vue" v-cloak>
    <div>名称：{{info.name}}</div>
    <div>地址：{{info.address.country}}-{{info.address.city}}-{{info.address.street}}</div>
    <div>连接：<a v-bind:href="info.url" target="_blank">{{info.url}}</a> </div>
    <div>数组：{{info.links[0].name}}: <a v-bind:href="info.links[2].url" target="_blank">{{info.links[2].url}}</a>
    </div>
</div>

<!--引入 JS 文件-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21/dist/vue.js"></script>
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>

<script type="text/javascript">
    var vm = new Vue({
        el: '#vue',
        data() { // 这个不是data属性, 这个是data() 方法
            return {
                info: { // 只写需要用到的字段,也可以不写,也可以只写一个info
                    name: null,
                    address: {
                        country: null,
                        city: null,
                        street: null
                    },
                    links: [
                        {
                            name: null,
                            url: null
                        }
                    ],
                    url: null
                }
            }
        },
        mounted() { //钩子函数 链式编程
            axios
                .get('../data.json')
                .then(response => (this.info = response.data));
        }
    });
</script>
```

解决闪烁问题
```vue
<head>
    <meta charset="UTF-8">
    <title>Title</title>

<!--    解决闪烁问题: 本质就是一开始白屏,有数据后再显示 -->
    <style>
        [v-cloak]{
            display: none;
        }
    </style>

</head>
```

### 8.计算属性
```vue
<div id="app">

    <div>
        <!--    调用方法  -->
        <p>currentTime1={{currentTime1()}}</p>
        <!--    使用计算属性  -->
        <p>currentTime2={{currentTime2}}</p>
    </div>
</div>

<!--1.导入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21"></script>

<script>


    let vm = new Vue({
        el: "#app",
        data: {
            message: "hello, ldx"
        },
        methods: {
            currentTime1: function () { // 方法调用,每次都会重新计算
                return Date.now(); // 返回一个时间戳
            }
        },
        computed: { // 计算属性,会缓存数据,浏览器中console数据不会变
            currentTime2: function () {
                return Date.now(); // 返回一个时间戳
            }
        }

    });
</script>
```
### 9.slot: 插槽
```vue
<div id="app">

    <todo>
        <todo-title slot="todo-title" :title="title"></todo-title>
        <todo-items slot="todo-items" v-for="item in todoItems" :item="item"></todo-items>
    </todo>

</div>

<!--1.导入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21"></script>

<script>

    // 内容分发
    // slot: 插槽
    Vue.component("todo",{
        template: '<div>\
                <slot name="todo-title"></slot>\
                <ul>\
                  <slot name="todo-items"></slot>\
                </ul>\
             </div>'
    })

    Vue.component("todo-title",{
        props: ['title'],
        template: '<div>{{title}}</div>'
    })

    Vue.component("todo-items",{
        props: ['item'],
        template: '<li>{{item}}</li>'
    })

    let vm = new Vue({
        el: "#app",
        data: {
            title: "ldx列表",
            todoItems: ['java', 'iOS', 'swift'],
        }
    });


</script>
```

### 10.内容分发
```vue
<div id="app">
    
    <!--遵循SoC关注度分离原则-->
    <todo>
        <todo-title slot="todo-title" v-bind:title="title"></todo-title>
        <todo-items slot="todo-items" v-for="(item,index) in todoItems"
                    :item="item" v-bind:index="index" v-on:remove="removeItems(index)"></todo-items>
    </todo>

</div>

<!--1.导入vue.js-->
<script src="https://cdn.jsdelivr.net/npm/vue@2.5.21"></script>

<script>

    // 内容分发
    // slot: 插槽
    Vue.component("todo",{
        template: '<div>\
                <slot name="todo-title"></slot>\
                <ul>\
                  <slot name="todo-items"></slot>\
                </ul>\
             </div>'
    })

    Vue.component("todo-title",{
        props: ['title'],
        template: '<div>{{title}}</div>'
    })

    Vue.component("todo-items",{
        props: ['item','index'],
        template: '<li>{{index}}--{{item}} <button @click="remove">删除</button> </li>',
        methods: {
            remove: function (index) {
                // 组件内部绑定事件使用到 this.$emit('事件名', 参数)
                this.$emit('remove', index);
            }
        }
    })

    let vm = new Vue({
        el: "#app",
        data: {
            title: "ldx列表",
            todoItems: ['java', 'iOS', 'swift'],
        },
        methods: {
            removeItems: function (index) {
                console.log("删除-"+this.todoItems[index]+"-ok");
                this.todoItems.splice(index, 1);
            }
        }
    });


</script>
```
