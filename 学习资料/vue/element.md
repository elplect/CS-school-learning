# vue-element
针对PC端的UI库

官网 https://element-plus.gitee.io/#/zh-CN/component/layout

### 安装

#### 1.完整安装

1. npm

   npm install element-plus --save

2. cdn

   ```html
   <!-- 引入样式 -->
   <link rel="stylesheet" href="https://unpkg.com/element-plus/lib/theme-chalk/index.css">
   <!-- 引入组件库 -->
   <script src="https://unpkg.com/element-plus/lib/index.full.js"></script>
   ```

#### 2.按需引入

1. 安装 babel-plugin-component

   npm install babel-plugin-component -D

2. 修改 babelrc

   ```javascript
   {
     "plugins": [
       [
         "component",
         {
           "libraryName": "element-plus",
           "styleLibraryName": "theme-chalk"
         }
       ]
     ]
   }
   ```

3. 引入所需插件

   ```javascript
   import { ElButton, ElSelect } from 'element-plus';
   const app = createApp(App)
   app.component(ElButton.name, ElButton); // app.use(ElButton)
   app.component(ElSelect.name, ElSelect); // app.use(ElSelect)
   ```

   