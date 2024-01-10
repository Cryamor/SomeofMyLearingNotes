# Vue3

- JavaScript框架
- 声明式、组件化、渐进式（灵活、可以被逐步集成）
- [Vue3官方中文文档](https://cn.vuejs.org/guide/introduction.html)
- 单文件组件（Single-File Components, SFC）或`*.vue`文件：类似HTML格式的文件，将一个组件的逻辑（JavaScript）、模版（HTML）和样式（CSS）封装在一个文件中
- API风格：选项式API / 组合式API

## 基础

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

可以访问到上述网址

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

`main.js `:

```javascript
import './assets/main.css'

import { createApp } from 'vue'
import App from './App.vue'

createApp(App).mount('#app')
```

每个Vue应用都是通过`createApp`函数创建的一个新的应用实例，传入其中的对象`App`是一个组件。

应用实例需要调用`mount()`方法挂载后才会渲染出来，该方法接收一个容器参数，可以是一个实际的DOM元素或CSS选择器字符串

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

HTML中属性的绑定使用`v-bind`指令

