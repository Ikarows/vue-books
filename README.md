# vue

> 介绍篇：vue 全家桶

```bash
vue                                    // 构建用户界面                            
vuex                                   // 组件状态管理
router                                 // 路由
less                                   // css 预处理器less
axios                                  // api 数据交互
cli                                    // 脚手架
```

> 安装篇：从零搭建环境

```bash
必经操作：
1. 安装 node.js                        // https://nodejs.org/en/
2. 安装 git                            // https://git-scm.com/downloads
3. 右键使用 Git Bash Here 命令操作
4. 安装 vue                            // npm install -g vue
5. 安装脚手架 cli                       // npm install -g vue-cli
6. 选择一个磁盘                         // c:/
7. 创建一个基于 "webpack" 模板的新项目   // vue init webpack vue
? Project name new-vue
? Project description (A Vue.js project)
? Author Cosplay
? Vue build standalone
? Install vue-router? (Y/n) y
? Use ESLint to lint your code? (Y/n) n
? Setup unit tests with Karma + Mocha? (Y/n) n
? Setup e2e tests with Nightwatch? (Y/n) n

8. 进入创建好的vue项目目录               // cd vue
9. 启动"蓝灯"安装 npm                   // npm install
10. 启动项目                            // npm run dev
11. 编译打包                            // npm run build

可选装其它依赖：
12. 引用 jquery (引用其它库 css/js 同理)// 在 main.js中加入：import '@/assets/jquery.min.js'
13. 安装 less                          // npm install less less-loader --save
14. 安装 vuex                          // npm install vuex --save
15. 安装 axios                         // npm install axios


**: 如果npm在国内的网络环境下可能会比较慢，解决方案：

  使用淘宝镜像:

1>官方网址：http://npm.taobao.org；
2>安装：npm install cnpm -g --registry=https://registry.npm.taobao.org；  
  注意：安装完后最好查看其版本号cnpm -v或关闭命令提示符重新打开，安装完直接使用有可能会出现错误；
3>注：cnpm跟npm用法完全一致，只是在执行命令时将npm改为cnpm（以下操作将以cnpm代替npm）。
4>如何更新npm的方法：  可以使用 npm install -g npm 来更新版本

```

>项目搭建篇：目录结构

```bash
  .
  |-- build                            // 编译打包
  |-- config                           // 项目开发环境配置
  |   |-- index.js                     // 项目打包部署配置
  |-- src                              // 源码目录
  |   |-- api                          // axios api 数据交互
  |   |-- assets                       // 资源文件目录
  |       |-- css                      // css 样式目录
  |       |-- less                     // less 样式目录
  |           |-- base.less            // 主样式集合文件
  |           |-- _theme.less          // 主题文件  
  |       |-- fonts                    // 公共字体文件目录
  |       |-- images                   // 公共图片目录
  |       |-- js                       // 公共js目录
  |   |-- components                   // 公共组件
  |       |-- header.vue               // 页面头部公共组件
  |   |-- router                       // 路由配置和程序的基本信息配置
  |       |-- index.js                 // 配置页面路由
  |   |-- store                         // vuex的状态管理
  |       |-- index.js                 // 加载各种store模块
  |       |-- user.js                  // 用户store
  |   |-- views                        // 模版html文件
  |       |-- index                    // 网站首页
  |       |-- login                    // 登录
  |   |-- app.vue                      // 页面入口文件
  |   |-- main.js                      // 程序入口文件，加载各种公共组件
  |-- static                           // 静态资源目录
  |-- .babelrc                         // ES6语法编译配置
  |-- README.md                        // 项目说明
  |-- index.html                       // html 页面入口文件
  |-- package.json                     // 配置项目相关信息
  .
```

>模块使用篇

#### components 组件拆分与引用

```html
html:
<v-header></v-header>
```

```js
js:
import header from '@/components/header.vue' //引用组件进来
export default {
	data () {
		return {
			message: "hello world!",
		}
	},
    //组件
	components: {
		'v-header': header // 映射组件以致模版中使用
	},
    //方法
	methods: {

	},
    //初始化
	mounted: function () {

	}
}
```

#### axios
1.新建目录/文件 api > api.js

```js
写入代码：

const BaseUrl = 'https://api.imjad.cn/cloudmusic/?'

/**
 * api 接口地址
 *
 */

export default {
    getMusic( msg ) { return BaseUrl + "type=search&s=" + msg + "&limit=10"; }
}




```

2.header.vue
```js
import api from '@/api'
export default {
    mounted: function() {
        //axios
        var msg = "人渣的本愿";

        //1.配合vuex后这步就放在store/index.vue下了，由dispatch调用，例2.
        axios.get(api.getMusic(msg)).then(function(data){
            if(data.data.result.songCount > 0){
                store.commit("setdata", data.data.result.songs)
            }else{
                store.commit("setdata", [{name: "暂无数据"}])
            }
        }).catch((err) => {
            store.commit("setdata", [{name: "获取数据出错"}])
            console.log(err);
        })

        //2.配合 vuex的写法，调用获取数据：
        this.$store.dispatch('getData', '人渣的本愿');
    }
}
```

#### vuex

1.新建目录/文件 store -> index.js

```js
写入代码：
import Vue from "vue"
import Vuex from "vuex"
import axios from "axios"
import api from '@/api'
Vue.use(Vuex);

const store = new Vuex.Store({
    //0.存储变量数据
    state:{
        show: true,
        massage: "这是标题",
        todos: ''
    },
    //2.改变变量数据
    mutations:{
        setdata(state, item){
            state.todos = item;
        },
        setLoading(state, item){
            state.show = item;
        }
    },
    //1.获取数据传给 mutations改变变量数据（己经把原先组件上的获取数据放到了这里来处理）
    actions:{
        getData(store, msg){
            axios.get(api.getMusic(msg)).then(function(data){
				if(data.data.result.songCount > 0){
					store.commit("setdata", data.data.result.songs)
				}else{
					store.commit("setdata", [{name: "暂无数据"}])
				}
			}).catch((err) => {
				store.commit("setdata", [{name: "获取数据出错"}])
				console.log(err);
			})
        }
    },
    modules:{}
});

export default store;
```

2.在 main.js 里全局注册

```js
import Vue from 'vue'
import App from './App'
import router from './router'
import store from './store' // => 就是这个

new Vue({
  el: '#app',
  router,
  store, // => 还有这个
  template: '<App/>',
  components: { App }
})

```

3.组件js里调用 hello.vue

```js

export default {
    name: 'hello',
    //获取store里的变量数据，替代以前的data(){}方法，模版上直接使用如：{{massage}}即可
    computed: {
        arrs () {
            return this.$store.state.todos;
        },
        massage () {
            return this.$store.state.massage;
        },
        show () {
            return this.$store.state.show;
        }
    },
    methods: {

    },
    mounted: function () {
      //vuex
      this.$store.dispatch('getData', '人渣的本愿');
      //关闭loading
      this.$store.state.show = false
    }

}
```

4.组件页面上使用 hello.vue

```html

<p>{{massage}}</p>
```

#### router

1.组件页面上的链接使用，替代a标签

```html
<!-- 普通链接 -->
<router-link :to="{ path: '/child' }">跳转到第二个页面</router-link>
<!-- 带参数的链接 -->
<router-link :to="{ path: '/child/page', query: {userid: 123} }">跳转到第三个页面</router-link>
```

2.获取路由参数
```html

<p>{{this.$route.query.userid}}</p>
```

3.路由配置

```js

import Vue from 'vue'
import Router from 'vue-router'
import hello from '@/components/hello'
import child from '@/components/child'
import page from '@/components/page'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path: '/',
      name: 'hello',
      component: hello
    },
    {
        path: '/child',
        name: 'child',
        component: child
    },
    {
        path: '/child/page',
        name: 'page',
        component: page
    }
  ]
})

/* 注：路由的全局注入方面在webpack建项目的时候就己经安装好路由了，所以就不用再手动去注入，只要去配置路由的各种路径即可。 */
```

未完待续...
