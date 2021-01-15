# 關於 Vuex

> ## 什麼是 Vuex（Vuex 原理）
**<回答1>**

vue 框架中狀態管理。在 main.js 引入 store，注入。新建了一個目錄 store，….. export 。場景有：單頁應用中，組件之間的狀態。音樂播放、登錄狀態、加入購物車
```javascript
// 新建 store.js
import vue from 'vue'
import vuex form 'vuex'
vue.use(vuex)
export default new vuex.store({
//...code
})
//main.js
import store from './store'
...
```

**<回答2>**

vuex 僅僅是作為 vue 的一個插件而存在，不像 Redux , MobX 等庫可以應用於所有框架，vuex 只能使用在 vue 上，很大的程度是因為其高度依賴於 vue 的 computed 依賴檢測系統以及其插件系統，

vuex 整體思想誕生於 flux，可其的實現方式完完全全的使用了 vue 自身的響應式設計，依賴監聽、依賴收集都屬於 vue 對對象 Property set get 方法的代理劫持。最後一句話結束 vuex 工作原理，vuex 中的 store 本質就是沒有 template 的隱藏著的 vue 組件。
> ## Vuex 有幾種屬性
有 5 種，分別是 state、getter、mutation、action、module。
> ## Vuex 的 store 特性是什麼
* vuex 就是一個倉庫，倉庫裡放了很多對象。其中 state 就是數據源存放地，對應於一般 vue 對象裡面的 data。state 裡面存放的數據是響應式的，vue 組件從 store 讀取數據，若是 store 中的數據發生改變，依賴這相數據的組件也會發生更新。它通過 mapState 把全局的 state 和 getters 映射到當前組件的 computed 計算屬性。
> ## Vuex 的 getter 特性是什麼
getter 可以對 state 進行計算操作，它就是 store 的計算屬性。雖然在組件內也可以做計算屬性，但是 getters 可以在多給件之間複用。如果一個狀態只在一個組件內使用，是可以不用 getters。
> ## Vuex 的 mutation 特性是什麼
action 類似於 muation，不同在於：action 提交的是 mutation，而不是直接變更狀態。另外，action 可以包含任意異步操作。如果被其他地方複用，請將請求放入 action 裡，方便複用，並包裝成 promise 返回。
> ## 不用 Vuex 會帶來什麼問題
* 可維護性下降：要修改數據，得維護 3 個地方。
* 可讀性下降：因為一個組件裡的數據，無法看出來源。
* 增加耦合：大量的上傳派發，會讓耦合性大大的增加，在 Vue 使用 Component 就是為了減少耦合，此行為和組件化的初衷相背。
> ## Vuex 如何區分 state 是外部直接修改，還是通過 mutation 方法修改的
Vuex 中修改 state 的唯一渠道就是執行 commit(‘xx’, payload) 方法，其底層通過執行 this._withCommit(fn) 設置_committing 標誌變量為 true，然後才能修改 state，修改完畢還需要還原_committing 變量。外部修改雖然能夠直接修改 state，但是並沒有修改_committing 標誌位，所以只要 watch 一下 state，state change 時判斷是否_committing 值為 true，即可判斷修改的合法性。

> ## Vue 中 ajax 請求代碼應該寫在組件的 methods 中還是 Vuex 的 action 中
如果請求來的數據不是要被其他組件公用，僅僅在請求的組件內使用，就不需要放入 vuex 的 state 裡。

---
#
#### *內文來源*
* #### *[2019前端面試題彙總（主要為Vue）](https://www.mdeditor.tw/pl/2U6o/zh-tw"2019前端面試題彙總（主要為Vue）")*
* #### *[年底前端面試vue總結](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/756634/#outline__1"https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/756634/#outline__1")*
* #### *[2019年web前端Vue面試題總結](https://kknews.cc/zh-tw/code/4k29znq.html"2019年web前端Vue面試題總結")*
* #### *[2019前端面試系列——Vue面試題](https://www.itread01.com/content/1564416182.html"2019前端面試系列——Vue面試題")*
* #### *[81道經典Vue試題總結](https://www.itread01.com/content/1543596364.html"81道經典Vue試題總結")*