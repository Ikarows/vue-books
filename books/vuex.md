# vuex

### 安装
```
// npm
npm install vuex --save

// cnpm
cnpm install vuex --save
```

### main.js 中注册
```
import Vue from 'vue'
import App from './App'
import store from './store' //全局注册

Vue.config.productionTip = false

new Vue({
    el: '#app',
    store, //全局注册
    components: { App },
    template: '<App/>'
})

```

### 项目结构
```
└── store
    ├── index.js          # 我们组装模块并导出 store 的地方
    └── modules
        ├── user.js       # 用户模块
        └── cart.js       # 购物车模块
```

### 基础代码与流程
```
//index.js
import Vue from "vue"
import Vuex from "vuex"
import user from './modules/user'
 
Vue.use(Vuex)
 
const store = new Vuex.Store({
    modules: {
        user
    }
})

export default store;
```

#### 
```
/*
* user.js
* 流程：actions -> mutations -> state
*/
import axios from "axios"
import api from '@/api'

export default {

    // 3.存储数据
    state: {
        todo: 'this is vuex data'
    },
    // 2.修改数据
    mutations: {
        setTodo (state, item) {
            state.todo = item;
        }
    },
    // 1.获取数据
    actions: {
        getData (store, msg) {
            //调用setTodo 分发数据进行修改
            store.commit('setTodo', data);
        }
    }
}
```

### 组件调用
```
export default {
    name: '',

    data () {
        return {
          msg: "",
        }
    },

    mounted: function () {
        //this.todo 使用数据
        this.msg = this.todo
    },

    computed: {
        //定义方法获取 state 数据
        todo () {
            return this.$store.state.user.todo;
        }
    }
}
```

### 模版调用
```
{{ todo }}
```

For a detailed explanation on how things work, check out the guide [veux](https://vuex.vuejs.org/zh/).