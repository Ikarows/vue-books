# router

### 安装
```
//npm
npm install vue-router

//cnpm
cnpm install vue-router

//cdn
<script src="https://unpkg.com/vue-router@3.0.1/dist/vue-router.js"></script>

```

### main.js 中注册
```
import Vue from 'vue'
import App from './App'
import router from './router' //全局注册

Vue.config.productionTip = false

new Vue({
	el: '#app',
	router, //全局注册
	components: { App },
	template: '<App/>'
})

```

### 基础代码
```
// router/index.js

import Vue from 'vue'
import Router from 'vue-router'
import HelloWorld from '@/components/HelloWorld'

Vue.use(Router)

export default new Router({
	routes: [{
		path: '/',
		component: HelloWorld
	}]
})

```

### 使用方式

> 普通路由

```
export default new Router({
	routes: [{
		path: '/',
		component: HelloWorld
	}]
})
```

> 子路由

```
export default new Router({
	routes: [
		{
			path: '/',
			component: HelloWorld,
			children: [
		        {
		          path: 'profile',
		          component: UserProfile
		        }
		    ]
		}
	]
})
```

> 按需加载

```
//使用按需加载的话就不用去import组件进来了，推荐使用此方式

export default new Router({
	routes: [
		{
			path: '/',
			component: resolve => require(['@/components/HelloWorld'], resolve),
			children: [
		        {
		          path: 'profile',
		          component: resolve => require(['@/components/child'], resolve)
		        }
		    ]
		}
	]
})

```

### 组件页面上的链接使用，替代a标签

```
// 方式1：

<!-- 普通链接 -->
<router-link :to="{ path: '/child' }">跳转到第二个页面</router-link> 

<!-- 带参数的链接 -->
<router-link :to="{ path: '/child/page', query: {userid: 123} }">跳转到第三个页面</router-link> 

<!-- 获取路由参数 -->
<p>{{this.$route.query.userid}}</p> 

// 方式2：

<router-link to="/game?age=20">to game</router-link>

<!-- 获取路由参数 -->
<p>{{this.$route.query.age}}</p> 

```

### 动态跳转

```
export default {
	name: 'game',
	mounted: function () {

		this.$router.push('home')

        //对象
        this.$router.push({ path: 'home' })

        //命名的路由 这里会变成 /user/123
        this.$router.push({ name: 'user', params: { userId: 123 }})

        //带查询参数，变成 /register?plan=private
        this.$router.push({ path: 'register', query: { plan: 'private' }})
	}
}
```

### 重定向

```
export default new Router({
	routes: [{
		path: '/',
		redirect: '/b',
		component: HelloWorld
	}]
})

```

### router.go(n)

```
// 在浏览器记录中前进一步，等同于 history.forward()
router.go(1)

// 后退一步记录，等同于 history.back()
router.go(-1)

// 前进 3 步记录
router.go(3)
```

### 全局钩子函数（可以做一些全局性的路由拦截，如登录验证） 写在main.js中

```
router.beforeEach((to, from, next)=>{
  //do something
  next();
});
router.afterEach((to, from, next) => {
    console.log(to.path);
});
```

### 单个页面钩子函数 写在router/index.js中

```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      // 配置哪几个页面需要登录的时候，你可以在meta中加入一个 requiresAuth标志位,然后在 全局钩子函数 beforeEach中去校验目标页面是否需要登录。
      meta: { requiresAuth: true },
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

### 登录验证实例

```
router.beforeEach((to, from, next) => {
  if (to.matched.some(record => record.meta.requiresAuth)) {
    //校验这个目标页面是否需要登录
    if (!auth.loggedIn()) {  
      next({
        path: '/login',
        query: { redirect: to.fullPath }
      })
    } else {
      next()
    }
  } else {
    next() // 确保一定要调用 next()
  }
})

//这个auth.loggedIn 方法是外部引入的，你可以先写好一个校验是否登录的方法，再import进 router.js中去判断。
```
 
For a detailed explanation on how things work, check out the guide [router](https://router.vuejs.org/zh/).
