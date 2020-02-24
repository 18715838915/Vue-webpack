
<!-- TOC -->

- [webpack和Vue的基本使用](#webpack和vue的基本使用)
    - [1.0.1. 学习要点](#101-学习要点)
    - [1.0.2. Webpakc基本配置](#102-webpakc基本配置)
    - [1.0.3. webpack中使用Vue](#103-webpack中使用vue)
    - [1.0.4. 使第三方loader导入 *.vue文件](#104-使第三方loader导入-vue文件)
    - [1.0.5. Vue-router的使用](#105-vue-router的使用)
- [Vue结合MUI、Mint-UI的简单应用](#vue结合muimint-ui的简单应用)
    - [vue-router结合MUI的tabbar组件来实现手机主页](#vue-router结合mui的tabbar组件来实现手机主页)

<!-- /TOC -->



## webpack和Vue的基本使用
***
### 1.0.1. 学习要点
1. webpack的基本使用
2. vue项目的实践应用
3. vue组件的应用
4. 使用Vue的组件Mint-UI
5. 结合MUI开发一个伪APP
***
### 1.0.2. Webpakc基本配置

   #### 初始化项目
- 使用VScode打开一个文件（注意：打开的文件夹的绝对路径不要包含中文)

- 在根目录中创建一个源文件的存放路径src文件夹   在终端中可用指令  *mkdir src*

- 在终端下使用 *npm init -y* 指令   （使用该指令需要安装Node,请提前安装) 
   - 使用该指令后，会在根目录中出现package.json文件
- 在终端下使用 *npm install webpack webpack-cli -D* 指令安装webpack
   - 创建文件
      - 在根目录下创建名为 webpack.config.js文件 文件名固定
      - 在src目录创建入口文件  main.js文件

- 在webpack.config.js 中配置webpack的入口和出口文件
```
 const path = require('path')
 module.exports = {
   // 入口
   entry: path.join(__dirname, './src/main.js'),     
   // 出口
   output: {
     // 出口目录
     path: path.join(__dirname, './dist'),
     filename: 'bundle.js'
   },
   // 开发模式
   mode: 'development'
 }
```
- 在package.json的scripts节点中添加一行脚本指令   
> "build": "webpack",

- 使用指令 *npm run build* 即可将main.js文件转换为bundle.js文件（出口文件)

***
### 1.0.3. webpack中使用Vue

 - 先使用*npm install vue -S* 安装Vue依赖
 - 在main.js文件中导入该包（在这里若直接导入需要配置webpack.config.js  否则导入的vue功能不全，无法使用）
   - 在webpack.config.js中配置如下
>     resolve:{
>     alias:{
>       "vue$":"vue/dist/vue.js"
>     }
>   }
- 在main.js中导入该依赖  *import Vue from 'vue'*
- 然后就可直接使用创建出Vue实例并使用了 O(∩_∩)O哈哈~

***
### 1.0.4. 使第三方loader导入 *.vue文件
- 在src目录下创建名为 *App.vue*的vue文件  其实该文件是vue的自定义组件
- 在main.js中引入该文件   *import App from './App.vue'*
- 安装可解析vue文件的loader和complier
   - 指令  *npm install vue-loader vue-template-compiler -D*
   - 在webpack.config.js中配置如下
      - 导入插件包
      > const VueLoaderPlugin=require('vue-loader/lib/plugin')
      - 配置该插件
          > plugins:[ new VueLoaderPlugin() ],
        
          
      - 配置加载loader匹配规则
      
        > module:{ rules:[ {test:/\.vue$/,use:'vue-loader'} ] ]
      
- 将该组件通过render的方式渲染到index.html中
   - 在vue的实例配置内部添加如下代码  
   
      >render:paste =>paste(App)
      - render函数与Vue的data和el属性是同级的
      - 这里的函数写法是使用ES6的箭头函数，更多render写法请自行百度
      - 参数App即我们之前自定义并导入的组件
- 使用 *npm run build* 构建脚本指令即可编译该文件类型 
- 最后记得在index.html中导入被webpack编译后的输出文件即可使用
***

### 1.0.5. Vue-router的使用

> 使用Vue-router的好处在于可以根据设定的router-link切换不同的vue组件，达到动态切换的效果，例如手机APP的底部导航栏就可以使用它来完成

- 先使用 **npm install vue-router** 安装vue-router模块
- 在src目录下创建component目录并在下面创建一个 index.vue文件
- 在src目录下创建router.js文件来管理所有的路由配置，并添加如下代码
```
import Vue from 'vue'
import Router from 'vue-router'

Vue.use(Router)

//导入组件
import Home from './component/index.vue'

//创建router对象
var routerObj = new Router({
    //设置匹配路由规则
    routes:[
        {path:'/index',component:Home}    //设置路由匹配规则
    ]
})

//向外暴露路由对象
export default routerObj;
```
- 在main.js文件中导入router文件中暴露 的对象并设置
   - `import routerObj from './router.js'`    
   - `router:routerObj`     
- 在App.vue组件中添加如下代码

```
        <router-link to="/index">主页</router-link>
        <router-view></router-view>
```


## Vue结合MUI、Mint-UI的简单应用

***
### vue-router结合MUI的tabbar组件来实现手机主页

- 我们先到mui官网下载mui的安装包，并放到src目录下

- 导入mui的样式文件mui.css
   - 注意：这里在编译时可能会报错，因为在这里我们导入了.css文件和.ttf后缀的文件,所以需要加载第三方loader，配置方法可[百度](https:www.baidu.com)

      ```
      //导入mui样式
      import './lib/mui/css/mui.min.css'
      ```
- 在App.vue文件中引入以下代码  

```
            <nav class="mui-bar mui-bar-tab">
               <a class="mui-tab-item mui-active" href="#tabbar">
                  <span class="mui-icon mui-icon-home"></span>
                  <span class="mui-tab-label">首页</span>
               </a>
               <a class="mui-tab-item" href="#tabbar-with-chat">
                  <span class="mui-icon mui-icon-email"><span class="mui-badge">9</span></span>
                  <span class="mui-tab-label">消息</span>
               </a>
               <a class="mui-tab-item" href="#tabbar-with-contact">
                  <span class="mui-icon mui-icon-contact"></span>
                  <span class="mui-tab-label">通讯录</span>
               </a>
            </nav>
 ```
- 将a标签全部切换为router-link 并且配置相应的路由地址

- 然后再router.js中配置路由地址

- 修改高亮
   1. 在App.vue中添加样式
   ```
   .mui-bar-tab .mui-tab-item.mui-active{
	color: #007aff;
   }
   ```
   2. 在router对象中添加
   > linkActiveClass:'mui-active'   //覆盖默认的路由高亮的类 link-active-class