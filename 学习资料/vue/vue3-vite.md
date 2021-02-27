## Vue3学习

### 1.安装

#### 1.CDN

```html
<script src="https://unpkg.com/vue@next"></script>
```

#### 2.npm

```bash
$ npm install vue@next
```

#### 3.CLI

```bash
npm install -g @vue/cli
```

#### 4.Vite

```bash
npm init @vitejs/app <project-name>
cd <project-name>
npm install
npm run dev
```

### 2.使用

从前的 data、computed、methods、watch、生命周期钩子之类的四分五裂的东西全都整合在一个组合API里面.

这个组合API是谁呢?没错,就是setup函数

将上述内容,全部定义在setup函数里,然后return 出来.接着就可以使用了.

同时也可以减少单文件的代码量

```javascript
// a.js 
export default function useRemoveStudent() {
  let state = reactive({
    stus:[
      {id:1,name:'zs'},
      {id:2,name:'ls'}
    ]
  })
  function remStu(index) {
    state.stus = state.stus.filter((stu,idx)=》 idx !== index)
  }
  return {state.remStu}
}
// a.vue
import useRemoveStudent from './a.'
setup() {
  let {state,remStu} = useRemoveStudent();
}
```

#### 1.应用配置

```js
const app = Vue.createApp({})
app.config = {...}
app.mount(...);
```

`config` 是一个包含了 Vue 应用全局配置的对象。你可以在应用挂载前修改其以下 property：

1. errorHandler

   1. ```js
      app.config.errorHandler = (err, vm, info) => {
        // 处理错误
        // `info` 是 Vue 特定的错误信息，比如错误所在的生命周期钩子
      }
      ```

   2. 获取渲染执行期间/侦听器 抛出的 未捕获错误. 用来获取错误信息和应用实例.

2. warnHandler

   1. ```js
      app.config.warnHandler = function(msg, vm, trace) {
        // `trace` 是组件的继承关系追踪
      }
      ```

   2. 为警告指定一个自定义处理函数

3. globalProperties

   1. 全局property

4. isCustomElement

5. optionMergeStrategies

6. performance

#### 2.应用API

1. component
   1. 全局组件
2. config
   1. 应用配置
3. directive
   1. 自定义全局指令
4. mixin
   1. 返回应用实例
   2. 不懂...
5. mount
   1. 根组件实例.挂载到DOM元素上.
6. provide
   1. 再说
7. unmount
   1. 卸载组件
8. use
   1. 安装js插件.

#### 3.重要函数

1. rel/reactive
2. shallowRef/shallowReactive
3. toRaw
4. markRaw
5. toRef
6. toRefs
7. customRef
8. ref
9. readonly

