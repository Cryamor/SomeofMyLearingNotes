# Vue3

- JavaScript框架
- 声明式、组件化、渐进式（灵活、可以被逐步集成）
- [Vue3官方中文文档](https://cn.vuejs.org/guide/introduction.html)
- 单文件组件（Single-File Components, SFC）或`*.vue`文件：类似HTML格式的文件，将一个组件的逻辑（JavaScript）、模版（HTML）和样式（CSS）封装在一个文件中
- API风格：选项式API（旧） / 组合式API（新）

## Before Start

### 安装与启动

#### NPM方法

```sh
# 查看版本
npm -v
```

在工作目录：

```sh
npm create vue@latest
```

这一指令将安装并执行`create-vue`，按照提示输入项目名和TypeScript、测试等选择可选功能支持（可以先全否）

安装依赖：

```sh
npm install
```

启动开发服务器：

```sh
npm run dev
```

```sh
VITE v4.5.0  ready in 536 ms

  ➜  Local:   http://localhost:5173/
  ➜  Network: use --host to expose
  ➜  press h to show help 
```

发布到生产环境时，使用：

```sh
npm run build
```

会生成一个`dist`文件夹

#### 仪表盘

```sh
vue ui
```

进行图形化操作

### 创建应用

用`npm create`生成的项目中的示例组件使用的是组合式API和`<script setup>`

`index.html`：

```html
<body>
    <div id="app"></div>
    <script type="module" src="/src/main.ts"></script>
</body>
```

引入`main.ts`

`main.ts `:

```javascript
import './assets/main.css'

import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```

每个Vue应用都是通过`createApp`函数创建的一个新的应用实例，传入其中的对象`App`是一个组件。

应用实例需要调用`mount()`方法挂载后才会渲染出来，该方法接收一个容器参数，可以是一个实际的DOM元素或CSS选择器字符串，`#app`即`index.html`中的`<div id="app"></div>`

`createApp`允许在同一页面中创建多个共存的Vue应用，每个应用拥有自己的作用域，适用于控制如一个大型页面中的一小部分：

```javascript
const app1 = createApp({
  /* ... */
})
app1.mount('#container-1')

const app2 = createApp({
  /* ... */
})
app2.mount('#container-2')
```

## 语法

### 模板语法

基于HTML的模板语法，在底层编译成高度优化的JavaScript代码

#### 插值

##### 文本

Mustache语法，即双大括号，双大括号将数据解释为纯文本，因此HTML不适用：

```vue
<span>Message: {{ msg }}</span>
```

标签内容会被替换为相应组件实例中`msg`属性的值，且同步更新

如果不想同步更新，可以使用`v-once`指令一次性插值，数据改变时插值内容不会同步更新：

```vue
<span v-once>Message: {{ msg }}</span>
```

##### HTML

使用`v-html`指令插入HTML代码：

```vue
<p>Using text interpolation: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>
```

##### 属性

`v-bind`单向绑定（`v-bind`可省略只留冒号）：

```vue
<input v-bind:value="info.name">
```

`v-model`双向绑定：

```vue
<input v-model="info.name">
```

### 选项式API

数据、方法、计算属性、监视等分散在data、methods、computed、watch等部分

#### 示例

完成一个Person组件：

`Person.vue`：

```vue
<template>
    <div class="person">
        <h2>Name: {{ name }}</h2>
        <h2>Age: {{ age }}</h2>
        <button @click="modifyName">Modify Name</button>
        <button @click="modifyAge">Modify Age</button>
        <button @click="showID">show ID</button>
    </div>
</template>

<script lang="ts">
    export default {
        name: "Person",
        data() {
            return {
                name:'ZhangSan',
                age:18,
                ID:'41000019991231000X'
            }
        },
        methods:{
            showID(){
                alert(this.ID)
            },
            modifyName(){
                this.name='zhang san'
            },
            modifyAge(){
                this.age+=1
            }
        }
    }
</script>

<style>
    .person {
        background-color: rgb(161, 231, 250);
        box-shadow: 0 0 10px;
        border-radius: 10px;
        padding: 10px;
    }
    button {
        margin: 0 10px;
        border-radius: 10px;
    }
</style>
```

`App.vue`中注册组件：

```vue
<template>
    <div class="app">
        <h1>Hello!</h1>
        <Person></Person>
    </div>
</template>

<script lang="ts">
    // JS or TS
    import Person from './components/Person.vue'
    export default {
        name: "App",
        components:{Person} // 需要显式注册组件
    }
</script>

<style>
    .app {
        background-color: white;
        box-shadow: 0 0 10px;
        border-radius: 20px;
        padding: 20px;
    }
</style>
```

#### 弊端

若要新增或修改一个需求，则需要分别修改data、methods、computed，不利于维护

### 响应式：`setup()` `ref()` `reactive()`

#### 基本类型数据

组合式API，将data、methods、computed等全部写入。

`<script setup>`语法糖，不需要暴露（return），无需注册组件：

```vue
<script lang="ts" setup>
    import {ref} from 'vue'

    let name = ref('ZhangSan')
    let age = ref(18)
    let id = '41000019991231000X'

    function modifyName() {
        name.value = 'zhang san'
    }
    function modifyAge() {
        age.value += 1
    }
    function showID() {
        alert(id)
    }
</script>
```

- let不加`ref()`是非响应式的
- 用`ref()`包裹在`RefImpl`对象中，用`.value`访问（模板中不需要）
- data、methods等能读取setup中的内容，反之不可以

#### 对象类型数据

`reactive()`将对象包裹在`Proxy`对象中：

```vue
<script lang="ts" setup>
    import {reactive} from 'vue'

    let info = reactive({name:'ZhangSan', age:18})

    function modifyName() {
        info.name = 'zhang san'
    }
    function modifyAge() {
        info.age += 1
    }
</script>
```

- `reactive()`是深层次的，即响应式对象内的嵌套对象依然是代理
- `reactive()`只能定义对象类型的响应式数据
- `ref()`也能定义对象类型响应式数据，value是一个`Proxy`对象（底层是`reactive()`）
- `reactive()`重新分配一个对象时会失去响应式，需要使用`Object.assign()`

#### `toRefs()`和`toRef()`

将响应式对象解构，`Proxy`→`RefImpl`

### 计算属性`computed()`

```vue
<script setup>
import { ref, computed } from 'vue'

const firstName = ref('John')
const lastName = ref('Doe')

const fullName = computed({
  // getter
  get() {
    return firstName.value + ' ' + lastName.value
  },
  // setter
  set(newValue) {
    // 注意：我们这里使用的是解构赋值语法
    [firstName.value, lastName.value] = newValue.split(' ')
  }
})
</script>
```

- 计算属性有缓存，方法无缓存
- 计算属性默认只读，可写时需要提供getter和setter

### `watch()`监视

#### 参数

watch的第一个参数是数据源，可以是：

- ref（不需要`.value`）
- reactive
- getter函数
- 多个数据源组成的数组

```typescript
const x = ref(0)
const y = ref(0)

// 单个 ref
watch(x, (newX) => {
  console.log(`x is ${newX}`)
})

// getter 函数
watch(
  () => x.value + y.value,
  (sum) => {
    console.log(`sum of x + y is: ${sum}`)
  }
)

// 数组
watch([x, () => y.value], ([newX, newY]) => {
  console.log(`x is ${newX} and y is ${newY}`)
})
```

#### 停止监视

在watch函数中调用返回的函数：

```typescript
const stop = watch(sum, (newValue, oldValue)=>{
    console.log("sum change from ", oldValue, " to ", newValue)
    if (newValue >= 10)
        stop()
})
```

#### 深度监视

1. 直接给 `watch()` 传入一个响应式对象（`reactive`），会**隐式**地创建一个深层侦听器，该回调函数在所有嵌套的变更时都会被触发

2. 对于`ref`则只监视地址，可以**显式**地加上`deep`选项，强制转换为深层监视：
   
   ```typescript
   watch(source, (newValue, oldValue)=>{
       console.log(oldValue, newValue)   
   },{deep:true})
   ```

#### 一次性监听器

回调只在源变化时触发一次，使用 `once: true` 选项：

```typescript
watch(source, (newValue, oldValue)=>{
    console.log(oldValue, newValue)   
},{once:true})
```

#### `watchEffect()`

- `watch` 只追踪明确侦听的数据源，更精确
- `watchEffect` 自动追踪所有能访问到的响应式属性，更方便简洁

```typescript
watchEffect(()=>{
    console.log(person.name,person.age)
})
```

### 模版引用

先声明一个ref：

```typescript
const input = ref(null)
```

然后在标签中加入：

```typescript
<input ref="input">
```

挂在之后便可引用：

```typescript
console.log(input.value)
// <input>
```

### 条件渲染`v-if`

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

### 循环渲染`v-for`

### Hooks

## 生命周期

![lifecycle_zh-CN.FtDDVyNA.png](./assets/lifecycle_zh-CN.FtDDVyNA.png)

Vue2到Vue3变化：

```
Vue2--------------vue3
beforeCreate  -> setup()
created       -> setup()
beforeMount   -> onBeforeMount
mounted       -> onMounted
beforeUpdate  -> onBeforeUpdate
updated       -> onUpdated
beforeDestroy -> onBeforeUnmount
destroyed     -> onUnmounted

activated     -> onActivated
deactivated   -> onDeactivated
errorCaptured -> onErrorCaptured
```

除了这些钩子函数外，Vue3还增加了`onRenderTracked` 和`onRenderTriggered`函数

## 路由Router

在`index.ts`中声明所用的路由，Vue3中必须声明路由模式：

```typescript
// src/router/index.ts

const router = createRouter({
    history:createWebHistory(),  // 路由模式
    routes:[
        {
            path:'/home',
            component:Home
        },
        {
            name:{
            name:'news',
            path:'/news',
            component:News,
            children:[
                {name:'news-detail',path:'detail',component:Detail}
            ]
        },
        {
            path:'/about',
            component:About
        },
        {
            path:'/',
            redirect:'/home' 
        }
    ]
})

export default router  // 暴露
```

此外还需要在`main.ts`中调用`use()`：

```typescript
import { createApp } from 'vue'
import App from './App.vue'
import router from './router'

const app = createApp(App)
app.use(router)
app.mount('#app')
```

### `RouterLink`和`RouterView`

```html
  <div class="app">
    <h2 class="title">Navigation</h2>
    <div class="navigation">
      <RouterLink to="/home">Home</RouterLink>
      <RouterLink to="/news">News</RouterLink>
      <RouterLink :to="{path:'/about'}">About</RouterLink>
    </div>
    <h2 class="title">Demonstration</h2>
    <div class="demonstration">
      <p>Content</p>
      <hr>
      <RouterView></RouterView>
    </div>
  </div>
```

`<RouterLink replace ...>`开启浏览器历史记录替换模式

### query参数

- 形如`news?a=1&b=2&c=3`

- 使用方法：先`useRoute`，然后形如`route.query.a`即可找到query中的项：
  
  ```typescript
  import { useRoute } from 'vue-router';
  let route = useRoute()
  ```

### params参数

- 形如`/news/a/b/c`

- 传递params参数时，不能使用path，需要提前在路由规则中占位：

```typescript
name:'news',
path:'/news/:id/:title',
component:News,typescript
```

```html
<RouterLink :to="{
    name:'news-detail',
    params:{
        id:news.id,
        title:news.title,
        content:news.content
    }
}">
    {{news.title}}
</RouterLink>
```

### props

先在规则中设置`props`为`true`：

```typescript
         {
            name:'news',
            path:'/news',
            component:News,
            children:[
                {
                    name:'news-detail',
                    path:'detail/:id/:title/:content',
                    component:Detail,
                    props:true
                }
            ]
        },    
```

用`defineProps()`调用后直接使用：

```
<template>
    <div class="detail">
        <ul class="news-content">
            <li>No.: {{ id }}</li>
            <li>Title: {{ title }}</li>
            <li>Content: {{ content }}</li>
        </ul>
    </div>
</template>

<script setup lang="ts" name="Detail">
    defineProps(['id','title','content'])
</script>
```

### 编程式路由导航

```typescript
import {useRouter} from 'vue-router'
const router = useRouter()
...

router.push('./')
```

## Typescript

声明一个接口，`export`分别暴露：

```typescript
// src/types/index.ts
export interface PersonI {
    id:string,
    name:string,
    age:number,
    addr?:string
}
```

```typescript
<script lang="ts" setup>
    import {type PersonI} from '@/types'

    let person:PersonI = {id:"11145141919810",name:"zhang",age:18}
    let personList:Array<PersonI> = [
        {id:"1234",name:"li",age:20},
        {id:"5678",name:"wang",age:22,addr:"Shanghai"}
    ]
</script>
```

类型引用前要加`type`
