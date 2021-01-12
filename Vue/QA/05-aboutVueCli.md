# 關於 Vue-cli

> ## 什麼是 vue-cli
 * 它是基於 Vue.js 進行快速開發的完整系統，也可以理解成是很多 npm 包的集合。

  * vue-cli 完成的功能

    * .vue 檔案 --> .js 檔案

    * ES6 語法 --> ES5 語法

    * Sass,Less,Stylus --> CSS

    * 對 jpg,png,font 等靜態資源的處理。

    * 熱更新。

    * 定義環境變數，區分 dev 和 production 模式。

  * 如果開發者需要補充或修改預設設定，需要在 package.json 同級下新建一個 vue.config.js 檔案。
> ## vue.cli 中怎樣使用自定義的組件
* 第一步
  
  在 components 目錄新建你的組件文件（indexPage.vue），`script` 一定要 `export default {}`。

* 第二步
  
  在需要用的頁面（組件）中導入：`import indexPage from '@/components/indexPage.vue'`。

* 第三步
  
  注入到 Vue 的子組件的 components 屬性上面，`components:{indexPage}`。

* 第四步
  
  在 template 視圖 view 中使用，例如有 indexPage 命名，使用的時候則 `index-page`。
> ## scss是什麼？在vue.cli中的安裝使用步驟是？有哪幾大特性
* scss 是 css 的預編譯。
* 使用步驟
  * 第一步
    
    先裝 css-loader、node-loader、sass-loader 等加載器模塊。

  * 第二步
    
    在 build 目錄找到 `webpack.base.config.js`，在那個extends 屬性中加一個拓展 `.scss`。
  * 第三步
  
    在同一個文件，配置一個 `module` 屬性。

  * 第四步
    
    然後在組件的 `style` 標籤加上 `lang` 屬性 ，例如：`lang=」scss」`。

    特性:

    * 可以用變量，例如 `$變量名稱=值`。

    * 可以用混合器。

    * 可以嵌套。
> ## 構建的 vue-cli 工程都到了哪些技術，它們的作用分別是什麼
> ## vue-cli 工程常用的 npm 命令有哪些

> ## 請說出 vue-cli 工程中每個資料夾和檔案的用處

> ## config資料夾下 `index.js` 的對於工程開發環境和生產環境的配置

> ## 請詳細介紹一些 package.json 裡面的配置

