# axios

### 安装
```
// npm
npm install axios

// cnpm
cnpm install axios

//cdn
<script src="https://unpkg.com/axios/dist/axios.min.js"></script>
```

### api

```
// api/index.js

/**
 * api 接口地址
 *
 */
//https://api.imjad.cn/hitokoto.md
const _base = 'https://api.imjad.cn';

export default {
    getData : _base + '/hitokoto/'
}

```


### 单独使用

```

//无参数
axios.get(api.getData).then(data => {
    console.log(data)
}).catch((err) => {
    console.log(err);
})

//带参数
let params = {
    id: '1',
    page: '1',
}
axios.get(api.getData, {params:params}).then(data => {
    console.log(data)
}).catch((err) => {
    console.log(err);
})


// post方法同上


// 添加请求拦截器
axios.interceptors.request.use(function (config) {
    // 在发送请求之前做些什么
    return config;
}, function (error) {
    // 对请求错误做些什么
    return Promise.reject(error);
});

// 添加响应拦截器
axios.interceptors.response.use(function (response) {
    // 对响应数据做点什么
    return response;
}, function (error) {
    // 对响应错误做点什么
    return Promise.reject(error);
});

```

### 与vuex使用
```

// 1.页面上发起请求
mounted: function () {
    let params = {
        cat: "",
        charset: "utf-8",
        length: "50",
        encode: "json",
        fun: "sync",
        source: "",
    }
    //调用actions里的方法请求数据
    this.$store.dispatch('getData', params);
}


// 2.vuex里请求

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
    getData (store, params) {
        axios.get(api.getData, {params:params}).then(data => {
            console.log(data)
            store.commit('settodo', data.data);
        }).catch((err) => {
            console.log(err);
        })
    }
}
```

For a detailed explanation on how things work, check out the guide [axios](https://www.kancloud.cn/yunye/axios/234845).