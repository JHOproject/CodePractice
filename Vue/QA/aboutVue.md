# 關於 MVVM
> ## 什麼是 MVVM
<回答1><br>
MVVM分為Model、View、ViewModel三者：
1. Model：資料模型，資料和業務邏輯都在 Model 層中定義。
2. View：UI 檢視，負責資料的展示。
3. ViewModel：負責監聽 Model 中資料的改變並且控制檢視的更新，處理使用者互動操作。
Model 和 View 並無直接關聯，而是通過 ViewModel 來進行聯絡，Model 和 ViewModel 之間有著雙向資料繫結的聯絡。因此當 Model 中的資料改變時會觸發 View 層的重新整理，View 中由於使用者互動操作而改變的資料也會在 Model 中同步。
這種模式實現了 Model 和 View 的資料自動同步，因此開發者只需要專注對資料的維護操作即可，而不需要自己操作 DOM。<br>

<回答2><br>
MVVM 是 Model-View-ViewModel 的縮寫。mvvm 是一種設計思想。Model 層代表數據模型，也可以在 Model 中定義數據修改和操作的業務邏輯；View 代表 UI 組件，它負責將數據模型轉化成 UI 展現出來，ViewModel 是一個同步 View 和 Model 的對象。<br>
在 MVVM 架構下，View 和 Model 之間並沒有直接的聯繫，而是通過 ViewModel 進行交互，Model 和 ViewModel 之間的交互是雙向的， 因此 View 數據的變化會同步到 Model 中，而 Model 數據的變化也會立即反應到 View 上。<br>
ViewModel 通過雙向數據綁定把 View 層和 Model 層連接了起來，而 View 和 Model 之間的同步工作完全是自動的，無需人為干涉，因此開發者只需關注業務邏輯，不需要手動操作 DOM, 不需要關注數據狀態的同步問題，複雜的數據狀態維護完全由 MVVM 來統一管理。
> ## 和 MVC、其它框架（jquery）的區別？哪些場景適合？
* mvc 和 mvvm 其實區別並不大。都是一種設計思想。主要就是 mvc 中 Controller 演變成 mvvm 中的 viewModel。mvvm 主要解決了 mvc 中大量的 DOM 操作使頁面渲染性能降低，加載速度變慢，影響用戶體驗。和當 Model 頻繁發生變化，開發者需要主動更新到 View 。

# 關於 Vue

> ## Vue 的優點
* 低耦合：視圖（View）可以獨立於 Model 變化和修改，一個 ViewModel 可以綁定到不同的”View”上，當 View 變化的時候 Model 可以不變，當 Model 變化的時候 View 也可以不變。
* 可重用性：你可以把一些視圖邏輯放在一個 ViewModel 裡面，讓很多 view 重用這段視圖邏輯。
* 獨立開發：開發人員可以專注於業務邏輯和數據的開發（ViewModel），設計人員可以專注於頁面設計，使用 Expression Blend 可以很容易設計界面並生成 xml 代碼。
* 可測試：界面素來是比較難於測試的，而現在測試可以針對 ViewModel 來寫。
> ## 描述 Vue 從「初始化頁面→修改資料重新整理頁面→UI」的過程
當 Vue 進入初始化階段時，一方面 Vue 會遍歷 data 中的屬性，並用 Object.defineProperty 將它轉化成 getter/setter 的形式，實現資料劫持(暫不談 Vue3.0 的 Proxy)；另一方面，Vue 的指令編譯器 Compiler 對元素節點的各個指令進行解析，初始化檢視，並訂閱 Watcher 來更新試圖，此時 Watcher 會將自己新增到訊息訂閱器 Dep 中，此時初始化完畢。
當資料發生變化時，觸發 Observer 中 setter 方法，立即呼叫 Dep.notify(),Dep 這個陣列開始遍歷所有的訂閱者，並呼叫其 update 方法，Vue 內部再通過 diff 演算法，patch 相應的更新完成對訂閱者檢視的改變。
> ## Vue 和 React 的簡單對比

> ## Vue 的 nextTick 原理為何




# 關於 Vue 的生命週期

> ## 什麼是 Vue 生命周期
Vue 實例從創建到銷毀的過程，就是生命周期。也就是從開始創建、初始化數據、編譯模板、掛載 DOM → 渲染、更新 → 渲染、卸載等一系列過程，我們稱這是 Vue 的生命周期。

> ## Vue 生命周期的作用是什麼
它的生命周期中有多個事件鉤子，讓我們在控制整個Vue實例的過程時更容易形成好的邏輯。
> ## Vue 生命周期總共有幾個階段
總共分為8個階段創建前/後，載入前/後，更新前/後，銷毀前/後。
* 創建前/後： 在 beforeCreate 階段，vue 實例的掛載元素 el 和數據對象 data 都為 undefined，還未初始化。在 created 階段， vue 實例的數據對象data 有了，el 還沒有。
* 載入前/後：在 beforeMount 階段，vue 實例的 $el 和 data 都初始化了，但還是掛載之前為虛擬的 DOM 節點，data.message 還未替換。在 mounted 階段，vue 實例掛載完成，data.message 成功渲染。
* 更新前/後：當 data 變化時，會觸發 beforeUpdate 和 updated 方法。
* 銷毀前/後：在執行 destroy 方法後，對 data 的改變不會再觸發周期函數，說明此時 vue 實例已經解除了事件監聽以及和 DOM 的綁定，但是 DOM 結構依然存在。
> ## 第一次頁面加載會觸發哪幾個鉤子
第一次頁面加載時會觸發 beforeCreate, created, beforeMount, mounted 這幾個鉤子。
> ## DOM 渲染在 哪個周期中就已經完成
DOM 渲染在 mounted 中已經完成。

> ## 簡單描述每個周期具體適合哪些場景

生命周期鉤子的一些使用方法：

* beforecreate : 可以在這加個 loading 事件，在加載實例時觸發。

* created : 初始化完成時的事件寫在這裡，如在這結束 loading 事件，異步請求也適宜在這裡調用。

* mounted : 掛載元素，獲取 DOM 節點。

* updated : 如果對數據統一處理，在這裡寫上相應函數。

* beforeDestroy : 可以做一個確認停止事件的確認框。

* nextTick : 更新數據後立即操作 DOM。


# 關於 Vue 的操作
> ## Vue 的響應式原理
當一個 Vue 實例建立時，Vue 會遍歷 data 選項的屬性，用 Object.defineProperty 將它們轉為 getter/setter 並且在內部追蹤相關依賴，在屬性被訪問和修改時通知變化。
每個元件例項都有相應的 watcher 程式例項，它會在元件渲染的過程中把屬性記錄為依賴，之後當依賴項的 setter 被呼叫時，會通知 watcher 重新計算，從而致使它關聯的元件得以更新。
<!-- TODO:什麼是例項? -->
> ## Vue 的雙向綁定原理
* 父元件通過 props 傳值給子元件，子元件通過 $emit 來通知父元件修改相應的props值。在父元件中做了兩件事，一是給子元件傳入props，二是監聽input事件並同步自己的value屬性。
* v-model（幫我們完成上面的兩步操作）。
> ## Vue 如何在元件內部實現一個雙向資料綁定

> ## Vue 元件間通訊的方式（組件間的傳值）

> ## Vue 如何實現按需加載配合webpack設置
> ## Vue 的指令及用法
v-html、v-show、v-if、v-for 等。
> ## 指令v-el的作用是什麼
> ## v-if 和 v-show 的區別
v-show 僅僅控制元素的顯示方式，將 display 屬性在 block 和 none 來回切換；而v-if會控制這個 DOM 節點的存在與否。當我們需要經常切換某個元素的顯示/隱藏時，使用 v-show 會更加節省效能；當只需要一次顯示或隱藏時，使用 v-if 更加合理。
> ## watch、methods 和 計算屬性的區別

> ## 為什麼使用key

> ## 為什麼避免 v-if 和 v-for 用在一起

> ## Vue 中如何監控某個屬性值的變化
* 通過 watch 實現：
```javascript
watch: {
      obj: {
      handler (newValue, oldValue) {
        console.log('obj changed')
      },
      // deep: true // deep屬性表示深層遍歷，會監控obj的所有屬性變化 // 並非現在想要的效果，所以註解
    }
  }
```
* 通過 computed 實現：利用計算屬性的特性來實現，當依賴改變時，便會重新計算一個新值
```javascript
computed: {
    a1 () {
      return this.obj.a
    }
}
```

> ## Vue中重置data的方法

> ## Vue中給data中的物件屬性新增一個新的屬性時會發生什麼，如何解決
以下示例可發現，點選 button 後，obj.b 成功新增，但檢視並未重新整理，原因在於在 Vue 實例建立時，obj.b 並未宣告，因此就沒有被 Vue 轉換為響應式的屬性，自然就不會觸發檢視的更新，這時就需要使用 Vue 的全域性 api $set()：
```javascript
// 示例
<template>
  <div>
    <ul>
      <li v-for="value in obj" :key="value">
        {{value}}
      </li>
    </ul>
    <button @click="addObjB">新增obj.b</button>
  </div>
</template>
<script>
export default {
  data () {
    return {
      obj: {
        a: 'obj.a'
      }
    }
  },
  methods: {
    addObjB () {
      this.obj.b = 'obj.b'
      console.log(this.obj)
    }
  }
}
</script>
<style></style>
```
```javascript
// 解法: $set()方法相當於手動的去把obj.b處理成一個響應式的屬性，此時檢視也會跟著改變
addObjB () {
      // this.obj.b = 'obj.b'
      this.$set(this.obj, 'b', 'obj.b')
      console.log(this.obj)
    }
```

> ## <keep-alive> </keep-alive>的作用是什麼

> ## active-class是哪個組件的屬性

> ## Vue中引入組件的步驟

> ## 在Vue中使用插件的步驟

> ## delete 和 Vue.delete 刪除陣列的區別
* delete：被刪除的元素變成 empty/undefined ，其他元素的鍵值仍然不變。
* Vue.delete：直接刪除陣列 改變陣列的鍵值。
```javascript
var a=[1,2,3,4]
    var b=[1,2,3,4]
    delete a[1]
    console.log(a)
    this.$delete(b,1)
    console.log(b)
```
> ## 元件中寫 name 選項的作用

> ## 如何讓CSS只在當前組件中起作用

> ## VNode是什麼？虛擬 DOM是什麼

> ## 動態綁定Class有幾種方式

# 關於 Vue-loader

> ## 什麼是 vue-loader

> ## 怎麼定義vue-router的動態路由以及如何獲取傳過來的動態參數

> ## vue-router 有哪幾種導航鉤子

> ## 請列舉出3個 Vue 中常用的生命周期鉤子函數

> ## vue-router 的鉤子函式

> ## 自定義指令（v-check, v-focus）的方法有哪些? 它有哪些鉤子函數? 還有哪些鉤子函數參數

> ## route 和 router 的區別

> ## 路由之間跳轉






# 關於 Vuex

> ## 什麼是 Vuex（Vuex 原理）

> ## Vuex 有幾種屬性

> ## Vuex 的 store 特性是什麼

> ## Vuex 的 getter 特性是什麼

> ## Vuex 的 mutation 特性是什麼

> ## 不用 Vuex 會帶來什麼問題

> ## Vuex 如何區分 state 是外部直接修改，還是通過 mutation 方法修改的

> ## Vue 中 ajax 請求代碼應該寫在組件的 methods 中還是 Vuex 的 action 中





# 關於 Vue-cli

> ## 什麼是 vue-cli

> ## scss是什麼？在vue.cli中的安裝使用步驟是？有哪幾大特性





# 關於其他前端問題

> ## 如何優化 SPA 應用的首屏載入速度慢的問題
* 公用的 JS 庫通過 script 標籤外部引入，減小 app.bundel 的大小，讓瀏覽器並行下載資原始檔，提高下載速度。
* 在配置路由時，頁面和元件使用懶載入的方式引入，進一步縮小 app.bundel 的體積，在呼叫某個元件時再載入對應的 js 檔案。
* 加一個首屏 loading 圖，提升使用者體驗
> ## 前端如何優化網站效能
* 減少 HTTP 請求數量：在瀏覽器與伺服器進行通訊時，主要是通過 HTTP 進行通訊。瀏覽器與伺服器需要經過三次握手，每次握手需要花費大量時間。而且不同瀏覽器對資原始檔併發請求數量有限（ 不同瀏覽器允許併發數 ），一旦 HTTP 請求數量達到一定數量，資源請求就存在等待狀態，這是很致命的，因此減少 HTTP 的請求數量可以很大程度上對網站效能進行優化。
    * CSS Sprites：國內俗稱 CSS 精靈，這是將多張圖片合併成一張圖片達到減少 HTTP 請求的一種解決方案，可以通過 CSS background 屬性來訪問圖片內容。這種方案同時還可以減少圖片總位元組數。
    * 合併 CSS 和 JS 檔案：現在前端有很多工程化打包工具，如：grunt、gulp、webpack等。為了減少 HTTP 請求數量，可以通過這些工具再發布前將多個 CSS 或者 多個 JS 合併成一個檔案。
    * 採用 lazyLoad：俗稱懶載入，可以控制網頁上的內容在一開始無需載入，不需要發請求，等到使用者操作真正需要的時候立即加載出內容。這樣就控制了網頁資源一次性請求數量。
* 控制資原始檔載入優先順序：瀏覽器在載入 HTML 內容時，是將 HTML 內容從上至下依次解析，解析到 link 或者 script 標籤就會載入 href 或者 src 對應連結內容，為了第一時間展示頁面給使用者，就需要將 CSS 提前載入，不要受 JS 載入影響。
一般情況下都是 CSS 在頭部，JS 在底部。
* 利用瀏覽器快取：瀏覽器快取是將網路資源儲存在本地，等待下次請求該資源時，如果資源已經存在就不需要到伺服器重新請求該資源，直接在本地讀取該資源。
* 減少重排（Reflow）：基本原理：重排是 DOM 的變化影響到了元素的幾何屬性（寬和高），瀏覽器會重新計算元素的幾何屬性，會使渲染樹中受到影響的部分失效，瀏覽器會驗證 DOM 樹上的所有其它結點的 visibility 屬性，這也是 Reflow 低效的原因。如果 Reflow 的過於頻繁，CPU 使用率就會急劇上升。
* 減少 Reflow，如果需要在 DOM 操作時新增樣式，儘量使用 增加 class 屬性，而不是通過 style 操作樣式。
    * 減少 DOM 操作
    * 圖示使用 IconFont 替換
> ## 網頁從輸入網址到渲染完成經歷了哪些過程
大致可以分為如下7步：
1. 輸入網址。
2. 傳送到 DNS 伺服器，並獲取域名對應的 web 伺服器對應的 IP 地址。
3. 與 web 伺服器建立 TCP 連線。
4. 瀏覽器向 web 伺服器傳送 http 請求。
5. web 伺服器響應請求，並返回指定 url 的資料（或錯誤資訊，或重定向的新的 url 地址）。
6. 瀏覽器下載 web 伺服器返回的資料及解析 html 原始檔。
7. 生成 DOM 樹，解析 css 和 js，渲染頁面，直至顯示完成。
> ## jQuery 獲取的 DOM 物件和原生的 DOM 物件有何區別
js 原生獲取的 DOM 是一個物件，jQuery 物件就是一個數組物件，其實就是選擇出來的元素的陣列集合，所以說他們兩者是不同的物件型別不等價。
* 原生DOM物件轉jQuery物件：
```javascript
var box = document.getElementById('box');
var $box = $(box);
```
* jQuery物件轉原生DOM物件：
```javascript
var $box = $('#box');
var box = $box[0];
```
> ## jQuery如何擴充套件自定義方法
```javascript
(jQuery.fn.myMethod=function () {
       alert('myMethod');
})
// 或者：
(function ($) {
        $.fn.extend({
             myMethod : function () {
                  alert('myMethod');
             }
        })
})(jQuery)
```
使用：
```javascript
$("#div").myMethod();
```
> ## axios 是什麼？怎麼使用？描述使用它實現登錄功能的流程

