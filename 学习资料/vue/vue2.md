## Vue 2 学习

---

### 1、安装

#### 1.CDN

<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

<script src="https://cdn.jsdelivr.net/npm/vue@2.6.12"></script>

#### 2.NPM

```shell
npm install vue
```

### 2、Vue核心

#### 1.模版

```html
<div id="app" v-for="">
  {{ message }}
</div>
```

```javascript
var app = new Vue({
  el: '#app',
  data: {
    message: 'Hello Vue!'
  },
  methods :{}
})
```

#### 2.Vue常用指令

```html
<div id="app" v-show="">
  {{ message }}
</div>
```

1. v-show
   1. 切换 display: none/other
2. v-if / v-else / v-else-if
   1. 虽可被vue-router代替,但是小组件的切换通过if来的直观
3. v-for
   1. 循环打印当前 元素
4. v-on: / @
   1. 绑定事件
5. v-bind / :
   1. 绑定属性
6. v-model
   1. 双向绑定数据 (控件/组件)
7. v-slot / #    **学习**
   1. 插槽
8. v-pre
   1. 跳过该元素以及子元素的编译,可加快编译速度
   2. 若一个元素没有,使用data,那么最好添加v-pre
9. v-cloak  **学习**
   1. 不知道啥用
10. v-once
    1. 只在最开始时渲染一次,之后重新渲染时,该元素以及子节点 将被视为 静态内容跳过
    2. 用于优化更新性能

#### 3.Vue常用选项

```javascript
var app = new Vue({
  el: '#app',
  data: function() {
    return {
      msg : 'hello,vue',
    }
  },
  methods: {
    say(){}
  },
  props: {},
  computed: {},
  watch: {}
})
```

1. name

   1. 组件名

2. el

   1. 挂载目标

3. data

   1. 存放Vue数据

4. methods

   1. 存放Vue方法
   2. 无缓存

5. props

   1. 接受父元素参数
   2. vuex代替

6. computed

   1. 计算属性
   2. 缓存 -- 性能好

7. watch

   1. 自动监听值的变化,然后触发函数

   2. ```javascript
      watch : {
        kilometer: function(newValue,oldValue){
          // 有2个参数,一个新值,一个旧值
        }  
      }
      ```

8. filter

   1. 过滤器

      1. 添加过滤器后,数据会自动执行filter函数

      2. ```html
         <div id="app">
             {{message | cap}}
             {{ message | filterA('arg1', arg2) }}
                // message第一个参数,arg1第二个参数,arg2第三个参数
             <script>
                 new Vue({
                     el : "#app",
                     data : {
                         message : 'runoob'
                     },
                     filters : {
                         cap : function (value) {
                             if (!value) return ''
                             value = value.toString()
                             return value.charAt(0).toUpperCase()+value.slice(1)
                         }
                     }
                 })
             </script>
         </div>
         ```

9. components

   1. 局部组件

      1. 还是一个vue,vue所有相关的数据都可以使用

      2. ```html
         <script>
           "grandson" : {
             template : "#grandson", // 模版
             components : {} // 子组件
           }
         </script>
         ```

10. template

    1. 替换模版

    2. ```html
       <template id="grandson">
           <div>
               <p>我是孙子组件</p>
           </div>
       </template>
       <script>
         "grandson" : {
         template : "#grandson",
           components : {}
         }
       </script>
       ```

11. director

    1. 自定义组件

    2. ```html
       <div id="app">
           <p v-color="'red'">我是段落</p>
           <p v-color="curColor">我是段落</p>
       </div>
       <script>
           Vue.directive("color",{
               bind:function(el,obj) {
                   el.style.color=obj.value;
               }
           })
           let vue = new Vue({
               el : '#app',
               data : {
                   curColor : 'green'
               }
           })
           let vue = new Vue({
               el :'#app',
               data : {},
               methods : {},
               directives : {
                   "color" : {
                       bind: function(el,obj) {
                           el.style.color = obj.value;
                       }
                   }
               }
           })
       </script>
       ```

12. render **学习**

13. extend 再说

14. provide / inject 再说

#### 3.Vue特殊属性

1. key

   1. 循环的时候加上就好
   2. 主要用于虚拟DOM算法,倒叙添加元素会用到
   3. 还可以触发过渡/ 生命周期钩子

2. ref

   1. Vue3 废弃了

   2. 操作DOM用.

   3. ```html
      <div id="app">
          <button @click="toggle">切换</button>
          <p ref="sq">{{msg}}</p>
      </div>
      <script>
          let vue = new Vue({
              el : "#app",
              data : {
                  msg : "elplect"
              },
              methods : {
                  toggle() {
                      // 在vue中，如果获取原生的元素或者获取自定义的组件，可以通过ref来获取
                      let sq = this.$refs.sq;
                      console.log(sq.innerHTML);
                  }
              }
          })
      </script>
      ```

3. is

   1. 动态选择组件用的,和<component>搭配使用

   2. ```html
      <button @click="toggle">切换</button>
      <component v-bind:is="isName"></component>
      <script>
          Vue.component('abc',{
              template: "#info1"
          });
          Vue.component('cba',{
              template : "#info2"
          });
          let vue = new Vue({
              methods : {
                  toggle() {
                      this.isName = this.isName === 'abc' ? 'cba':'abc';
                  }
              }
          })
      </script>
      ```

4. slot 插槽   

   1. 已废弃   v-slot

   2. ```html
      <father>
        <div>草</div>
      </father>
      <template id="father">
          <div>
              <p>我是父组件</p>
              <son></son>
              <slot>我是默认数据</slot>
          </div>
      </template>
      ```

#### 4.常用标签/内置组件

1. template

   1. 模版

2. component

   1. ```html
      <component :is="componentId"></component>
      ```

   2. 决定渲染哪一个组件

3. transition

   1. 过渡用
   2. 只能过渡第一个子元素

4. transition-group

   1. 过渡用
   2. 全部可以过渡

5. keep-alive

   1. 缓存
   2. 保留组件状态,避免重新渲染...主要用于切换

#### 5.生命周期钩子

1. beforeCreate
   1. vue刚创建,啥都还没
2. **created**
   1. 有了data、methods
   2. 没有编译模版 , 还没根据vue生成html
3. beforeMount
   1. 生成了html,但是还没有渲染到界面山,数据仅保存在内存
   2. {{msg}}还没替换
4. **mounted**
   1. 替换完成.意味着创建结束
5. **beforeUpdate**
   1. 代表Vue实例的数据被修改了
   2. 但是界面还没有更新
6. **updated**
   1. 界面更新完了
7. activated
   1. keep-alive缓存的组件激活的时候使用
8. deactivated
   1. keep-alive缓存的组件停用时调用
9. **beforeDestroyed**
   1. 销毁前,处理数据
10. **destroyed**
    1. 销毁后,通告其他组件,我销毁了
11. errorCaptured
    1. 子孙组件被错误调用的时候使用

#### 6.实例方法

基本就是 选项

只不过换成了操作 vue..而不是直接操作vue里的数据.

个人感觉不会很常用.

#### 7.全局API

vue里的局部方法,在全局的声明..

个人觉得也不会很常用,用到的时候查文档就行了.

#### 8.全局配置

创建项目后,根据文档配置一下就可以了

#### 9.其他

有兴趣看看

1. propsData
   1. 方便测试
2. parent
   1. 指向父实例
3. children
   1. 指向子实例
4. mixins
5. model

#### 10. 核心插件

1. vuex

   1. 移步vuex-study

2. vue-router

   1. 移步vue-router-study

3. 服务器渲染 等学习下述技术后再说

   1. webpack
   2. vue-loader
   3. Node.js
   4. 预渲染

4. axios

   1. ```html
      <script>
      mounted() {
        axios // 异步执行
          .get('../json/index.json')
          .then(response => (this.info = response.data)) // 回调函数
          .catch(function (error) { // 请求失败处理
          alert(error)
        });
      }
      </script>
      ```

#### 11.UI库

1. elementUI
   1. 查文档,直接复制粘贴
2. bootstrap-vue
   1. 查文档

#### 12.常用脚手架

1. vue-cli
   1. spa好,移步vue-cli-study
2. nuxt.js
   1. seo好

