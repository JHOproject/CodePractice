# 關於 MVVM
> ## 什麼是 MVVM
MVVM分為Model、View、ViewModel三者：
1. Model：資料模型，資料和業務邏輯都在 Model 層中定義。
2. View：UI 檢視，負責資料的展示。
3. ViewModel：負責監聽 Model 中資料的改變並且控制檢視的更新，處理使用者互動操作。

Model 和 View 並無直接關聯，而是通過 ViewModel 來進行聯絡，Model 和 ViewModel 之間有著雙向資料繫結的聯絡。因此當 Model 中的資料改變時會觸發 View 層的重新整理，View 中由於使用者互動操作而改變的資料也會在 Model 中同步。
這種模式實現了 Model 和 View 的資料自動同步，因此開發者只需要專注對資料的維護操作即可，而不需要自己操作 dom。

> ## 和 MVC、其它框架（jquery）的區別？哪些場景適合？



# 關於 Vue

* ## Vue 的優點

* ## 描述 Vue 從「初始化頁面→修改資料重新整理頁面→UI」的過程

* ## Vue 和 React 的簡單對比

* ## Vue 的 nextTick 原理為何




# 關於 Vue 的生命週期




# 關於 Vue 的操作
* ## Vue 的指令及用法
v-html、v-show、v-if、v-for 等。
* ## 指令v-el的作用是什麼

* ## Vue 雙向綁定(響應式)原理

    * ## Vue 如何在元件內部實現一個雙向資料綁定

* ## Vue 元件間通訊的方式(組件間的傳值)

* ## Vue 如何實現按需加載配合webpack設置

* ## v-if 和 v-show 的區別

* ## watch、methods 和 計算屬性的區別

* ## 為什麼使用key

* ## 為什麼避免 v-if 和 v-for 用在一起

* ## Vue 中如何監控某個屬性值的變化

* ## Vue中重置data的方法

* ## Vue中給data中的物件屬性新增一個新的屬性時會發生什麼，如何解決

* ## <keep-alive> </keep-alive>的作用是什麼

* ## active-class是哪個組件的屬性

* ## Vue中引入組件的步驟

* ## 在Vue中使用插件的步驟

* ## delete 和 Vue.delete 刪除陣列的區別

* ## 元件中寫 name 選項的作用

* ## 如何讓CSS只在當前組件中起作用

* ## VNode是什麼？虛擬 DOM是什麼

* ## 動態綁定Class有幾種方式

# 關於 Vue-loader

* ## 什麼是 vue-loader

* ## 怎麼定義vue-router的動態路由以及如何獲取傳過來的動態參數

* ## vue-router 有哪幾種導航鉤子

* ## 請列舉出3個 Vue 中常用的生命周期鉤子函數

* ## vue-router 的鉤子函式

* ## 自定義指令(v-check, v-focus) 的方法有哪些? 它有哪些鉤子函數? 還有哪些鉤子函數參數

* ## route 和 router 的區別

* ## 路由之間跳轉






# 關於 Vuex

* ## 什麼是 Vuex(Vuex 原理)

* ## Vuex 有幾種屬性

* ## Vuex 的 store 特性是什麼

* ## Vuex 的 getter 特性是什麼

* ## Vuex 的 mutation 特性是什麼

* ## 不用 Vuex 會帶來什麼問題

* ## Vuex 如何區分 state 是外部直接修改，還是通過 mutation 方法修改的

* ## Vue 中 ajax 請求代碼應該寫在組件的 methods 中還是 Vuex 的 action 中





# 關於 Vue-cli

* ## 什麼是 vue-cli

* ## scss是什麼？在vue.cli中的安裝使用步驟是？有哪幾大特性





# 關於其他前端問題

## 如何優化SPA應用的首屏載入速度慢的問題

## 前端如何優化網站效能

## 網頁從輸入網址到渲染完成經歷了哪些過程

## jQuery 獲取的 dom 物件和原生的 dom 物件有何區別

## axios 是什麼？怎麼使用？描述使用它實現登錄功能的流程

