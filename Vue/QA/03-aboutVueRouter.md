# 關於 Vue-router
> ## 怎麼定義vue-router的動態路由以及如何獲取傳過來的動態參數
* 在router目錄下的 `index.js` 文件中，對 path 屬性加上 `/:id`。
* 使用 router 對象的 `params.id`。
> ## vue-router 有哪幾種導航鉤子
以下三種：
* 全局導航鉤子
  ```javascript
  router.beforeEach(to, from, next),
  router.beforeResolve(to, from, next),
  router.afterEach(to, from )
  ```
* 組件內鉤子
  ```javascript
  beforeRouteEnter,
  beforeRouteUpdate,
  beforeRouteLeave
  ```
* 單獨路由獨享組件
  ```javascript
  beforeEnter
  ```
> ## vue-router 的鉤子函式
（官方文件：vue-router鉤子函式）
* 全域性前置守衛 router.beforeEach
* 全域性解析守衛 router.beforeResolve
* 全域性後置鉤子 router.afterEach
* 路由獨享的守衛 beforeEnter
* 元件內的守衛 beforeRouteEnter、beforeRouteUpdate、beforeRouteLeave
> ## route 和 router 的區別
route 是「路由資訊物件」，包括 path、params、hash、query、fullPath、matched、name 等路由資訊引數。
router 是「路由例項物件」，包括了路由的跳轉方法（push、replace），鉤子函式等。
> ## 路由之間跳轉
* 聲明式（標籤跳轉）
* 編程式（ js 跳轉） router.push(‘index’)
* 懶加載（按需加載路由）（常考）

  webpack 中提供了 require.ensure()來實現按需加載。以前引入路由是通過 import 這樣的方式引入，改為 const 定義的方式進行引入。
* 不進行頁面按需加載引入方式：
  ```javascript
  import  home   from '../../common/home.vue'
  ```
* 進行頁面按需加載的引入方式：
  ```javascript
  const  home = r => require.ensure( [], () => r (require('../../common/home.vue')))
  ```

> ## 嵌套路由怎麼定義
在實際項目中我們會碰到多層嵌套的組件組合而成，但是我們如何實現嵌套路由呢？因此我們需要在 VueRouter 的參數中使用 children 配置，這樣就可以很好的實現路由嵌套。
* index.html，只有一個路由出口。
  ```javascript
  <div id="app">
  <!-- router-view 路由出口, 路由匹配到的組件將渲染在這裡 -->
  <router-view></router-view>
  </div>
  ```
* main.js，路由的重定向，就會在頁面一加載的時候，就會將 home 組件顯示出來，因為重定向指向了 home 組件，redirect 的指向與 path 的必須一致。children 裡面是子路由，當然子路由裡面還可以繼續嵌套子路由。
  ```javascript
  import Vue from 'vue'
  import VueRouter from 'vue-router'
  Vue.use(VueRouter)
  //引入兩個組件
  import home from "./home.vue"
  import game from "./game.vue"
  //定義路由
  const routes = [
  { path: "/", redirect: "/home" },//重定向,指向了home組件
  {
  path: "/home", component: home,
  children: [
  { path: "/home/game", component: game }
  ]
  }
  ]
  //創建路由實例
  const router = new VueRouter({routes})
  new Vue({
  el: '#app',
  data: {
  },
  methods: {
  },
  router
  })
  //home.vue，點擊顯示就會將子路由顯示在出來，子路由的出口必須在父路由裡面，否則子路由無法顯示。
  ```

---

#### *內文來源*
* #### *[2019前端面試題彙總（主要為Vue）](https://www.mdeditor.tw/pl/2U6o/zh-tw"2019前端面試題彙總（主要為Vue）")*
* #### *[年底前端面試vue總結](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/756634/#outline__1"https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/756634/#outline__1")*
* #### *[2019年web前端Vue面試題總結](https://kknews.cc/zh-tw/code/4k29znq.html"2019年web前端Vue面試題總結")*
* #### *[2019前端面試系列——Vue面試題](https://www.itread01.com/content/1564416182.html"2019前端面試系列——Vue面試題")*
* #### *[81道經典Vue試題總結](https://www.itread01.com/content/1543596364.html"81道經典Vue試題總結")*