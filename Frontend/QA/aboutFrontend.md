# 關於其他前端問題


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
示例：
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
axios 是請求後臺資源的模塊。 npm i axios -S。如果發送的是跨域請求，需在配置文件中 config/index.js 進行配置。
