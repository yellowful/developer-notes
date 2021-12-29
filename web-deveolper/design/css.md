# css筆記

1. [repo](https://github.com/yellowful/css-exercise)
2. css的c是cascading，意思是同時出現相同的樣式時，以最後出現的為準。
3. 插入css有三種方式：
   1. tag的一種屬性
   2. 或在head裡用一組style的tag包住
   3. 分開的一個檔案，在head裡去連結(rel代表relation)
4. 為什麼要把css分開成一個檔呢？主要是大的project如果通通塞在一個html檔，易讀性太差。而且可以讓設計人員不需要去煩腦html裡的程式碼，專心於設計css檔就好了。
5. 顏色配色用這個網站：  
    <https://paletton.com/>
6. 常用的css的property參考：  
    <https://css-tricks.com/almanac/>
almanac有詳列property
6. css重要用法： 

    <table>
    <tr>
        <th>css檔裡</th>
        <th>html檔裡</th>
        <th>說明</th>
    </tr>
    <tr>
        <td>h1, h2</td>
        <td>h1 /h1 <br>h2 /h2</td>
        <td>代表h1和h2都選擇了</td>
    </tr>
    <tr>
        <td>.xxx(.class)</td>
        <td>class = xxx</td>
        <td>很常用，很好用，可以重覆使用，是這個class都會被選</td>
    </tr>
    <tr>
        <td>#xxx(#id)</td>
        <td>div id = xxx /div</td>
        <td>html裡id是唯一，不能重覆</td>
    </tr>
    <tr>
        <td> * </td>
        <td></td>
        <td>所有沒被css設定過樣式的都會被設定</td>
    </tr>
    <tr>
        <td> p h2 </td>
        <td> p div h2 /h2 div /p</td>
        <td>選擇的是p裡面只要有h2的都會被選，子孫都會被選到的意思</td>
    </tr>  
    <tr>
        <td> p > h2 </td>
        <td>p h2 /h2 /p</td>
        <td>h2的父繼承自p時才會被選到，如果h2被div包到，這時候h2父繼承自div，則不會被選到。只有兒子會被選到的意思。</td>
    </tr>
    <tr>
        <td> p + h2 </td>
        <td>p h2 /h2 /p</td>
        <td>h2剛好接在p後面時才會被選到，如果中間隔有其它tag，則不會被選到。例如中間也有一個div，一樣不會被選到。只有弟弟會被選到的意思。</td>
    </tr>
    <tr>
        <td> .xxx:last-child </td>
        <td>div .xxx <br> .xxx div</td>
        <td>每一個div裡面，最後一個class</td>
    </tr>  
    <tr>
        <td> .xxx:first-child </td>
        <td>div .xxx <br> .xxx div</td>
        <td>每一個div裡面，第一個class</td>
    </tr>  
    <tr>
        <td> !important </td>
        <td>color: pink !important</td>
        <td>很重要的一點就是，儘量不要用，因為會破壞css的規則，破壞程式易讀性，程式很大時，會找不到bug</td>
    </tr>  
    <tr>
        <td> h1:hover </td>
        <td> h1 /h1</td>
        <td>個人覺得有趣，但不實用，手機無法顯示效果<br>值得一提的是，要讓last child產生hover的話，語法為:hover:last-child，瀏覽器會去認最後那個last-child</td>
    </tr>  
    </table>  

7. 其中，逗號最不特定，代表都選擇。空格較不特定，>和+都是比較特定的情況，>是和繼承有關，+是和連接有關。
8. 判斷重要性有三個原則：
> 1. 特定性：愈特定，重要性愈高，由加總最高重要性的方式畫出來。  
寫在html > 的tag裡(inline styles) > ID > class > 一般element
> 2. !important，但最好不要用。
> 3. css的出現順序，愈晚的愈優先。載入css樣式檔也是，列愈後面的檔，愈優先。
9. 其它selector練習：https://flukeout.github.io
> * :nth-child(2)：順得數第2個，而且是**不管哪種type**
> * A.className：所有這個class裡的a
10. 文字：
> * 可以改的常用屬性有style(italic)、weight(bold, lighter)、size(x%)、decoration(underline, line-throgh)
> * 網頁內容要顯示中文的話，記得在head的地方設定meta charset，另外可以在字型設定的地方(font-family)，備用字型放上google的字型，例如：Noto Sans TC。  
注意，html要在head載入網路的google fonts，這會有個缺點，網頁載入會稍慢一點。
> * 尺寸有px、em、rem，em是相對本段的比例，rem是相對root(html)的比例，所以調本段字型大小時，就不會受影響。
11. 圖片：
> * float屬性，可以讓文字繞圖，可以讓圖靠右或靠左。
> * clear:both屬性，則可以讓文字取消圖的float屬性，不再繞圖。
12. 盒子：
> * 由內而外是content, padding, border, margin
> * width和height是content的大小
> * display: inline-block的意思是content的文字，不自動斷行。
> * border要設solid才看得見  
13. position：
    1. static：沒什麼用途，就照html裡div的位置安排。
    2. relative：會照原本static所在位置，位移一定距離，很好理解。
       1. 位移位置是相對原本static的位置，而不是相對parent的位置。
       2. 不會影響兄弟的位置。
    3. absolute：最難理解，它會跳脫本身static的位置，相對某個ralative的parent的位置 而移動。
       1. 因為不管原本static的位置了，原本static的位置會消失，被兄弟div補上。
       2. 位置是相對parent移動，重點是這個parent必須不是static，如果是static就會忽略，會再找更上層。
       3. 要蓋在別的div上，就得用absolute才有辦法蓋在其他兄弟上。
       4. 可以配合設定top right bottom left的像素等於0，來控制靠左或靠右。
14. 背景照片透明文字不透明的用法：
    1.  css沒有所謂透明背景的用法，而是把圖片設透明，然後把z軸改成-1、位置設成absolute，蓋在另一個relative圖層上，如下：
    2.  div {
          width: 200px;
          height: 200px;
          display: block;
          position: relative;
        }

        div::after {
          content: "";
          background: url(image.jpg);
          opacity: 0.5;
          top: 0;
          left: 0;
          bottom: 0;
          right: 0;
          position: absolute;
          z-index: -1;
        }
15. tachyons：
    1. 一種css的框架。
    2. 用class命名和css幾乎一樣，用class就可以操作css，用框架的同時可以練習css
    3. 完全responsive，用命名就可以設定不同螢幕大小的顯示方式，不用設定@media
    4. 最重要的觀念是：
        1. 預設大小是手機
        2. 不是手機大小的時候
            1. -m：比是中螢幕大小大的話要顯示的
            2. -l：比大螢幕大的話要顯示的
            3. -ns：-m或-l其中至少某一個沒設定的時候，會取代那個設定
            4. 所以-m和-l同時有設定時，就不用設定-ns
            5. 如果只有設定-m的話，那-l的設定回函-m相同
    5. 容易忘記的class：
       1. input-reset：讓ios的input不會變形
       2. button-reset：讓按完button不會有很醜的藍色外框
16. 其他：
    1. ::before和::after：
       1. 主要是在原來的tag「內」的content「前」和「後」，加入東西。
       2. 裡面可以加上屬性content:“內容”，如此可以不用用html而達到插入網頁內容的功能。
    2. display：
       1. flex：會橫向延伸到最邊邊。
       2. block：一個可以設定width和height的區塊，下一個區塊會換行。
       3. inline：不換行，不能設定width和height的區塊。
       4. inline-block：一個可以設定width和height的區塊，和inline一樣，不換行。
    3. width：
       1. min-with：可以超出div
       2. max-with：還沒碰到div邊緣，也不會再更大。
    4. 要緊密綁在一起的東西，可以用table把東西都綁在一起。
    5. 字型：在`@font-face`裡面，`font-display:swap`代表先載入瀏覽器字型，之後再切換要用的字型。

## Less, Sass, Scss 預處理器

1. 三者不同之處：
   1. Sass和Scss很像，只是Sass和python一樣是縮排格式，Scss和JS一樣是{}格式。
   2. 變數開頭不同：
      1. Less：`@`
      2. Scss：`$`
   3. 功能不同：
      1. Less有基本的變量，繼承，運算，函數功能。
      2. Scss多了一些功能。
   4. css函式庫：
      1. Bootstrap：Less
      2. Bulma：Scss
2. [scss教學影片](https://www.youtube.com/watch?v=roywYSEPSvc)：
   1. 檔名用底線開頭不會被compile，其他會被compile成個別的css檔。
   2. 匯入其他檔案：`@use ‘檔名’；`或是`@import '檔名'；`
   3. mixin：
      1. 和`@function`很像，`@function`會return值，`@mixin`不會，`@mixin`是設定屬性。
      2. 引用的時候要有`@include`，而`@function`不需要`@include`。
      3. [參考](https://ithelp.ithome.com.tw/articles/10156850)
      4. `@content`：代表呼叫這個mixin時的設定，要放在哪裡，例如：

         ``` scss
         @mixin desktop{
           @media(min-width:#{$desktop}){
              @content
           }
         }
         body{
           font-family:'Montserrat';
           margin:0;
           #bg {
             width:100%;
             height:100%;
             @include desktop{
               width:80%;
               height:80%;
             }
           }
         }
         ```

   4. extend：
      1. 關鍵字是`%`和`@extend`
      2. 主要可以創建一些共用的css，不用repeat yourself。

         ```scss
         %message-shared{
           border:1px solid #ccc;
           padding:10px;
           color:#333;
         }
         .message{
           @extend %message-shared;
         }
         .success{
           @extend %message-shared;
           border-color:green;
         }
         ```

   5. 邏輯：`@if`和`@else`
   6. 替代parent的符號：`&`
   7. 內建function：
      1. 顏色變淡：`lighten(顏色,10%)`
      2. 顏色變深：`darken(顏色,10%)`
      3. 測量亮度(例如背景色)：`lightness(變數)`
   8. Loop：
      1. 關鍵字：`(item1,item2....)`,`@each in`,`#{item}`
      2. 像是JS的forEach的用法，可以用來做出多個class，例如m-1,m-2,p1,p2之類的。

         ```scss
         $space-amounts = (1,2,3,4,5);
         @each $space in  $space-amounts {
           .m-#{$space}{
             margin:#{space} em;
           }
           .p-#{$space}{
             padding:#{space} em;
           }
         }
         ```

   9. 多層變數的用法：
      1. 關鍵字：
         1. `$變數`：外層變數要`$`，內層變數不用`$`
         2. `();`：是小括號不是大括號
         3. `逗號`：內層變數是逗號結尾，不是分號結尾
         4. `map-get(外層變數,內層變數)`：呼叫內層變數的時候是用這個function呼叫
      2. 範例：

         一般的用法：

         ```scss
         $color:(
           primary:#005DFF,
           second:#FFF6BB
         );
         body{
           background-color:map-get($color,primary);
         }
         ```

         比較漂亮的寫法：

         ```scss
         $color:(
           primary:#005DFF,
           second:#FFF6BB
         );
         @function color($color-name){
           map-get($color,$color-name)
         }
         body{
           background-color:color(primary);
         }
         ```

3. bulma：
   1. 算是精簡版和純css的bootstrap。
   2. 自由度較低，為了增加自由度，最好用自訂變數的用法：
      1. <https://bulma.io/documentation/customize/with-node-sass/>
      2. 開一個自訂css的sass專案。
      3. 灌node sass和bulma進去。
         `npm install node-sass --save-dev`
         `npm install bulma --save-dev`
      4. 改package.json：

         ```js
         "scripts": {
           "css-build": "node-sass --omit-source-map-url sass/mystyles.scss css/mystyles.css",
           "css-watch": "npm run css-build -- --watch",
           "start": "npm run css-watch"
         }
         ```

      5. 把你要自訂的功能和class貼進去html。
      6. 把要自訂的變數貼入後，再匯入bulma.sass
         @charset "utf-8";
         自訂的變數
         @import "../node_modules/bulma/bulma.sass";
      7. 看結果：`npm start`
      8. 編譯成css：`npm run css-build`
      9. 把編譯完的css檔放到react的js檔裡面去import，react app不用再灌bulma。
   3. 自訂的話：
      1. 如果針對bulma某個component，用更動component的變數，比較好，就不會去動到大家其他的變數。
      2. 可以import必要的.sass檔就好了，就不用匯入整個bulma.sass，比較不會有class name conflic發生。
      3. 特別是elements和components最好不要全部匯入，很容易衝突。
