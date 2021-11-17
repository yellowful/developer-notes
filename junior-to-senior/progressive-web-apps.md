# progressive web apps

1. navtive相對於web apps
   1. 優勢：
      1. push notification
      2. 不會有blank screen，因為可以offline執行
      3. 相機，位置感測器等
   2. 缺點：
      1. 佔記憶體
      2. 要下載
2. progressive web apps可以讓web app像native app：
   1. 沒有網址列，全螢幕
   2. 可以offline
   3. 可以push notification
   4. 更新不需要向app store 提交
   5. 執行速度超快
3. <whatwebcando.today>
   1. 不能的：
      1. inter-app sharing
      2. user idle detection
      3. NFC
      4. 測量環境光線
      5. 聯絡人資料
      6. 簡訊
      7. Geofencing：進入某一個特定地點，可以產生什麼功能
      8. 近場感應
   2. 可以的、特殊的：
      1. 藍芽
      2. usb
      3. 序列埠
      4. 行事曆
      5. 振動
      6. 電池狀態
      7. 離線儲存
      8. 動態感測器
      9. 鎖定螢幕旋轉
4. 商業策略：
   1. google提出progressive web apps的想法
   2. apple最慢採用這些標準
   3. 即使apple不採用，只要有一部份人可以用，還是能讓網站得到很多好處，而且網站速度也較快，所以還是值得學會。
   4. 所以create react app預設有progressive web apps。
5. lighthouse可以看到progressive web apps的效能。
6. 補充：
   1. [仍可以向app store提交](http://debuggerdotbreak.judahgabriel.com/2018/04/13/i-built-a-pwa-ad-published-it-in-3-app-stores-heres-what-i-learned/)
   2. [在android和ios上的比較](https://medium.com/@firt/progressive-web-apps-on-ios-are-here-d00430dee3a7)
   3. [頂尖的PWAs](https://appsco.pe/)
7. 用PWAs網址或公司：
   1. twitter、uber、lift、alibaba
   2. offline也可以用：
      1. <https://pokedex.org/>
      2. <https://hnpwa.com/>：有各種hacker news的PWAs
      3. andrei示範chrome把PWAs加入桌面的app，但我試用失敗，我猜現在android已經改了，在網址列就可以直接安裝PWAs
   3. 作法：
   4. [check list](https://web.dev/pwa-checklist/)：依check list一條一條做好，就會變成一個PWAs。
   5. 裡面最重要的事：
      1. https
      2. app manifest
      3. service worker
      4. 其它：push notification還沒到很通用，想做的話可以參考[怎麼做push notification](https://auth0.com/blog/introduction-to-progressive-web-apps-push-notifications-part-3/)
   6. https：
      1. [let's encrypt](https://letsencrypt.org/zh-tw/getting-started/)：免費，documentation很棒
      2. cloudflare：
         1. 也是個CDN
         2. 像netlify一樣[可以放你的網站](https://developers.cloudflare.com/pages/framework-guides/deploy-a-react-application)
         3. 對個人的部落格也是免費
   7. app manifest：
      1. `index.html`裡要放上`<meta name="viewport" content="width=device-width, initial-scale=1">`：
         1. <https://web.dev/viewport/>
         2. 這一行是讓手機能正常顯示的，必要的。
         3. 如果沒有的話，lighthouse會顯示錯誤。
      2. `manifest.json`：
         1. [各個參數的詳細說明](https://web.dev/add-manifest/)
         2. 用來定義這個app在手機上看起來怎麼樣。
         3. 在手機上如何啟動，例如：桌面上。
         4. [realfavicongenerator](https://realfavicongenerator.net/)
            1. 可以產生manifest需要的icon
            2. splash screen：
               1. 讓load之前，不要一片空白
               2. 可以有背景色，有name、icon
               3. 這是為了學native app，因為native app在load的時候，也是會先顯示東西。
            3. `index.html`裡的link的href可能要加上`%PUBLIC_URL%/`才能正常做用
            4. favicon.ico，可以改成png的，也可以自己把大圖.png加進去，寫法相同，只是格式不同。
            5. start_url要從`index.html`改成`.`
   8. service worker：
      1. 是一種瀏覽器在背景執行的script，這個script和web page是分開的。
      2. 主要執行一些不需要web page也不需要和使用者互動的功能。
      3. 相對於service worker的是main thread，專門在處理html、css、javascript等，而service worker是一個background worker。
      4. 有programmable proxy的功能，讓我們可以控制request和response這種事。
      5. 舊版create react app：
         1. create react app裡面的`serviceWorker.js`，`index.js`裡面import之後，要加上一行`ServiceWorker.register();`
         2. `if ('serviceWorker' in navigator)`代表的是，判斷瀏覽器支不支援serviceWorker。
         3. `navigator.serviceWorker.register('service-worker.js')`裡面`service-worker.js`是build的時候，才會在build的版本裡。
         4. 裝`sw-precache`：
            1. 新版create react已經改用workbox，不用灌sw-precache，也不用設定sw-precache-config.json。
            2. 如果有裝`sw-precache`的話，要設定`sw-precache-config.js`，裡面要填入要cache的檔案
      6. 總之，當serviceWorker有註冊的時候，javascript要去request的時候都會先問serviceWorker，serviceWorker再判斷是否需要向後端要東西，或是這東西serviceWorker其實有cache起來，不需要向後端要，其實就像是proxy。
      7. serviceWorker沒有cache到的，將會代替browser向server request。
      8. cache可以在browser的Cache Storage看到。
      9. [參考程式碼](https://github.com/jeffposnick/create-react-pwa/compare/starting-point...pwa)
      10. 新版create-react-app (React 17)：
          1. 舊的專案更新create-react-app之後，要手動去更新serviceWorker.js檔，可以先去新建一個新的create-react-app，拿裡面的檔案來用：
             1. 現在新版的，如果要serviceWorker的話，要建立create-react-app的時候，下這個指令才會出現`npx create-react-app my-app --template cra-template-pwa`
             2. 把`serviceWorker.js`、`serviceWorkerRegistration.js`拷貝過去原有專案。
             3. `index.js`裡serviceWorker的呼叫要抄過去。
             4. `package.json`裡的要把workbox相關的都拷貝過來。
          2. [我自己的練習](https://github.com/yellowful/robofriends-redux)
      11. [gatsby-plugin-offline](https://www.gatsbyjs.com/plugins/gatsby-plugin-offline/)：
          1. workbox：[globPatterns](https://github.com/isaacs/node-glob#glob-primer)：是一種特殊語法，和regex不太一樣。
          2. lighthouse：對某些service worker(包括gatsby的)有問題，會卡住，相關issue在此：
             1. <https://github.com/GoogleChrome/lighthouse/issues/13273>
             2. <https://github.com/gatsbyjs/gatsby/issues/33837>
   9. 小遊戲：chrome藏了一個小遊戲，一但離線連不上網站，會出現一隻小恐龍，按一下空白鍵，會出現一個小孔龍的小遊戲。 
   10. [超多做PWAs的工具](https://progressivetooling.com/)
   11. 心得：
       1. 依照lighthouse去看PWAs還差什麼，一樣一樣的去補足就可以了。
       2. 新版React 17要動的東西愈來愈少。
       3. 實做的時候遇到service worker的route亂導，仔細看，似乎和gatsby的port衝突，改個port就好了。