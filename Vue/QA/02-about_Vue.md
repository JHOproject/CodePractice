# 目錄
* 一、關於 Vue
  * 1-1. Vue 的優點
  * 1-2. 描述 Vue 從「初始化頁面→修改資料重新整理頁面→UI」的過程
  * 1-3. Vue 和 React 的簡單對比
  * 1-4. Vue 的 nextTick 原理為何
  * 1-5. VNode是什麼？虛擬 DOM 是什麼
* 二、關於 Vue 的生命週期
  * 2-1.什麼是 Vue 生命周期
  * 2-2.Vue 生命周期的作用是什麼
  * 2-3.Vue 生命周期總共有幾個階段
  * 2-4.請列舉出3個 Vue 中常用的生命周期鉤子函數
  * 2-5.第一次頁面加載會觸發哪幾個鉤子
  * 2-6.DOM 渲染在哪個周期中就已經完成
  * 2-7.簡單描述每個周期具體適合哪些場景
* 三、關於 Vue 的操作
  * 3-1. Vue 的響應式原理（Vue Reactivity）
  * 3-2. Vue 的雙向綁定原理 / Vue 如何在元件內部實現一個雙向資料綁定
  * 3-3. Vue 元件間通訊的方式（組件間的傳值）
  * 3-4. Vue 如何實現按需加載配合webpack設置
  * 3-5. Vue 的指令及用法
  * 3-6. 指令 v-el 的作用是什麼
  * 3-7. v-if 和 v-show 的區別
  * 3-8. watch、methods 和 計算屬性的區別
  * 3-9. 為什麼使用 key
  * 3-10. 為什麼避免 v-if 和 v-for 用在一起
  * 3-11. Vue 中如何監控某個屬性值的變化
  * 3-12. Vue 中重置data的方法
  * 3-13. Vue 中給 data 中的物件屬性新增一個新的屬性時會發生什麼，如何解決
  * 3-14. 自定義指令（v-check, v-focus）的方法有哪些？它有哪些鉤子函數？還有哪些鉤子函數參數
  * 3-15. `<keep-alive> </keep-alive>` 的作用是什麼
  * 3-16. active-class 是哪個組件的屬性
  * 3-17. Vue 中引入組件的步驟（在 Vue 中使用插件的步驟）
  * 3-18. delete 和 Vue.delete 刪除陣列的區別
  * 3-19. 元件中寫 name 選項的作用
  * 3-20. 如何讓CSS只在當前組件中起作用
  * 3-21. 動態綁定Class有幾種方式
  * 3-22. 如何優化 SPA 應用的首屏載入速度慢的問題

---


# 一、關於 Vue

> ## 1-1. Vue 的優點

* 低耦合：視圖（View）可以獨立於 Model 變化和修改，一個 ViewModel 可以綁定到不同的 View 上，當 View 變化的時候 Model 可以不變，當 Model 變化的時候 View 也可以不變。

* 可重用性：可以把一些視圖邏輯放在一個 ViewModel 裡面，讓很多 view 重用這段視圖邏輯。

* 獨立開發：開發人員可以專注於業務邏輯和數據的開發（ViewModel），設計人員可以專注於頁面設計，使用 Expression Blend 可以很容易設計界面並生成 XML 代碼。

* 可測試：界面素來是比較難於測試的，而現在測試可以針對 ViewModel 來寫。

> ## 1-2. 描述 Vue 從「初始化頁面→修改資料重新整理頁面→UI」的過程

當 Vue 進入初始化階段時，一方面 Vue 會遍歷 data 中的屬性，並用 `Object.defineProperty` 將它轉化成 getter / setter 的形式，實現資料劫持（暫不談 Vue3.0 的 Proxy）；另一方面，Vue 的指令編譯器 Compiler 對元素節點的各個指令進行解析，初始化檢視，並訂閱 Watcher 來更新試圖，此時 Watcher 會將自己新增到訊息訂閱器 Dep 中，此時初始化完畢。

當資料發生變化時，觸發 Observer 中 setter 方法，立即呼叫 `Dep.notify()`，Dep 這個陣列開始遍歷所有的訂閱者，並呼叫其 update 方法，Vue 內部再通過 diff 演算法，patch 相應的更新完成對訂閱者檢視的改變。

> ## 1-3. Vue 和 React 的簡單對比

* 監聽資料變化的實現原理不同
  
  * Vue 通過 getter/setter 以及一些函式的劫持，能精確快速的計算出 Virtual DOM 的差異。這是由於它在渲染過程中，會跟蹤每一個元件的依賴關係，不需要重新渲染整個元件樹。

  * React 預設是通過比較引用的方式進行的，如果不優化，每當應用的狀態被改變時，全部子元件都會重新渲染，可能導致大量不必要的 VDOM 的重新渲染。

  Vue 不需要特別的優化就能達到很好的效能，而對於 React 而言，需要通過 PureComponent / shouldComponentUpdate 這個生命週期方法來進行控制。如果你的應用中，互動複雜，需要處理大量的 UI 變化，那麼使用 Virtual DOM 是一個好主意。如果你更新元素並不頻繁，那麼 Virtual DOM 並不一定適用，效能很可能還不如直接操控 DOM。

  為什麼 React 不精確監聽資料變化呢？這是因為 Vue React 設計理念上的區別，Vue 使用的是可變資料，React 更強調資料的不可變。

* 資料流的不同

  * Vue 中預設支援雙向繫結，元件與 DOM 之間可以通過 v-model 雙向繫結。但是，父子元件之間，props 在 2.x 版本是單向資料流。

  * React 一直提倡的是單向資料流，他稱之為 `onChange` / `setState()` 模式。
  
  不過由於我們一般都會用 Vuex 以及 Redux 等單向資料流的狀態管理框架，因此很多時候我們感受不到這一點的區別了。

* 模板渲染方式的不同

  * 在表層上， 模板的語法不同：React 是通過 JSX 渲染模板，而 Vue 是通過一種拓展的 HTML 語法進行渲染。

  * 在深層上，模板的原理不同，這才是他們的本質區別：

    * React 是在元件 JS 程式碼中，通過原生 JS 實現模板中的常見語法，比如插值，條件，迴圈等，都是通過 JS 語法實現的。

    * Vue 是在和元件 JS 程式碼分離的單獨的模板中，通過指令來實現的，比如條件語句就需要 v-if 來實現。

    React 的做法更加純粹原生，而 Vue 的做法顯得有些獨特，會把 HTML 弄得很亂。
    
* 舉例說明 React 的好處

  react 中 render 函式是支援閉包特性的，所以我們 import 的元件在 render 中可以直接呼叫。但是在 Vue 中，由於模板中使用的資料都必須掛在 this 上進行一次中轉，所以我們 import 一個元件完了之後，還需要在 components 中再宣告下，這樣顯然是很奇怪但又不得不這樣的做法。

> ## 1-4. Vue 的 nextTick 原理為何
* 為什麼需要 nextTick

  Vue 是非同步修改 DOM 的並且不鼓勵開發者直接接觸 DOM，但有時候業務需要必須對資料更改--重新整理後的 DOM 做相應的處理，這時候就可以使用 `Vue.nextTick(callback)` 這個 API 了。

* 理解原理前的準備
  
  首先需要知道事件迴圈中 ***巨集任務*** 和 ***微任務*** 這兩個概念（這其實也是面試常考點）。

  * 常見的巨集任務： `script`, `setTimeout`, `setInterval`, `setImmediate`, `I/O`, `UI rendering`。
  * 常見的微任務： `process.nextTick(Nodejs)`, `Promise.then()`, `MutationObserver`。

* 理解 nextTick

  而 nextTick 的原理正是 vue 通過非同步佇列控制 DOM 更新和 nextTick 回撥函式先後執行的方式。如果大家看過這部分的原始碼，會發現其中做了很多 `isNative()` 的判斷，因為這裡還存在相容性優雅降級的問題。可見 Vue 開發團隊的深思熟慮，對效能的良苦用心。

> ## 1-5. VNode是什麼？虛擬 DOM 是什麼
Vue 在 頁面上渲染的節點，及其子節點稱為「虛擬節點 （Virtual Node）」，簡寫為「VNode」。「虛擬 DOM」是由 Vue 組件樹建立起來的整個 VNode 樹的稱呼。

---

# 二、關於 Vue 的生命週期

> ## 2-1. 什麼是 Vue 生命周期
Vue 實例從創建到銷毀的過程，就是生命周期。也就是從開始創建、初始化數據、編譯模板、掛載 DOM → 渲染、更新 → 渲染、卸載等一系列過程，我們稱這是 Vue 的生命周期。

> ## 2-2. Vue 生命周期的作用是什麼
它的生命周期中有多個事件鉤子，讓我們在控制整個Vue實例的過程時更容易形成好的邏輯。

> ## 2-3. Vue 生命周期總共有幾個階段

#### **<回答1>**

* beforeCreate 和 created

* beforeMount 和 mounted

* beforeUpdate 和 updated

* beforeDestory 和 destoryed

* activated 和 deactivated

#### **<回答2>**

總共分為8個階段創建前/後，載入前/後，更新前/後，銷毀前/後。

* 創建前/後： 在 beforeCreate 階段，vue 實例的掛載元素 el 和數據對象 data 都為 undefined，還未初始化。在 created 階段， vue 實例的數據對象data 有了，el 還沒有。

* 載入前/後：在 beforeMount 階段，vue 實例的 $el 和 data 都初始化了，但還是掛載之前為虛擬的 DOM 節點，data.message 還未替換。在 mounted 階段，vue 實例掛載完成，data.message 成功渲染。

* 更新前/後：當 data 變化時，會觸發 beforeUpdate 和 updated 方法。

* 銷毀前/後：在執行 destroy 方法後，對 data 的改變不會再觸發周期函數，說明此時 vue 實例已經解除了事件監聽以及和 DOM 的綁定，但是 DOM 結構依然存在。

> ## 2-4. 請列舉出3個 Vue 中常用的生命周期鉤子函數
* created：實例已經創建完成之後調用，在這一步，實例已經完成數據觀測，屬性和方法的運算，watch / event 事件回調。然而，掛載階段還沒有開始， `$el` 屬性目前還不可見

* mounted：el 被新創建的 `vm.$el` 替換，並掛載到實例上去之後調用該鉤子。如果 root 實例掛載了一個文檔內元素，當 mounted 被調用時 `vm.$el` 也在文檔內。

* activated：keep-alive 組件激活時調用。

> ## 2-5. 第一次頁面加載會觸發哪幾個鉤子
第一次頁面加載時會觸發 beforeCreate, created, beforeMount, mounted 這幾個鉤子。

> ## 2-6. DOM 渲染在哪個周期中就已經完成
DOM 渲染在 mounted 中已經完成。

> ## 2-7. 簡單描述每個周期具體適合哪些場景

生命周期鉤子的一些使用方法：

* beforecreate : 可以在這加個 loading 事件，在加載實例時觸發。

* created : 初始化完成時的事件寫在這裡，如在這結束 loading 事件，異步請求也適宜在這裡調用。

* mounted : 掛載元素，獲取 DOM 節點。

* updated : 如果對數據統一處理，在這裡寫上相應函數。

* beforeDestroy : 可以做一個確認停止事件的確認框。

* nextTick : 更新數據後立即操作 DOM。


---

# 三、關於 Vue 的操作

> ## 3-1. Vue 的響應式原理（Vue Reactivity）
當一個 Vue 實例建立時，Vue 會遍歷 data 選項的屬性，用 Object.defineProperty 將它們轉為 getter/setter 並且在內部追蹤相關依賴，在屬性被訪問和修改時通知變化。
每個元件例項都有相應的 watcher 程式例項，它會在元件渲染的過程中把屬性記錄為依賴，之後當依賴項的 setter 被呼叫時，會通知 watcher 重新計算，從而致使它關聯的元件得以更新。
<!-- TODO:什麼是例項? -->

> ## 3-2. Vue 的雙向綁定原理
#### **<回答1>**
* 父元件通過 props 傳值給子元件，子元件通過 $emit 來通知父元件修改相應的props值。在父元件中做了兩件事，一是給子元件傳入props，二是監聽input事件並同步自己的value屬性。
* v-model（幫我們完成上面的兩步操作）。

#### **<回答2>**

vue.js 是採用數據劫持結合發佈者 - 訂閱者模式的方式，通過 `Object.defineProperty()` 來劫持各個屬性的 setter，getter，在數據變動時發佈消息給訂閱者，觸發相應的監聽回調。

* 幾個要點
  
  * 實現一個數據監聽器 Observer，能夠對資料物件的所有屬性進行監聽，如有變動可拿到最新值並通知訂閱者。
  
  * 實現一個指令解析器 Compile，對每個元素節點的指令進行掃描和解析，根據指令模板替換資料，以及繫結相應的更新函式。

  * 實現一個 Watcher，作為連線 Observer 和 Compile 的橋樑，能夠訂閱並收到每個屬性變動的通知，執行指令繫結的相應回撥函式，從而更新檢視。
  * mvvm 入口函式，整合以上三者。

* 具體步驟
  
  * 第一步
    
    需要 observe 的數據對象進行遞歸遍歷，包括子屬性對象的屬性，都加上 setter 和 getter 這樣的話，給這個對象的某個值賦值，就會觸發 setter，那麼就能監聽到了數據變化。

  * 第二步
    
    compile 解析模板指令，將模板中的變量替換成數據，然後初始化渲染頁面視圖，並將每個指令對應的節點綁定更新函數，添加監聽數據的訂閱者，一旦數據有變動，收到通知，更新視圖。

  * 第三步
    
    Watcher 訂閱者是 Observer 和 Compile 之間通信的橋樑，主要做的事情是：
    
    * 在自身實例化時往屬性訂閱器（dep）裡面添加自己。
    * 自身必須有一個 update() 方法。
    * 待屬性變動 dep.notice() 通知時，能調用自身的 update() 方法，並觸發 Compile 中綁定的回調，則功成身退。

  * 第四步
    
    MVVM 作為數據綁定的入口，整合 Observer、Compile 和 Watcher 三者，通過 Observer 來監聽自己的 model 數據變化，通過 Compile 來解析編譯模板指令，最終利用 Watcher 搭起 Observer 和 Compile 之間的通信橋樑，達到數據變化 -> 視圖更新；視圖交互變化(input) -> 數據 model 變更的雙向綁定效果。


> ## 3-3. Vue 元件間通訊的方式（組件間的傳值）

#### **<回答1>**

* 父組件與子組件傳值

  * 父組件通過標籤上面定義傳值

  * 子組件通過props方法接受數據

* 子組件向父組件傳遞數據

  * 子組件通過$emit方法傳遞參數

#### **<回答2>**

Vue 元件間 6 種通訊方式
* props / $emit
* $emit / $on
* vuex
* $attrs / $listeners
* provide / inject
* $parent / $children 與 ref

> ## 3-4. Vue 如何實現按需加載配合webpack設置
webpack 中提供了 `require.ensure()` 來實現按需加載。以前引入路由是通過 `import` 這樣的方式引入，改為 `const` 定義的方式進行引入。

* 不進行頁面按需加載引入方式：
  
  `import home from '../../common/home.vue'`

* 進行頁面按需加載的引入方式
  
  `const home = r => require.ensure( [], () => r (require('../../common/home.vue')))`

> ## 3-5. Vue 的指令及用法
* v-if：判斷是否隱藏。
* v-for：數據循環出來。
* v-bind:class：綁定一個屬性。
* v-model：實現雙向綁定。
* v-html
* v-show

> ## 3-6. 指令 v-el 的作用是什麼
提供一個在頁面上已存在的 DOM 元素作為 Vue 實例的掛載目標，可以是 CSS 選擇器，也可以是一個 HTMLElement 實例。

> ## 3-7. v-if 和 v-show 的區別
#### **<回答1>**

v-show 僅僅控制元素的顯示方式，將 display 屬性在 block 和 none 來回切換；而v-if會控制這個 DOM 節點的存在與否。當我們需要經常切換某個元素的顯示/隱藏時，使用 v-show 會更加節省效能；當只需要一次顯示或隱藏時，使用 v-if 更加合理。

#### **<回答2>**

v-show 指令是通過修改元素的 display CSS 屬性讓其顯示或者隱藏；v-if 指令是直接銷燬和重建 DOM 達到讓元素顯示和隱藏的效果。

> ## 3-8. watch、methods 和 計算屬性的區別
watch 為了監聽某個響應資料的變化。計算屬性是自動監聽依賴值的變化，從而動態返回內容，主要目的是簡化模板內的複雜運算。所以區別來源於用法，只是需要動態值，那就用計算屬性；需要知道值的改變後執行業務邏輯，才用 watch。

methods是一個方法，它可以接受引數，而computed 不能，computed 是可以快取的，methods 不會。computed 可以依賴其他 computed，甚至是其他元件的 data。

> ## 3-9. 為什麼使用 key
當有相同標籤名的元素切換時，需要通過 key 特性設置唯一的值來標記以讓 Vue 區分它們，否則 Vue 為了效率只會替換相同標籤內部的內容。

> ## 3-10. 為什麼避免 v-if 和 v-for 用在一起
當 Vue 處理指令時，v-for 比 v-if 具有更高的優先級，通過 v-if 移動到容器元素，不會再重複遍歷列表中的每個值。取而代之的是，我們只檢查它一次，且不會在 v-if 為否的時候運算 v-for。

> ## 3-11. Vue 中如何監控某個屬性值的變化
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


> ## 3-12. Vue 中重置data的方法
使用 `Object.assign()`， `vm.$data` 可以獲取當前狀態下的 data， `vm.$options.data` 可以獲取到元件初始化狀態下的 data。
```javascript
Object.assign(this.$data, this.$options.data())
```

> ## 3-13. Vue 中給 data 中的物件屬性新增一個新的屬性時會發生什麼，如何解決
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

> ## 3-14. 自定義指令（v-check, v-focus）的方法有哪些？它有哪些鉤子函數？還有哪些鉤子函數參數
* 全局定義指令：在 vue 對象的 directive 方法裡面有兩個參數, 一個是指令名稱, 另一個是函數。
* 組件內定義指令：directives。
* 鉤子函數: bind(綁定事件出發)、inserted(節點插入時候觸發)、update(組件內相關更新)。
* 鉤子函數參數： el、binding。

> ## 3-15. `<keep-alive> </keep-alive>` 的作用是什麼
包裹動態組件時，會緩存不活動的組件實例，主要用於保留組件狀態或避免重新渲染。

> ## 3-16. active-class 是哪個組件的屬性
vue-router 模塊的 router-link 組件。

> ## 3-17. Vue 中引入組件的步驟（在 Vue 中使用插件的步驟）
1. 採用 ES6 的 `import ... from ...` 語法或 CommonJS 的 `require()` 方法引入組件。

2. 對組件進行註冊，代碼如下

    `Vue.component('my-component', { template:'A custom component!'})`

3. 使用組件 `<my-component> </my-component>`

> ## 3-18. delete 和 Vue.delete 刪除陣列的區別
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

> ## 3-19. 元件中寫 name 選項的作用
* 專案使用 keep-alive 時，可搭配元件 name 進行快取過濾。

* DOM 做遞迴元件時需要呼叫自身 name。

* vue-devtools 除錯工具裡顯示的組見名稱是由 Vue 中元件 name 決定的。

> ## 3-20. 如何讓CSS只在當前組件中起作用
將當前組件的 style 修改為 style scoped。



> ## 3-21. 動態綁定Class有幾種方式

> ## 3-22. 如何優化 SPA 應用的首屏載入速度慢的問題
* 公用的 JS 庫通過 script 標籤外部引入，減小 app.bundel 的大小，讓瀏覽器並行下載資原始檔，提高下載速度。
* 在配置路由時，頁面和元件使用懶載入的方式引入，進一步縮小 app.bundel 的體積，在呼叫某個元件時再載入對應的 js 檔案。
* 加一個首屏 loading 圖，提升使用者體驗

---

#
#### *內文來源*
* #### *[2019前端面試題彙總（主要為Vue）](https://www.mdeditor.tw/pl/2U6o/zh-tw"2019前端面試題彙總（主要為Vue）")*
* #### *[年底前端面試vue總結](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/756634/#outline__1"https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/756634/#outline__1")*
* #### *[2019年web前端Vue面試題總結](https://kknews.cc/zh-tw/code/4k29znq.html"2019年web前端Vue面試題總結")*
* #### *[2019前端面試系列——Vue面試題](https://www.itread01.com/content/1564416182.html "2019前端面試系列——Vue面試題")*
* #### *[81道經典Vue試題總結](https://www.itread01.com/content/1543596364.html "81道經典Vue試題總結")*
* #### *[程式設計師應該知道的MVC，MVP，MVVM設計模式區別及優缺點](https://kknews.cc/zh-tw/news/8qzkmog.html "程式設計師應該知道的MVC，MVP，MVVM設計模式區別及優缺點")*
* #### *[MVVM架構](https://ithelp.ithome.com.tw/articles/10192829 "MVVM架構")*
* #### *[Vue Reactivity 響應式原理](https://chiafangsung.medium.com/vue-reactivity-%E9%9F%BF%E6%87%89%E5%BC%8F%E5%8E%9F%E7%90%86-ef4d2a14f908 "Vue Reactivity 響應式原理")*
* #### *[常見的實現MVVM方式：發布者-訂閱者模式，髒值檢查，數據劫持](https://kknews.cc/zh-tw/code/69r8qlv.html "常見的實現MVVM方式：發布者-訂閱者模式，髒值檢查，數據劫持")*
* #### *[一篇文章徹底弄懂Android-MVVM](https://www.mdeditor.tw/pl/ptbs/zh-tw "一篇文章徹底弄懂Android-MVVM")*
* #### *[Day 5 淺談 Android 設計架構 MVC、MVP、MVVM](https://ithelp.ithome.com.tw/articles/10218263 "Day 5 淺談 Android 設計架構 MVC、MVP、MVVM")*
* #### *[前端面試題：這是我理解的MVVM，請注意查收](https://codertw.com/%E7%A8%8B%E5%BC%8F%E8%AA%9E%E8%A8%80/693368/ "前端面試題：這是我理解的MVVM，請注意查收")*