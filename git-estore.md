# FETNET ESTORE

## Summary
- [FETNET ESTORE](#fetnet-estore)
  - [Summary](#summary)
  - [壹、專案介紹](#壹專案介紹)
    - [一、Estore 開發簡述](#一estore-開發簡述)
    - [二、Git 位置及使用說明](#二git-位置及使用說明)
        - [Git 位置](#git-位置)
        - [下載步驟](#下載步驟)
  - [貳、React 架構說明](#貳react-架構說明)
  - [參、SASS 樣式設定](#參sass-樣式設定)
    - [一、瀏覽器斷點](#一瀏覽器斷點)
    - [二、基礎定義及修改](#二基礎定義及修改)
    - [三、修改及區塊設定方式](#三修改及區塊設定方式)
    - [四、使用樣式工具](#四使用樣式工具)
  - [肆、切版開發說明](#肆切版開發說明)
    - [一、HTML 大架構說明](#一html-大架構說明)
    - [二、常用套件及共用工具](#二常用套件及共用工具)
        - [Material-UI](#material-ui)
        - [PropTypes](#proptypes)
        - [ReactSlick](#reactslick)
        - [FormsyReact](#formsyreact)
        - [Paginate](#paginate)
        - [ScrollMemory](#scrollmemory)
        - [Utils](#utils)
  - [伍、Router 及全域設定](#伍router-及全域設定)
    - [一、App.js](#一appjs)
    - [二、Loader.js](#二loaderjs)
  - [陸、Redux 使用方式](#陸redux-使用方式)
    - [一、State](#一state)
    - [二、Reducer](#二reducer)
    - [三、Action/action/index.js 設定從模組呼叫 redux 的函式。](#三actionactionindexjs-設定從模組呼叫-redux-的函式)
    - [四、Componentonent 中帶入對應的 action 與 redux 函式。](#四componentonent-中帶入對應的-action-與-redux-函式)
  - [柒、模組與流程](#柒模組與流程)
    - [一、基本模組ments/button.md)](#一基本模組mentsbuttonmd)
    - [二、表單模組](#二表單模組)
    - [三、主要共用模組](#三主要共用模組)
    - [四、頁面流程](#四頁面流程)

---

## 壹、專案介紹

### 一、Estore 開發簡述

遠傳 Estore 專案是使用 React 開發，並經過 EBU、CBU、Eservice 的開發後，延續一貫的開發邏輯再擴展而成，為了切版與開發不相互影響，因此切版與開發分屬於不同 Git 做維護，本文件只說明切版時的檔案結構與注意事項。在開發時需注意與 IT 串接端如何同步。

本文件不特別說明 React 、JS、CSS 等基礎使用方式，僅針對開發所使用的架構邏輯做說明，如有闕漏可再聯絡撰寫人補充。


### 二、Git 位置及使用說明


##### Git 位置

[https://github.com/32017/fetnet-estore-ui.git](https://github.com/32017/fetnet-estore-ui.git)


##### 下載步驟

1. Git 由遠傳的 Roger 管理，在使用前需請 Roger 加入權限
2. Git clone 專案到本機的專案資料夾
3. 在 terminal 輸入  `cd /fentet-estore-ui ` 進入專案資料夾
4. 執行 ` npm install `  安裝 npm 開發工具
5. 執行 ` npm run start ` 進行開發與除錯
6. 需打包時使用 ` npm run build ` 打包開發檔案，並將 build 資料夾內的內容上傳測試環境

---

## 貳、React 架構說明

在此條列與開發有直接相關的檔案與資料夾做說明：


<table>
  <tr>
   <td colspan="2" ><strong>public</strong>
   </td>
  </tr>
  <tr>
   <td><strong>路徑</strong>
   </td>
   <td><strong>說明</strong>
   </td>
  </tr>
  <tr>
   <td>/resources
   </td>
   <td>放切版時會用到的圖檔，共用圖檔放在 common/images 中，其餘的圖檔根據頁面或功能分資料夾存取。
   </td>
  </tr>
  <tr>
   <td>.htaccess
   </td>
   <td>放到測試環境與 server 時務必上傳 .htaccess 處理網址對應目錄。
   </td>
  </tr>
</table>

<table>
  <tr>
   <td colspan="3" ><strong>src</strong>
   </td>
  </tr>
  <tr>
   <td colspan="2" ><strong>路徑</strong>
   </td>
   <td><strong>說明</strong>
   </td>
  </tr>
  <tr>
   <td rowspan="2" >/component
   </td>
   <td>/
   </td>
   <td><code>根據功能屬性放置對應的模組，無法被分類的則直接放在第一層</code>
   </td>
  </tr>
  <tr>
   <td>/partials
   </td>
   <td>屬於一個或多個模組（component）合成的區塊（section），如同 compoennt 根據功能做分類
   </td>
  </tr>
  <tr>
   <td>/mock
   </td>
   <td>
   </td>
   <td>切版時放置模擬用資料。
   </td>
  </tr>
  <tr>
   <td>/pages
   </td>
   <td>
   </td>
   <td>pages 當中的檔案可對應到 App.js 的 router 中，因此在尋找畫面時可透過 <strong>url →  router → page → component</strong>  逐層尋找。
   </td>
  </tr>
  <tr>
   <td>/sass
   </td>
   <td>
   </td>
   <td>sass 資料結構主要為參考 Sass Guidelines 做開發，詳細內容在後面章節說明。
   </td>
  </tr>
  <tr>
   <td>/service
   </td>
   <td>
   </td>
   <td>API 串接用，切版時用不到；但可以在此引入 mock data 模擬 API 取得資料的狀況
   </td>
  </tr>
  <tr>
   <td>/stores
   </td>
   <td>
   </td>
   <td><code>Redux 紀錄全域設定內容</code>
   </td>
  </tr>
  <tr>
   <td>/App.js
   </td>
   <td>
   </td>
   <td>切版的頁面與入口
   </td>
  </tr>
</table>

---

## 參、SASS 樣式設定

顏色設定與一些詳盡的使用方式可參考 Gitbook，Gitbook 連結：https://app.gitbook.com/@ajacreative/s/fetnet-1/


### 一、瀏覽器斷點

React 主要使用 Material-UI 的網格系統，參考其主要的瀏覽器尺寸斷點，實際開發時可再根據實際狀況或裝置需求增加，以下列出目前使用的斷點：


<table>
  <tr>
   <td>
   </td>
   <td><strong>Breakpoint</strong>
   </td>
   <td><strong>補充 breakpoint 設定</strong>
   </td>
   <td><strong>說明</strong>
   </td>
  </tr>
  <tr>
   <td>
   </td>
   <td>
   </td>
   <td><= 374px or <= 480
   </td>
   <td>針對手機額外設定
   </td>
  </tr>
  <tr>
   <td><strong>sm</strong>
   </td>
   <td>>= 600px or 
   </td>
   <td>>= 768 and <= 1024
   </td>
   <td>針對 iPad 尺寸額外設定
   </td>
  </tr>
  <tr>
   <td><strong>md</strong>
   </td>
   <td>>= 960px 
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td><strong>lg</strong>
   </td>
   <td>>= 1200px
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td><strong>xl</strong>
   </td>
   <td>>= 1920px
   </td>
   <td>
   </td>
   <td>
   </td>
  </tr>
</table>



### 二、基礎定義及修改

<table>
  <tr>
   <td colspan="2" ><strong>路徑</strong>
   </td>
   <td><strong>說明</strong>
   </td>
  </tr>
  <tr>
   <td>/base
   </td>
   <td>
   </td>
   <td>基本樣式如：display、space、grid、color 等基本樣式定義，整合在 _default.sass 後，由 main.sass 引入。
   </td>
  </tr>
  <tr>
   <td rowspan="2" >/layout
   </td>
   <td>/*.sass
   </td>
   <td>HTML 基本排版設定、包含 header、footer、sidebar 等區塊。所有檔案的設定整合在 _main.sass 中。
   </td>
  </tr>
  <tr>
   <td>/section/*.sass
   </td>
   <td>根據不同頁面設定的對應樣式。
   </td>
  </tr>
  <tr>
   <td>/modules
   </td>
   <td>
   </td>
   <td>模組樣式設定，根據功能分類如按鈕、牌卡、開闔、麵包屑等功能。所有檔案的設定整合在 _main.sass 中。
   </td>
  </tr>
  <tr>
   <td>/state
   </td>
   <td>
   </td>
   <td>根據不同頁面做差異的設定，部分與 /layout/section 性質有重複，主要是以系列頁面為單位做設定。
   </td>
  </tr>
  <tr>
   <td>/utils
   </td>
   <td>
   </td>
   <td>共用參數、函式設定
   </td>
  </tr>
  <tr>
   <td>main.sass
   </td>
   <td>
   </td>
   <td>main.sass 整合所有的樣式結構，可從檔案中看到

```scss
@import ./utils/variable 
@import ./utils/retina-background


@import ~slick-carousel/slick/slick.css
@import ~react-datepicker/dist/react-datepicker.css

// 基礎樣式的設定
@import ./base/default

// 基本版面的設定
@import ./layout/main

// 不同模組的主題
@import ./module/main

// 不同狀態或頁面的設定
@import ./state/default
@import ./state/farnet-page
@import ./state/rate-plan
@import ./state/exclusive
@import ./state/product
@import ./state/happy-go

// 不同主題的設定，目前沒用到  
@import ./theme/default
```
   </td>
  </tr>
</table>



### 三、修改及區塊設定方式

在頁面上針對需要修改或新增的模組，到對應的 CSS 調整樣式，若找不到對應的樣式可在 `/sass/module` 中新增檔案，並 import 進 ` /sass/module/_main.sass`，讓樣式可以順利被使用。

如果要根據不同頁面做差異化的設定，則建議新增在 ` /sass/state ` 中，並依照檔案 import 進 main.sass 中。


### 四、使用樣式工具

在 ` sass/base ` 中有對應的樣式名稱可參考，在切版時局部改變文字顏色、間距大小。並可根據大小網需求做 sm、md、lg、xl 的設定做出大小網不同的設定，以下列舉出常用項目，詳細使用方法可直接觀看 sass 檔案。 \

<table>
  <tr>
   <td colspan="3" ><strong>文字設定</strong>
   </td>
  </tr>
  <tr>
   <td><strong>樣式名稱</strong>
   </td>
   <td colspan="2" ><strong>說明</strong>
   </td>
  </tr>
  <tr>
   <td>p, .text, .body-1
   </td>
   <td colspan="2" ><strong>字級：</strong>16px(1rem)，<strong>行距：</strong>28px
   </td>
  </tr>
  <tr>
   <td>h1, .heading-1
   </td>
   <td colspan="2" ><strong>字級：</strong>32px，<strong>行距：</strong>32px
   </td>
  </tr>
  <tr>
   <td>h2, .heading-2
   </td>
   <td colspan="2" ><strong>字級：</strong>28px，<strong>行距：</strong>42px
   </td>
  </tr>
  <tr>
   <td>h3, .heading-3
   </td>
   <td colspan="2" ><strong>字級：</strong>24px，<strong>行距：</strong>36px
   </td>
  </tr>
  <tr>
   <td>h4, .heading-4
   </td>
   <td colspan="2" ><strong>字級：</strong>22px，<strong>行距：</strong>33px
   </td>
  </tr>
  <tr>
   <td>h5, .heading-5
   </td>
   <td colspan="2" ><strong>字級：</strong>20px，<strong>行距：</strong>30px
   </td>
  </tr>
  <tr>
   <td>h6, .heading-6, .subtitle
   </td>
   <td colspan="2" ><strong>字級：</strong>18px，<strong>行距：</strong>27px
   </td>
  </tr>
  <tr>
   <td>.body
   </td>
   <td colspan="2" ><strong>字級：</strong>18px，<strong>行距：</strong>32px
   </td>
  </tr>
  <tr>
   <td>small, .text-sm, .body-2
   </td>
   <td colspan="2" ><strong>字級：</strong>14px，<strong>行距：</strong>24px
   </td>
  </tr>
  <tr>
   <td>.text-mini
   </td>
   <td colspan="2" ><strong>字級：</strong>12px
   </td>
  </tr>
  <tr>
   <td>.is-text-bold
   </td>
   <td colspan="2" ><strong>字重：</strong>600
   </td>
  </tr>
  <tr>
   <td>.is-text-medium
   </td>
   <td colspan="2" ><strong>字重：</strong>500
   </td>
  </tr>
  <tr>
   <td>.is-text-regular
   </td>
   <td colspan="2" ><strong>字重：</strong>400
   </td>
  </tr>
  <tr>
   <td>.is-text-light
   </td>
   <td colspan="2" ><strong>字重：</strong>300
   </td>
  </tr>
</table>

<table>
  <tr>
   <td colspan="3" ><strong>Display 顯示</strong>
   </td>
  </tr>
  <tr>
   <td width='300'><strong>樣式名稱</strong>
   </td>
   <td><strong>說明</strong>
   </td>
  </tr>
  <tr>
   <td>d-inline, d-{size}-inline
   </td>
   <td rowspan="7" colspan="2" >對應到 CSS 中 display 的設定，建議對 CSS 較熟悉的再使用，剛開始只要使用 inline、 inline-block、block、none 就可以了。
<p>

**使用範例：** 
`<span class='d-none d-md-block'>小網隱藏、大網顯示</span>`
   </td>
  </tr>
  <tr>
   <td>d-inline-block, d-{size}-inline-block
   </td>
  </tr>
  <tr>
   <td>d-block, d-{size}-block
   </td>
  </tr>
  <tr>
   <td>d-table, d-{size}-table
   </td>
  </tr>
  <tr>
   <td>d-flex, d-{size}-flex
   </td>
  </tr>
  <tr>
   <td>d-inline-flex, d-{size}-inline-flex
   </td>
  </tr>
  <tr>
   <td>d-none, d-{size}-none
   </td>
  </tr>
</table>

<table>
  <tr>
   <td colspan="2" ><strong>對齊</strong>
   </td>
   <td>
   </td>
  </tr>
  <tr>
   <td><strong>樣式名稱</strong>
   </td>
   <td><strong>說明</strong>
   </td>
   <td><strong>備註</strong>
   </td>
  </tr>
  <tr>
   <td>align-center, align-center-{size}
   </td>
   <td>置中對齊
   </td>
   <td rowspan="6" >可參考<strong>瀏覽器斷點</strong>，在斷點以上的尺寸就會使用相關設定。
   </td>
  </tr>
  <tr>
   <td>align-left, align-left-{size}
   </td>
   <td>靠左對齊
   </td>
  </tr>
  <tr>
   <td>align-right, align-right-{size}
   </td>
   <td>靠右對齊
   </td>
  </tr>
  <tr>
   <td>float-left, float-left-{size}
   </td>
   <td>區塊向左浮動
   </td>
  </tr>
  <tr>
   <td>float-right, float-right-{size}
   </td>
   <td>區塊向右浮動
   </td>
  </tr>
  <tr>
   <td>float-middle, float-middle-{size}
   </td>
   <td>區塊置中
   </td>
  </tr>
</table>

<table>
  <tr>
   <td colspan="2" ><strong>間距</strong>
   </td>
  </tr>
  <tr>
   <td><strong>樣式名稱</strong>
   </td>
   <td><strong>說明</strong>
   </td>
  </tr>
  <tr>
   <td>m{dir}-{size}-{0-10}
   </td>
   <td rowspan="6" >

**m：** margin | **p：** padding 

**dir(方向)**
**t：** 上  |  **b：** 下  |  **l：** 左  |  **r：** 右  |  **x：** 左右間距  |  **y：** 上下間距 
 
**size：** 可參考**瀏覽器斷點**，在斷點以上的尺寸就會使用相關設定。

**使用範例：**
`<span class='mt-5'>上方間距增加 40px</span>`
`<span class='py-0'>左右空間 0</span>`

   </td>
  </tr>
  <tr>
   <td rowspan="5" >p{dir}-{size}-{0-10}
   </td>
  </tr>
  <tr>
  </tr>
  <tr>
  </tr>
  <tr>
  </tr>
  <tr>
  </tr>
</table>


**色表**

![alt_text](images/colors.png "image_tooltip")


<table>
  <tr>
   <td colspan="2" ><strong>顏色</strong>
   </td>
  </tr>
  <tr>
   <td><strong>樣式名稱</strong>
   </td>
   <td><strong>說明</strong>
   </td>
  </tr>
  <tr>
   <td>is-text-{color}
   </td>
   <td rowspan="2" ><strong>color：</strong>帶入顏色名稱，可設定文字或背景顏色
   </td>
  </tr>
  <tr>
   <td>is-bg-{color}
   </td>
  </tr>
</table>

---

## 肆、切版開發說明
### 一、HTML 大架構說明
HTML 架構大致如下：
```html
<html>
  <head>
    <!-- Meta / CSS / JS -->
  </head>

  <body>
    <!-- 共用 Header Start-->
    <header></header>
    <!-- 共用 Header End-->

    <!-- 頁面：/src/pages 切版的部分，每個頁面所處理的區塊 -->
      <main>
        <!-- Partials：/src/components/partials ，以 section 為單位做區分 -->
          <section>
              <!-- Component：/src/components 以一筆完整的資料為單位做區隔，如：牌卡、下拉選單......等 -->
              <Cards />
          </section>
      </main>
    <!-- 頁面 -->

    <!-- 共用 Footer  Start -->
    <footer></footer>
    <!-- 共用 Footer  End -->

  </body>
</html>
```

### 二、常用套件及共用工具
為方便開發，使用一些文件較為齊全的 react 套件便於日後維護與擴充。


##### Material-UI

網址：[https://material-ui.com/zh/](https://material-ui.com/zh/) 

主要使用在 Grid system，可直接按照文件處理排版。


##### PropTypes

網址：[https://www.npmjs.com/package/prop-types](https://www.npmjs.com/package/prop-types)

紀錄 components 使用的 props 與資料型別，建立模組與使用模組時可先看 PropTypes 確認需要使用的參數。


##### ReactSlick

網址：[https://react-slick.neostack.com/](https://react-slick.neostack.com/) 

輪播圖套件，使用方式可參考 Banner 的使用。


##### FormsyReact

網址：[https://github.com/formsy/formsy-react](https://github.com/formsy/formsy-react)  \
表單元件驗證套件，需注意使用 formsy 的模組，不可包兩層以上。


##### Paginate

網址：[https://www.npmjs.com/package/react-paginate](https://www.npmjs.com/package/react-paginate) 

用於需要分頁的多筆資料，因為有客製需求，將套件移至 **/components/paginate**，仍可參考主要的參數設定。


##### ScrollMemory

網址：[https://www.npmjs.com/package/react-router-scroll-memory](https://www.npmjs.com/package/react-router-scroll-memory) 

紀錄上一頁的滾動位置，也有客製需求，移動到 /components/ScrollMemory，使用方式如官方文件。


##### Utils

共用、客製函式區。

<table>
  <tr>
   <td><strong>檔案</strong>
   </td>
   <td><strong>說明</strong>
   </td>
  </tr>
  <tr>
   <td>validation.js
   </td>
   <td>
   
共同使用的表單驗證規則，引入需要的表單模組中。 

- isNumberValid：身號驗證 
- emailValid：Email 驗證 
- mobileValid：手機驗證 

**使用範例：**
```jsx
import * as Validation from '../../utils/validation';

<LabelInput
  name='id_number'
  label='身分證字號'
  validations={{
    idNumberValid: <strong>Validation.idNumberValid</strong>,
  }}
  validationErrors={{
     isDefaultRequiredValue: '請輸入身分證字號',
     idNumberValid: '請輸入有效身分證字號！',
  }}
  placeholder='請輸入身分證字號' \
/>
```
   </td>
  </tr>
  <tr>
   <td>ad-tracking.js
   </td>
   <td>追蹤碼回傳的函式
   </td>
  </tr>
  <tr>
   <td>device-util.js
   </td>
   <td>判斷是否為行動裝置，用於 ad-tracking.js 中
   </td>
  </tr>
  <tr>
   <td>inputFilter.js
   </td>
   <td>用於信用卡付款判斷只能輸入數字的功能。
   </td>
  </tr>
  <tr>
   <td>trackingRoute.js
   </td>
   <td>建立追蹤碼用
   </td>
  </tr>
</table>

---

## 伍、Router 及全域設定

### 一、App.js
<table>
  <tr>
   <td><strong>行數</strong>
   </td>
   <td><strong>函數</strong>
   </td>
   <td><strong>說明</strong>
   </td>
  </tr>
  <tr>
   <td>62
   </td>
   <td>window.addEventListener('popstate')
   </td>
   <td>
   React 回上一頁時無法觸發 API，於是設定回到上一頁時畫面重新讀取。
   若是從結帳流程回到申辦流程則不會啟動重新讀取機制。
   </td>
  </tr>
</table>

### 二、Loader.js
<table>
  <tr>
   <td><strong>行數</strong>
   </td>
   <td><strong>函數</strong>
   </td>
   <td><strong>說明</strong>
   </td>
  </tr>
  <tr>
   <td>21
   </td>
   <td>useLayoutEffect
   </td>
   <td>
有時候 useEffect 會失效，故加上 useLayoutEffect 偵測畫面是否有更新。

1. 判斷是否為申辦流程
2. 是，執行 gotoHash 根據 url hash 進入申辦流程對應的位置。
3. 否，執行 loadingAnimate 並判斷是否有 url hash，有則再執行 gotoHash。
   </td>
  </tr>
  <tr>
   <td>63
   </td>
   <td>useEffect
   </td>
   <td>window.onload 時執行 loadingAnimate 並判斷是否有 url hash，有則再執行 gotoHash。
   </td>
  </tr>
  <tr>
   <td>184
   </td>
   <td>unlisten
   </td>
   <td>偵測網址異動後執行判斷 

1. 判斷是否為申辦流程
2. 是，執行 gotoHash 根據 url hash 進入申辦流程對應的位置。
3. 否，執行 loadingAnimate 並判斷是否有 url hash，有則再執行 gotoHash。
   </td>
  </tr>
  <tr>
   <td>156
   </td>
   <td>detectPath
   </td>
   <td>根據全域變數判斷放入共用樣式名稱
   </td>
  </tr>
  <tr>
   <td>170
   </td>
   <td>gotoHash
   </td>
   <td>根據 url hash 捲動到對應位置
   </td>
  </tr>
  <tr>
   <td>227
   </td>
   <td>loadingAnimate
   </td>
   <td>進場畫面流程
   </td>
  </tr>
  <tr>
   <td>271
   </td>
   <td>setHeader
   </td>
   <td>頁面更新時重設 header 狀態，避免保留有 navAnchor 時的 style 設定。
   </td>
  </tr>
</table>

---

## 陸、Redux 使用方式
Redux 的優點與基礎操作說明可參考[中文文件](https://chentsulin.github.io/redux/index.html)
目前在 `src/App.js` 中已有預設載入 Redux，只需在 `/src/stores` 中加入對應的 action 與 state ，並引入模組中即可。

### 一、State
以購物車為例，在 `/src/stores/state.js` 新增 cartState 紀錄購物車資料。
```jsx
export const cartState = {
  cart: null, // 購物車
};
```

### 二、Reducer
新增 `/src/stores/reducer/cart.js` 設定 cartReducer 處理購物車的新增、修改、刪除。
```jsx
import { TYPES } from '../action'; // 載入 actionType
import { cartState } from '../state';

export default function cartReducer(state = cartState, action) {
  switch (action.type) {
    case TYPES.UPDATE_CART_DATA:
      return Object.assign({}, state, {
        list: action.cart,
      });

    case TYPES.REMOVE_CART_DATA:
      let removeList = null;
      // console.log('cartReducer', state, action);
      if (Array.isArray(state.list)) {
        removeList = [...state.list];
        removeList.splice(action.index, 1);
      } else {
        removeList = { ...state.list };
        removeList[action.product].splice(action.index, 1);
      }

      return Object.assign({}, state, {
        list: removeList,
      });

    case TYPES.SET_CART_DATA:
      return Object.assign({}, state, {
        list: action.cart,
      });

    default:
      return state;
  }
}
```

### 三、Action/action/index.js 設定從模組呼叫 redux 的函式。
```jsx
//設定對應的 actionType 與 reducer 同步
export const SET_CART_DATA = 'SET_CART_DATA';
export const REMOVE_CART_DATA = 'REMOVE_CART_DATA';
export const UPDATE_CART_DATA = 'UPDATE_CART_DATA';

export const TYPES = {
  SET_CART_DATA,
  REMOVE_CART_DATA,
  UPDATE_CART_DATA
}

// 根據刪除購物車所需要的參數與資料，個別帶入需要的參數。
export function removeCartData(index, product) {
  return {
    type: REMOVE_CART_DATA,
    index,
    product,
  };
}
// Actions
export function updateCartData(index, cart) {
  return {
    type: UPDATE_CART_DATA,
    index,
    cart,
  };
}
// Actions
export function setCartData(cart) {
  return {
    type: SET_CART_DATA,
    cart,
  };
}
```

### 四、Componentonent 中帶入對應的 action 與 redux 函式。
```jsx
import { setCartData } from '../../stores/action';
import { bindActionCreators } from 'redux';
import { connect } from 'react-redux';
```

將需要引入的 state 與 action 傳入 props
```jsx
const mapStateToProps = (state) => {
  return {
    cart: state.cartReducer,
  };
};

const mapDispatchToProps = (dispatch) =>
  bindActionCreators(
    {
      setCartData,
    },
    dispatch
  );

export default connect(mapStateToProps, mapDispatchToProps)(YourComponenetName);
```

在對應的 method 中呼叫 redux action 將資料存入
```jsx
addToCart = () => {
  this.props.setCartData(list);
}
```

---

## 柒、模組與流程

### 一、基本模組ments/button.md)
- [Link](./elements/link.md)
- [Item](./elements/item.md)
- [LinkItem](./elements/linkitem.md)
- [SocialMedia](./elements/socailmedia.md)
- [Table](./elements/table.md)

### 二、表單模組

表單元件 HTML 結構，如果有客製化狀況，可依照下方表單結構自行變化 HTML 內容
```html
<div className='form-group'>
  <label>欄位標題，可省略<label>
  可直接放 
  <LabelInput /> 等欄位內容或
  <div>
    用 div 包住客製的 HTML 元件，如 DatePicker
  </div>
</div>
```


**Fetnet 表單模組**
- [Formsy](./form/formsy.md)
- [Input](./form/input.md)
- [Radio button](./form/radio-buton.md)
- [Checkbox](./form/checkbox.md)
- [Select](./form/select.md)
- [Textarea](./form/Textarea.md)

**Estore 表單模組**
- [CheckboxCards](./form/checkbox-cards.md)
可選擇商品的牌卡列表
- [CheckboxCardCarousel](./form/checkbox-card-carousel.md)
商品選擇輪播模組，用於申辦流程
- [ColorSelector](./form/color-selector.md)
用於產品頁選擇顏色
- [DatePicker](./form/date-picker.md)
日期選擇
- [ImageUpload](./form/image-upload.md)
圖片上傳
- [NumberSelector](./form/number-selector.md)
產品數量選擇器
- [RadioCard](./form/radio-card.md)
申辦流程的資費牌卡
- [RadioCarousel](./form/radio-carousel.md)
用於門號選擇


### 三、主要共用模組
- [EstoreBanner](./estore/estore-banner.md)
Estore 的共用 Banner
- [EstoreModal](./estore/estore-modal.md)
Estore 為方便管理共用的跳窗
- [CompareBar](./estore/compare-bar.md)
資費比一比控制條與跳窗
- [Cart](./estore/cart.md)
一般商品購物車
- [FlowCart](./estore/flow-cart.md)
申辦流程購物車

### 四、頁面流程
- [申辦流程](estore/apply-flow.md)