# performance

## part 1

1. 可以改進performance有三個方面：
   1. frontend：
      1. critical render path
      2. optimized code
      3. progressive web app
   2. internet latancy
      1. minimize files
      2. minimize delivery
   3. server side response
      1. CDN
      2. caching
      3. load balancing
      4. gzip
      5. db scaling
2. 改進performance最重要的事，能夠找出performance的比較基礎，有能比較的數字，針對效能差的地方去找出來改進，否則將很可能把performance不差的地方改差，浪費時間。
3. minimize files：
   1. 相片：
      1. 選對檔案格式
      2. media query：`@media screen`
         1. 根據螢幕調整相片檔案大小，讓不同螢幕背景指向不同尺寸的檔案。
         2. 瀏覽器會跟據大小下載符合的相片而已，**不符合的並不會在背景偷偷下載**。
         3. [各種螢幕的尺寸](https://css-tricks.com/snippets/css/media-queries-for-standard-devices/)：
            1. Retina螢幕：`-webkit-min-device-pixel-ratio: 2`
            2. 直躺：`orientation: portrait`
            3. 橫躺：`orientation: landscape`
         4. [各種螢幕的尺寸](https://gist.github.com/bartholomej/8415655)
      3. CDN：
         1. 用[imgix](https://imgix.com/)
         2. imgix會自動最小化相片
      4. 去掉meta data：
         1. [verexif](https://www.verexif.com/)可以看得到meta data，也可以用來移除。
         2. 可以減少幾k。
         3. 可以保護隱私。
      5. JPG：
         1. 降到30~60%的quality，會大大減少體積。
      6. [hero image](https://www.freecodecamp.org/news/gatsby-perfect-lighthouse-score/)：
         1. 大大減少了lighthouse的分數。
         2. 改成svg背景，速度會大大加快。
         3. resources
            1. [Hero Patterns](https://www.heropatterns.com/)
            2. [SVG Patterns](https://philiprogers.com/svgpatterns/)
            3. [SVGOMG](https://jakearchibald.github.io/svgomg/)
   2. 瀏覽器：
      1. 有傳輸限制：
         1. max parallel requests數目
         2. 檔案數目
      2. chrome開發工具：
         1. Network：
            1. slow 3g
            2. disable cache
            3. filter
         2. refresh
            1. 長按refresh可以hard reload
            2. 或是shift+cmd+r
   3. critical render path：
      1. 瀏覽器下載順序：DOM => CSSOM => DOMContentLoaded => render tree => layout => paint => load
      2. js可能在DOMContentLoaded之前就開始加入，依html的順序，可能馬上就執行造成parse blocking，最後才變成render tree
      3. 相片不是和html+css+js一起下載的，他是自己在背景下載的，一但下載完成就會顯示。
      4. 瀏覽器最先下載html，然後開始parse裡面的文字，parse的時候如果看到外面的資源會同步下載，其中看到css和js會較優先下載。
      5. 優化：
         1. 讓css愈早load愈好，js愈晚load愈好，因為js最好DOM和CSSOM都好了再render。
            1. javascript parser blocking：javascript在如果放在header下載，會擋到DOM和CSSOM的下載和建立。
            2. 有些javascript的程式碼跑很久，會阻擋DOM和CSSOM的下載和建立。
            3. 例外：google analytics需要先執行，不過這會降低網站速度。
            4. css要放在head裡面，這稱為above the fold loading，因為有css render blocking的問題。
         2. css render blocking：
            1. 大家都在等css render好，才能往下做。
            2. 使用者會先看到css render好的畫面。
            3. 解決css render blocking的方法：
               1. 不重要的css先不要load
               2. load不重要的css的方式：最下面寫一段javascript去load，在`window.onload`的時候，才去`document.createElement('link')`，把css載入。
               3. 這樣會讓first load時間大大加快，因為不重要的css是在load完之後才load，你會看到那個style閃一下。
            4. media attribute：可以這樣用`<link media="only screen and (min-width:500px)">`，讓不需要螢幕，不下載某些css。
            5. less specificity：影響比較不大
               1. css條件愈多，render起來愈慢，因為要在parse DOM和CSSOM的時候，很耗cpu。
               2. 例如，以下條件就很多：

                  ```css
                  .header .nav .item .link a.important {
                     color:pink;
                  }
                  ```

               3. 搞不好這是tachyons的優勢，每一個DOM單獨的css，而不會有很複雜的邏輯考驗瀏覽器，速度會較快。
            6. 將css寫在html裡可以加快速度：
               1. 在html裡用style的tag
               2. 在element上用style的attribute
               3. 缺點：每一個html page都要去寫css，css不能重覆用，所以多頁網頁的時候很麻煩。
            7. 解決javascript造成的parser blocking(第47課)：
               1. 不同瀏覽器行為可能不同，利如chrome已經不會讓js造成parser blocking了。
               2. 利用同步：
                  1. `<script async />`：
                     1. script下載後馬上執行。
                     2. 不保證執行順序。
                  2. `<script defer />`：
                     1. script下載後不會執行，等到HTML parsing完成後，也就是在`DOMContentLoaded`，才會執行。
                     2. 會依照script放的位置執行。
                     3. 但是我親自試驗的結果，defer會馬上執行。
                  3. <https://stackoverflow.com/questions/10808109/script-tag-async-defer>
               3. 如果載入的JS libarary是core function，可以用`<script async />`，如果載入的JS library不是core function，可以用`<script difer />`。
               4. 自己寫的重要的scripts或是app scripts可以用一般的`<script />`。
               5. event listener：
                  1. DOMContentLoaded：DOM都parse完了，但圖片可能還在背景下載。
                  2. load：所有東西都下載了，包括圖片。
               6. client side render：
                  1. JS可能改變render tree，接著瀏覽器又會重跑一次後半部的流程`layout => paint => load`
                  2. 測試performance的網站：
                     1. [PageSpeed Insights](https://developers.google.com/speed/pagespeed/insights/)
                     2. [webpagetest](https://www.webpagetest.org/)
4. [練習](https://yellowful.github.io/keiko-corp/)
   1. 我的想法：
      1. 原本：
         1. document complete：2.856
         2. fully loaded：2.910
         3. 最大的paint：3.458
         4. first contentful paint：0.787
      2. hero.jpg
         1. 要縮小
         2. 並且依media query載入。
         3. 要用progressive壓縮。
         4. 改完最大paint降到1.278
      3. icon1~5由png改成svg：
         1. <https://heroicons.com/>
         2. 改了之後
            1. 最大paint降到1.177
            2. fully loaded降到1.301
            3. 改完最大paint升到1.288
            4. first contenful paint:0.705
      4. 看起來所有的js都在DOMContentLoaded之前就載入且執行
         1. 可是blocking time是0，似乎沒關係？
         2. 其它library用async載入
            1. 改完first contentful paint降到0.681
            2. 但是最大contentful paint卻升到3.5
         3. main.js移到最前面
      5. 用javascript讓字型在load之後再載入？
      6. 加入favicon.ico
   2. 解答：
      1. 移除jquery：去google"you don't need jquery"，可以找到資源把jquery程式碼改成原生的JS。
      2. 把最上面的css用`<style />`寫好，其它的用javascript等page load之後再load就好了。
      3. 要思考：不需要的動畫不要用，否則會跑一堆javascript。
      4. 其它方式優化工具：第55課
         1. [prefetch](https://css-tricks.com/prefetching-preloading-prebrowsing/)
         2. chrome裡面的工具：
            1. [可以找出最耗時的活動](https://developer.chrome.com/docs/devtools/evaluate-performance/reference/#activities)
            2. [動畫](https://developer.chrome.com/docs/devtools/evaluate-performance/reference/#fps)：可以看出網頁載入或是互動的順不順，不順的會變紅色。
            3. [即時監控資源](https://developer.chrome.com/blog/new-in-devtools-64/#perf-monitor)
            4. [各個過程的截圖](https://developers.google.com/web/tools/chrome-devtools/evaluate-performance/reference#screenshots)
   3. http2：
      1. binery而不是textual
      2. parallelism：可以同時傳很多檔案
      3. server push
      4. 還沒被很廣泛的使用，但很快會被擴泛使用的
      5. 用法：在node.js裡用`require('http2')`
      6. [原理](https://developers.google.com/web/fundamentals/performance/http2/)
      7. [http3還不普遍](https://blog.cloudflare.com/http3-the-past-present-and-future/)
   4. everything is a tradeoff：如果流量很小，根本不需要optimization。
5. module bundling：
   1. 三大frame work比較：
      1. angular：
         1. 是frame work提供所有解決方案，像是廚房。
         2. 大專案適合，例如銀行業。
         3. 有特定的寫code方式，所以一大堆不同工程師，大家需要follow相同的規則。
      2. react：
         1. 彈性，像烤箱。
         2. 很強的團隊，可以依不同專案需求加入不同的library。
         3. 可以選擇不同專長的工程師去做不同的事情。
         4. 他的語法和javascript相同，一生受用，但是angular有特殊的語法，angular以外用不到。
      3. vue：
         1. 簡單易入門，適合小專案，像微波爐。
         2. 假如顧用很多junior developer的話，適合用。
   2. why react?
      1. 隨著專案變大，jquery的DOM manupulate難以追蹤什麼改變了，什麼還沒改。
      2. 比較declarative。
      3. 特別是view的部份，state很predictable而且容易scale。
      4. React只管view的部份，所以可以用在瀏覽器以外的部份，例如VR眼鏡、手機或是desktop的native app。
      5. separation of concerns，容易分工。
      6. component的思維：
         1. components：
            1. atoms：無法分離的item，例如：icons,images,tags
            2. molecules：提供一些(共同的)功能，會組合一些atoms，例如：navigation bar
            3. organisms：和molecules很像，只是提供更大的功能。
            4. templates：由organisms組合，用來定義你的整個網站要長什麼樣子。
            5. pages：你的app的頁面。
         2. 好處：
            1. 可以重複利用
            2. 例如：[MDB](https://mdbootstrap.com/docs/react/)的component，我們都可以拿來用。
            3. 這些compoent不是只有html和css，甚致是包含功能的。
            4. virtual DOM：沒有真的DOM，只變更需要變更的view，所以效能較佳。
      7. one way data flow：爸爸狀態變了，只有小孩的狀態會變，其它親戚的state都不會變，所以比較predictable。
      8. virtual DOM：react會在底層去query select那些element，去做DOM manupulation。
      9. 生態：生態系完整。
   3. react app的建立：
      1. `package.json`：
         1. `react-script`幫我們弄上最新的webpack、babel…等等。
         2. 假如project愈來愈大，可以用`eject`來customize `react-script`。
         3. manifest.json用來處理讓網頁在桌面建立捷徑的icon
         4. `ReactDOM.render(<App />,document.getElementById('root'))`：
            1. ReactDOM是import進來的object
               1. ReactDOM是給web page用的
               2. 手機或其它裝置就不用ReactDOM了，例如ReactNative。
            2. render的method
               1. 第一個參數是放jsx語法的component
               2. 第二個參數是放，要render html裡面的哪一個element
         5. React.Component：
            1. React是一個object
            2. .Component是一個attribute，是一個class
         6. React被import是用來解讀jsx用的
      2. serviceworker：
         1. `.register()`：progressive才需要
         2. 平常`.unregister()`就好了
      3. classes vs hooks：
         1. hooks的語法非常react，其它地方用不到。
         2. 而classes的語法非常javascript，甚至其它語言也用得到。
         3. hooks有一個缺點是他會增加react的complexity，要小心使用。
         4. class component的寫法很confusing：
            1. 實際上可以看成一個javascript會return jsx的function而已。
            2. 這個class被initiate成一個object的時候，會被執行`.render()`這個method，就會return jsx
      4. React.Fragment：
         1. 用short hand就好：`<> </>`
         2. [特殊情況才需要用](https://blog.logrocket.com/rendering-child-elements-react-fragments/)，例如html5的description list裡面，如果用.map來做，需要放key的時候，不能放`<> </>`，就只能用：

         ```jsx
         <React.Fragment key={word}>
            <dt>{word}</dt>
            <dd>{definition}</dd>
         </React.Fragment>
         ```

      5. key：
         1. `.map()`裡面的component要加上key，因為下次render要刪掉一個component的時候React才知道要刪掉哪一個，才不用全部一起重新render，效能才會好。
         2. key**不要用**index，要用不會變動的，要用會跟著component的。因為如果用index的話，下次`.map()`的array少一個item的話，index會跟著變動，React還是會不知道是哪一個item被刪除了，所以會失效。
      6. 兩個component之間要溝通，要透過從爸爸建立一個自己的state，然後變成小孩的props往下傳。
         1. pure component、dumb component、presentational component：只有props而沒有state，是一個pure function。
         2. state在computer science的領域代表的是你的app描述。
         3. react裡面state就是一個object，這個object可以描述你的app。
         4. state可以改變可以變動，例如因為操作或是fetch而變動，props單純是爸爸的state傳過來，props無法改變。
            1. class component裡宣告state的方式和一般js的class裡宣告一個變數的方式是一樣的，`this.state = {key:value,...}`。
            2. this.state其實就是這個class被initiate之後的object的其中一個attribute叫作state。
            3. render(){}
               1. 就是一個method，名稱叫作render，只是這是React預先建立的API，之後會把這個method回傳的東西拿去render。
               2. 和一般我們自己建立的不同在於render裡面認得this是誰，而我們自己建立的function不認得，所以為了解決這個問題有兩個方法
                  1. 在constructor裡面去.bind(this)
                  2. 用arrow function，因為arrow function沒有this，所以他的this就會是component被建立時，代表這個component。
               3. 在render()裡面設定的variable會隨著state更新而變動，每次有state更新就會跟著更新，跟hooks裡面是一樣的，所以改變view不一定是state，在render裡面被state改變的variable也可以改變view。
               4. 基於上一點，在render裡設定state和在hooks裡設定state一樣，要小心會成為無窮迴圈。
            4. this.setState()就是從super()複製而來，也就是React寫好的Component裡面已經寫好setState()的method怎麼運作了。
            5. 當成props傳下去的function被執行，就會改變state，state就會被轉成props傳進render()這個pure function，就可以被預期的render出來。
      7. componentDidMount()和render()一樣是React的life cycle的一部份，所以不需要arrow function。
      8. `constructor(props)`：
         1. constructor的小括號裡，就像是定義function的小括號，是外面可以傳進來的variable，所以在react裡面，就是這個class component被instantiated的時候(被new的時候)，會傳進來的參數，那就會是props。
         2. `super(props)`，super是複製Component這個class裡面的code，也會是現在這個class component的一部份，所以也會需要帶props進去給他，就像帶props進現在這個class一樣。
         3. 如果想要改變props，可以先當作這個class component的state的初始值，再改變state來用。
      9. component也可以是功能性的：
         1. 例如`<scroll />`就是只產生sroll的功能，任何想要有scroll功能的app，我們都可以加上去。
         2. children：component有三種東西可以用，state、props、children，每一個props都有children，不需要傳遞就會有。
      10. error boundry在development mode底下不會顯示，但在build mode底下會顯示。
      11. hooks：
          1. React專用語法。
          2. useState或是useEffect不能放在內部一般function裡面。
          3. useState或是useEffect不能放在code block裡面，意思是不能放在if else或是loops裡面。
          4. hooks另一個用途是可以用來做重複使用的state，可以跨component使用，例如寫一個React的functional component，然後會回傳Online或是Offline的字串，用來抓取使用者的上線狀態，並且讓各個component裡面可以用到。聽說這個用class component很難做得到？
      12. redux：
            1. 概念：
               1. 可以減少rerender。
               2. state可以改變，props不可以改變，在React底下，當App愈來愈大，會有很多container component，也就是有很多state需要追踨，因為他們會改變，這造成追踨困難。而redux把state集中在一個地方管理，而且用sql的技術良好管理。
               3. 第103課state management：React很擅長處理view，但不見得很擅長處理state，處理state是redux的專長。
               4. flux pattern：
                  1. action => dispatcher => store => view
                  2. action就是event (value)，dispatcher就是reducer這個pure function，store會把state變成props丟出來，view就是react
               5. mvc：
                  1. action => controller => model => view
                  2. action就是event，controller就是javascript，model可以看做state，view就是結果。
                  3. 其中controller會產生一大堆沒規律難以追踨的state，這些state會改變view，但是這些view可能又改變了state而去改變了view，很難追踨。 
               6. 總之redux就是用來取代this.state的。
            2. 實作注意事項：
               1. 用redux toolkit可以協助快速建立redux，不用寫那麼多code。
               2. reducer：
                  1. single source of truth：所以要從store拿state出來
                  2. state is read only：所以用Object.assign()
                  3. **chages** using pure functions：reducer這個function沒有side effect，只需要return新的state。
                  4. return的`Object.assign({},state,{searchField:action.payload})`中，其中的state是整個store的大object，這一行程式，是對整個store複製一個大object，但是這個大object只有searchField這個attribute不一樣，有變動。
                  5. default是return state，這就是原來的大object。
                  6. redux API對reducer做的事：
                     1. 丟state大object和action進去。
                     2. 接收reducer丟出來的新state，把新的state存在store裡面。
               3. redux：
                  1. redux：
                     1. createStore：用來丟reducer進去
                     2. store可以直接當成props傳給app印出來，裡面是個大object，用getState()可以得到state。
                     3. Provider：但我們通常會把store放到Provider的props裡。
                  2. react-redux：
                     1. 主要用Provider和connect來取代.getState()和.subscribe()，.getState()可以取得要的state，.subscribe()可以取得要訂閱props的component。
                     2. Provider：用來把store裡的state轉成props下去，給所有的子孫。
                     3. connect：
                        1. higher order function：第一個括號run完會回傳另一個function，這個被回傳的function才會把第二個括號run完。
                        2. 主要是說，我要訂閱redux store所有的state變化。
                        3. mapStateToProps和mapDispatchToProps是可以自己命名的，用來告訴connect，我要訂閱什麼state和什麼dispatch。
                        4. 在flux裡面dispatch的意思是，觸發action的東西。
                        5. action就是一個有名稱有值的object，需要被dispatch送去給reducer。
                        6. mapDispatchToProps範例：

                           ```js
                              const mapDispatchToProps = (dispatch) => {
                                 return {
                                    onSearchChange:(event)=>dispatch(setSearchField(event.target.value))
                                 }
                              }
                           ```

                        7. 上面的範例裡：
                           1. mapDispatchToProps是一個object，所以function要return一個object
                           2. 這個object的一個attribute就會放一個function，這個function被觸發的時候會變成一個object。
                           3. dispatch()裡面要放action的object，所以setSearchField就會回傳一個object。
                           4. onSearchChange這個attribute放的就是onSearchChange這個attribute，所以會被event觸發，所以parameter會是event。
                           5. 被觸發後要把action丟進dispatch裡面去，所以要寫一個function叫setSearchField()，只要把event.target.value丟進這個function後，就會把一個action丟出來。
                        8. 一個container component對應一個connect
                  3. 可以只有部份state和redux連接，例如課程第109堂redux connect，就只有先把searchField的state變成redux，robots還是this.state，還是可以用。
                  4. database會遇到一堆read和write的要求，同時又要減少bug和錯誤的發生，所以redux就用類似database處理資料的方式處理state，就可以非常好的整理state。
                  5. middleware：
                     1. thunk：
                        1. 一般function是return一個action object，async function裡面一開始只有dispatch不同的action，沒有return任何東西。
                        2. mapDispatchToProps：
                           1. 例如`onRequestRobots:()=>dispatch(requestRobots())`
                           2. 其中，onRequestRobtos這個props會是一個function，執行的時候會dispatch一個function出去，這個function叫作requestRobots()。
                           3. 而requestRobots()回傳需要是一個function，redux thunk才會等他，所以在action那邊，requestRobots()會需要是一個higher order function才能回傳一個function。
                           4. 這個被回傳的function會被thunk丟dispatch進去parameter，然後這個function要把action放到dispatch裡面，thunk就會把這些action丟給reducer了。
                  6. folder：
                     1. 通常實務上會一個container一個folder，裡面裝了相關的representive component和相關的reducers和actions。
                     2. 這樣會很容易分享這個component，也很容易思考，和找bug。
                     3. 實務上也會有一個API的資料夾，裡面可以有function讓我們可以把url丟進去，就獲得資料，不用一直repeat .then這些。
   4. javascript生態常用的工具：
      1. react router：機乎和react和redux搭擋成標配了
      2. Ramda：非常適合functional programing的js library，另外一套很受歡迎的是lodash。
      3. css：
         1. glamorous
         2. styled components
         3. css modules
      4. gatsby
      5. next.js：server side的app都適合
      6. 可以直接抓建好的component來用：
         1. MaterialUI
         2. semanticUI
      7. reslect：讓使用redux更有效率
      8. redux-saga：redux-thunk的超強版
      9. immutable js：讓state變成immutable
   5. [為什麼我不用redux了](https://www.youtube.com/watch?v=pUlwhe-kmog)：
      1. 原本使用redux在以下三種情形：
         1. 操控離很遠的state：redux
         2. fetch API後把資料存下來：redux-thunk或redux-saga
         3. 處理form validation：redux form
      2. 取而代之：
         1. formik取代redux form：
            1. 因為form data是local的，不需要用到redux的store。
            2. 你每敲打一個字，redux-form都會動一次store，app遇來遇大會產生效能問題。
            3. formik更小。
         2. graphQL：
            1. 所有project都改用graphQL了，不用fetch REST API了，所以不用redux了。
            2. 用appollo去fetch所有的api
               1. fetch data
               2. store data
               3. cache data
            3. 距離很遠的component傳遞state，改用其中之一：
               1. unstated
               2. useContext

## react performance

1. code splitting
2. React dev tool：
   1. 網址結尾加上`?react_perf`可以看到performance
   2. reverse christmas tree：愈深效能愈差
   3. high light updates when components render：
      1. 可以打勾，components有更新的話，可以看到顏色改變。
      2. 小心點一個小按鈕如果整頁都render，可能會影響效能
3. redux可以接近底層的container components，減少大串components的rerender
4. react：
   1. shouldComponentUpdate(nextProps,nextState)
      1. props和nexProps比較，如果不同回傳true，就是如果爸爸傳過來的props有變化就更新。
      2. state和nextState做比較，如果自己內部的state有變化，就回傳true，就更新。
      3. 所以可以分別控制要依哪種規則來更新。
      4. extends PureComponent：
         1. 意思是這個class component只有props有改變時，才會rerender。
         2. 但是這個component用一種叫作shallow comparison of props來決定要不要更新，如果props的結構比較深的object，可能會無法發現props有更新。
         3. shouldComponentUpdate應該比較好用。
      5. 一樣要小心使用，他是有額外的步驟，是會對performance有不良影響。
   2. 改變的state和上個state有關的話，setState裡面用一個function來收到上一個state，並return新的state。
5. [why did you render](https://www.npmjs.com/package/@welldone-software/why-did-you-render)：
   1. 可以幫助檢查rerender
   2. 會在console裡列出哪些props和state沒變化，卻仍然rerender的。
   3. 記得develop mode才用
6. setState()是非同步的：
   1. <https://werehamster.medium.com/beware-react-setstate-is-asynchronous-ce87ef1a9cf3>
   2. <https://vasanthk.gitbooks.io/react-bits/content/patterns/19.async-nature-of-setState.html>
7. memory leaks (第141堂課)：要移除用不到的event listener，不然愈來愈多的event listener，會用到愈來愈多的memory，然後就產生memory leaks，也就是記憶體滿了，然後就crash了。
