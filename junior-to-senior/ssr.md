# SPA vs SSR

1. [第一個網站](http://info.cern.ch/)
2. [SPA](https://www.producthunt.com/)，試點很多連結nav bar都沒有變，不需要全頁載入。
3. spa的framework：angular one, backbone(前端MVC), react。
4. spa有兩個問題：
   1. 一開始需要送一堆js給client，app會比較晚load。
   2. SEO不佳
      1. google robot只會看到一個div。
      2. 而且google載入div之後，ajax下載其它的東西，還有render時間較久，google記錄這個網站內容時，可能內容都還沒render出來。
      3. 順利的話google可能等20秒，做snapshot，但是這個功能不一定有做用，而且只有google(chrome)才有這功能。
      4. 其它bing, yahoo, 百度可能又不同的機制。
5. CSR的過程：
   1. 下載未render的html，裡面有id是root的DOM，畫面是空的。
   2. 下載js，畫面仍是空的。
   3. 執行js，畫面仍是空的。
   4. render完成，畫面才能互動。
6. CSR的優點是一但render完成，點連結取得新的view的時候，都不需要向後端做request重新載入了，直接在browser裡重新render出新的view，只需要更新需要更新的一小部份，速度較快(faster response)。
7. SSR的過程：
   1. 直接就下載render過的html，然後才下載js，所以顯示速度會比CSR快多了。
   2. 但是使用者仍然需要等js下載和載入完成後，才能互動。
8. 把robot friends轉成SSR：
   1. 用`react-dom/server`的`renderToString`，可以把RobofriendsApp先render成html，然後我們再用express送往前端。
   2. 用`fs`把`robots.json`的資料讀進來，然後當成props傳給RobofriendsApp。
   3. 但是注意
      1. 這裡是已經先把robots資料載進去然後render出來。
      2. client side和server side都需要react。
         1. server side：`ReactDOMServer.renderToString()`
         2. client side：影片7分鐘的地方解釋，`ReactDOM.hydrate()`和呼叫`ReactDOM.render()`很像，只是他把server render完成而丟過來的markup(DOM)純文字留著，但是貼上event handler，讓網頁可以互動。
         3. 不管vue或是angular，所有SPA都有都是這樣的過程，因為在browser環境底下才有window object讓我們可以把events listener貼上這些node。
      3. 新版的用`ReactDOMServer.renderToNodeStream()`，速度較快。
      4. 後端也需要react，因為我們需要virtual DOM，在server端就幫我們mounting那些components。
9. SSR的原理和過程就差不多是這樣，但是因為react是為了CSR而設計，所以SSR有些edge cases不好處理，其中`Next.js`比較少設定，所以是個SSR的好工具。
10. SSR的優缺點：
    1. 優點：
       1. initial page load快速。
       2. SEO好。
       3. 內容網站很適合static site。
    2. 缺點：
       1. 點別頁需要full page reloads，一次就要跟後端request一整頁的資料。
       2. 即使每一頁只有一點點不同，仍然需要全頁載入，render較慢。
       3. server要負責render，負擔較大，如果某網頁需要等server render很久，這可能造成客戶不想交易了。
       4. 對開發者來說也多了一層complexity，要多花一點時間才能讓SSR能跑。
11. React的SSR套件：
    1. Gatsby.js：
       1. ssg
       2. react官網就用gatsby.js搭建而成
       3. 靠提供platform讓你架站來營利
       4. gatsby cloud希望取代wordpress。
    2. Next.js：
       1. 設定少
       2. 已經有code splitting、performance optimization、SEO、hot module loading、prefetching…等。
       3. 已經屬於vercel這間公司的了。
       4. 靠提供platform讓你架站來營利。
12. static vs ssr vs csr：開發者要決定js要在哪邊render畫面
    1. static：
       1. 在開發端的電腦上就把東西render好了，才上傳server
       2. 不用用到server的算力
    2. ssr：
       1. 完全靠server的算力把畫面render出來，然後把畫面傳給client。
       2. client不用花太多算力。
    3. csr：
       1. 讓js在client side執行。
       2. 不用花server的算力，花使用者的chrome算力。
13. Next.js
    1. 課程第245課是next.js 5的版本，現在已經是第12版了。
    2. `npx create-next-app nextjs-blog`，以下舊方法可以忽略了：
       1. `npm init -y`
       2. `npm install next react react-dom`
    3. 功能：
       1. 已包含webpack
       2. 已包含less, scss
       3. 支援type script
    4. `npm start`的script改成`"next"`
    5. `npm start`之後，會自動產生`.next`的資料夾：bundles都自動放到dist資料夾裡。
    6. pages資料夾的用法和gatsby.js是一樣的。
    7. 用chrome打開看"view page source"，會看到render完的結果，body裡面會有一堆html的tag，然而SPA打開看"view page source"，body裡面的html tag只會看到一個div，然後id是root。
    8. router：
       1. react-router用在ssr有點麻煩。
       2. next用Link來route，非常方便，語法和gatsby.js很像，next的Link的attribute不是用`to`而是維持用`href`，但是next的Link一定要包住一個tag，不然直接包字。
       3. [a tag和Link的差別](https://medium.com/@wilbo/server-side-vs-client-side-routing-71d710e9227f)
          1. a tag：用到的是server side的render，因為會向後端做request，頁面會全部下載和render，會看到flash。
          2. Link：是用SPA的方式來route，用JS來render，不需要向後端做request全部的檔案，只會request ajax的部份，只會下載和render必要的部份。
    9. deploy：`now`的指令就會自動deploy了。
    10. [google機械人如何耙你的網站](https://developers.google.com/search/docs/advanced/javascript/javascript-seo-basics)：
        1. [seo影片](https://www.youtube.com/watch?v=nwGY-9lwTF4)：
           1. rendering是用api的data把templates發佈出來的過程。
           2. 連到別的網頁一定要用a tag用href連過去，而不要用其它div或span之類的tag或event handler去連，機器人才耙得到。
           3. 假如是用javascript來處理兩個網頁之間的轉換，用History API像一般的URL來處理，而不要用hash-based(井字號)的routing技術來處理，爬蟲會忽略這種方式。
           4. `<meta name="robots" content="noindex">`會阻止google爬蟲抓內容。
14. progressive rendering：
    1. 運用code splitting的技術。
    2. 先render一部份，之後再背景或是使用者互動到的功能再去render出來。
    3. 這過程，使用者一開始就看到畫面了，可能不會注意到背景仍在render東西。
    4. 去react的官網reload，看networks的tool，紅線是下載完成了，之後仍有東西在背景下載。
    5. 很多公司用這種技巧：
       1. Flipkart：速度感覺真得很快。
       2. Ali Express：速度的確也蠻快的。
    6. 他們會serving一個shell或是一個skeleton，讓前端好像立即看到畫面一樣，但是事實上前端仍然在背景下載很多東西。
15. [prerender.io](https://prerender.io/)：他們的server可以儲存你render好網頁，讓爬蟲可以抓。
16. 重視SEO的amazon、walmart等都用ssr，他們很適合。
17. 其它有的網站可能會用progressive rendering，來加速顯視的速度。

## dynamic apps

1. fetch API：
   1. 可以在server side就先request：[getInitialProps已經不用了](https://nextjs.org/docs/api-reference/data-fetching/get-initial-props)
      1. page或component去定義`.getInitialProps`來取得資料，這個`.getInitialProps`會定義成一個function，然後return的值就是要傳進這個component的props。 
      2. 用jsonplaceholder練習render一列名字出來。
      3. 在`.getInitialProps`裡面`consolog.log`抓回來的資料，我們用Link連來這個page和直接對這個page做reload，會發現有不同的現象。
         1. 用Link連來這個page或是重逛這個網頁：console.log會發生在瀏覽器。
         2. 直接對這個page做hard reload：console.log會發生在node環境下。
         3. 不管以上哪種方式，都可以看到render完成的畫面。
   2. 可以在client side才做request：如上面提到的，用Link來連這個page。
2. 練習任務：
   1. 把robotfriend改成next。
   2. 把每一個robot都連結到一個robot的page。
