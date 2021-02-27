### Vue Router
Vue Router和v-if/v-show一样，是用来切换组件 显示的.

v-if/v-show 是标记来切换(true/false)

Vue Router 是哈希来切换的(#/xxx)

比v-if和v-show强大的是Vue Router不仅仅能切换组件的显示，还能在切换的时候传递参数

Vue Router使用
1. 导入 Vue Router

2. 定义路由规则

   ```html
   <script>
     const routes = [
       {path : '/',component : one},
       {path : '/one',component : one},
       {path : '/two/:name/:age',component: two}
     ];
   </script>
   ```

3. 根据路由规则创建路由对象

   ```html
   <script>
     const router = new VueRouter({
       routes : routes,
       linkActiveClass : "nj-active"  // 激活组件 添加类
     })
   </script>
   ```

4. 将路径对象挂载到Vue实例中

   ```html
   <script>
     let vue = new Vue({
       router : router
     })
   </script>
   ```
   
5. 修改URL哈希值

   ```html
   <div id="app">
       <router-link to="/one?name=elplect&age=20" tag="button">切换到第一个页面</router-link>
       <router-link to="/two/qwerwsx123/21" tag="button">切换到第二个页面</router-link>
       <router-view></router-view>
   </div>
   ```

6. 通过<router-view>渲染匹配组件

   ```html
   <router-view></router-view>
   ```

   
