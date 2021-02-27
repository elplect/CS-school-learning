## Vuex

用于共享数据. 只要根组件 有 store,所有组件就都可以用store里的共享数据

否则 获取祖先数据、获取子孙数据、获取兄弟数据都过于麻烦.

通过方法,传递数据给父元素,通过prop给子元素传值..一层一层..

### 1.使用

1. 导入vuex

2. 创建vuex对象

   ```html
   <script>
   const store = new Vuex.Store({
     state : { // 这里的state相当于组件的data，用于保存共享数据
       msg : 0
     },
     mutations : { // 保存共享方法.
       mAdd(state) {
         state.msg += 1;
       },
     },
     getters : { // 保存计算属性
     } // 还有 module 和 action 看官方文档
   })
   </script>
   ```

3. 组件中添加vuex对象

   ```html
   <script>
     grandFather = {
       template : "#grandfather",
       store : store,
     }
   </script>
   ```

4. 使用 this.$store.state.xxx

   ```html
   <div id='app'>
     <p>grandfather:{{this.$store.state.msg}}</p>
   </div>
   <script>
     grandFather = {
       template : "#grandfather",
       store : store,
       methods : {
         add() {
   				this.$store.commit("mAdd");
         }
       }
     }
     let vue = new Vue({
       el : '#app',
       store:store,
       components : {
         'grandfather' : grandfather
   		}
     })
   </script>
   ```

