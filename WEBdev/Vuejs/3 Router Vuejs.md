[/toc/] 
{{toc}} 
[[__TOC__]] 
[toc]


https://viblo.asia/p/tim-hieu-ve-vue-router-gAm5ybvwKdb

### Vue Router là gì
Khi click vào link trong Vue Single Page Application chúng ta không gởi bất kì một request html mới nào. Thay vào đó Vue sẽ can thiệp vào request đó. Vue sẽ đọc link (route) mà chúng ta click vào và render component tương ứng với route đó
## Cài đặt Vue Router

```bash
cd my-project
npm install vue-router --save
```

sẽ xuất hiện folder **src/router** trong đó có file **index.js** 

## Set up routes

### router/index.js
ta thấy array routes:
src/router/index.js
```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
import About from './components/About.vue'
import Home from './components/HelloWorld.vue'

Vue.use(VueRouter)

export const router = new VueRouter({
    routes: [
        {path: '/', name:'Home', component: Home},
        {path: '/about', name:'About', component: About}
    ]
})
...
```

Khởi tạo router instance:
```javascript
const router = createRouter({
  history: createWebHistory(process.env.BASE_URL),
  routes
})

export default router
```

``` history: createWebHistory(process.env.BASE_URL)```  sử dụng Web History api trong Browser để ta có thể click Forward và Backward `process.env.BASE_URL` là base-url của project
### src/main.js
Sau đó router được import vào file src/main.js và mount lên #app
```javascript
import router from './router'
axios.defaults.baseURL = "http://127.0.0.1:8000"
createApp(App).use(store).use(router, axios).mount('#app')
```
ta không cần khai báo tên của file index.js trong `import store from ./router` vì file tên index.js sẽ tự động được import

### router-view trong App.vue

```javascript
<section class="section">
  <router-link to="/about">About</router-link>
  <router-view/>
</section>
```

`<router-link> </router-link>` Là nơi ta đặt link, y hệt <a> tag
Tag `<router-view/>` là nơi route component được dymically injected 