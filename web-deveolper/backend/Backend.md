Backend
1. 什麼是前端、什麼是後端？
   1. 你去pchome買東西會發生什麼事？pchome會有一台電腦（所以稱做服務端，server），你也會有一台手機（也算一台電腦，稱做客戶端，client），這筆交易要開始的話，需要把兩台電腦連接在一起，也就是server端會傳一個畫面相關的資料給client端，這些畫面怎麼顯示相關的資料，相關資料有html、css、javascript三種檔案組成，寫這三種檔案，專門呈現畫面的就稱為前端工程師。
   2. 你在pchome註冊之後，pchome會需要把你註冊相關資料儲存起來，不然下次你來買的時候，他就不知道你註冊過了。還有很多資料，你買過哪些東西啊，你放在待購的產品啊，之類的。這些資料平常使用時，不需要回傳到你的瀏覽器給你看，但是卻得儲存起來，不管你關機了(client)，或pchome(server端)關機了，都不能忘記。寫程式把這些東西儲存到一個資料庫，並且容易搜尋出來，驗證使用者正確性，怎麼把前端的資料傳給瀏覽器...等等，就是後端工程師的工作。
   3. 這裏比較容易搞混的地方在於：
      1. html、css、javascript是前端負責寫的，因為是關於使用者看得到的網頁相關操作流程，所以屬於前端。但是這幾個檔案要放在那裡，怎麼放，怎麼傳給client卻是server的事，也就是後端負責。
      2. 前端的資料會傳給client的browser，在瀏覽器裡面執行程式，但是平常前端的資料是儲存在server端，而不是儲存在client端的browser中，只有browser要的時候，才傳給browser執行，使用者用完關掉browser，client端就不再有這幾個前端資料了。
   4. 把pchome想像成一間實體店面，你走近店裏消費，店就是服務端(server端)，你就是客戶端(client端)，而前來招呼你的鮮肉或正妹業務就是前端，前端要跟你介紹產品，幫助你走完結帳流程。假如你跟業務買了一隻手電筒，結完帳之後，會有以下消費者看不到的事，都是後端在處理處理。
      1. 上架人員把手電筒補貨到架上
      2. 會有人通知廠商要進貨
      3. 會有人把這隻交易的相關資料都記錄下來
      4. 指派其他業務接洽新客戶
   5. 業務(front-end)雖然要面對客戶(client)，甚至有時候需要出門拜訪客戶，但是平常仍是店裏的一部分，常駐在店裏，領店裏的薪水。
2. 為什麼要分前後端？
   1. javascript是傳原始碼到使用者的瀏覽器，不是機器碼，所以所有使用者都看得到瀏覽器這一端怎麼運作的，但有些程式碼邏輯或個資不願給瀏覽器這端看到的，都放後端，就不會被直接看到了。
   2. 一些不會讓使用者操作只會讓管理者操作的功能就放在後端。
   3. 同一套商業邏輯之下，要應用在不同的平台的時候，例如瀏覽器、ios app、android app、apple tv app、software，相同的邏輯可以放在後端，那就可以共用。其他瀏覽器顯示相關的，交給前端或app端就好了。
3. backend大致有兩種：
   1. Apache Server：跑一些基本的http協定，有一些基本的網頁功能，用PHP跑一些server的邏輯，向mySQL資料庫要求儲存客戶資料。
   2. Node Server：可以透過Ajax,JSON等處理server和browser之間的溝通，可以跑Web App，資料量小、更快、效率高，可以做出更多功能。用和JavaScript相同語法的Node.js處理Server端的邏輯，並且和PostgreSQL和MongoDB溝通。
4. 比較大的系統，會用多台Node Server處理request；再更大，就會在backend前面放balance的server；再更大Database會有多台備份。
5. API：
   1. 免費：
      1. 亂數產生機器人：https://robohash.org/
      2. github的API：https://api.github.com/
      3. W3C的API：https://www.w3schools.com/xml/simple.xml
      4. 星際大戰：https://swapi.co/
      5. 天氣：https://openweathermap.org/api?ref=public-apis
      6. telegram：https://core.telegram.org/api
      7. andrei介紹：
         1. [numbers api](http://numbersapi.com/#42)
         2. [chuk norris](https://api.chucknorris.io/)：笑話
         3. [pokemon](https://pokeapi.co/)
   2. 付費：
      1. facebook
      2. google maps
      3. 金流：https://stripe.com/docs/api/balance/balance_object
      4. 傳簡訊：https://www.twilio.com/docs/usage/api
   3. 其他：
      1. https://public-apis.xyz/
      2. https://apilist.fun/
      3. https://github.com/hsiangfeng/APIList
6. Node.js+Express.js：
   1. Node特性：
      1. 用在server端，就是後端的意思，也就是需要用到幫使用者儲存資料的時候，就需要用檔案或資料庫的方式儲存與一台電腦上，這就是後端在做的事，也是前端無法做到的事。
      2. node是2009被寫出來的，非常的新，相當多大公司使用，例如IBM、Micorsoft、LinkedIn、Rakuten、Paypal
      3. node最大的優勢是非阻塞(Non-block)，一般的server端的語言有一個很大的缺點，例如PHP，開thread很麻煩，很花效能。而node就是為了改進這個缺點而生的，他解決的方式是事件驅動，所以可以輕易開超多callback function，也不會阻塞。
      4. node是用google V8引擎，速度很快，而且可以用JS語法來寫。
      5. google花超多資源在V8引擎上，所以相對另一種熱門的後端語言python，node還是快很多，甚至在IoT領域，如果你的project需要做realtime，都可以考慮用nodeJS。當然要更快的化，就是用C++了。
      6. 相對於python，nodeJS或Javascript可以有新的語法，因為有babel專案可以編譯，並加快速度，但是python目前沒有類似babel的專案可以用，所以python 3和2相容性就很差。
      7. 有意思的是，facebook雖然仍然用php寫後端，但是他們曾經找一群高手，內部弄了一個編譯器，可以把php編譯成C++，用高效的核心跑，所以兼顧留住php的人才，和維持效能。
      8. 因為不是給瀏覽器用的，所以沒有window和document的物件了,但是有global和process物件。
      9. 可以去johnny-five玩玩IoT，缺點是arduino無法把node程式直接燒入，需要一直插著usb才能跑，所以最好搭配著。
      10. node的缺點是：
          1. 雖然它有V8引擎，速度很快，但是他是單執行緒，如果某使用者執行了一個大量運算的程式，其他使用者將需要等待那個運算完成。
          2. <https://medium.com/hobo-engineer/ricky%E7%AD%86%E8%A8%98-django-vs-express-2e30e61aa23d>
   2. 常用指令：
      1. `node -v`：查看版本
      2. 更新：
         1. 更新node直接去官網下載，會一同更新npm
         2. 如果只要更新npm：`npm install -g npm@latest`
         3. npm 7以上，如果install package的時候看到這樣的錯誤`npm ERR! ERESOLVE unable to resolve dependency tree`，解決方式加上：`--legacy-peer-deps`
      3. node：類似python，直接進入ide的script模式，可以直接在命令列寫javascript
      4. process.exit()
   3. Modules：
      1. import和export是ECMAS 6的語法，node還沒支援這兩個指令。
      2. export改成module.exports，module是一個物件，module.exports也是一個物件，key是要exports出去的名稱，value是module裡最後要被exports出去的變數(結果)。
      3. import改成require('路徑檔名')，會得到檔案的路徑，然後「.名稱」才能拿到之前匯出的變數。
      4. import和export是非同步載入，瀏覽器是在網路環境，所以不能卡在某個不能載入的module上，而node是從硬碟載入，所以沒有這樣的問題，所以require和module.exports是同步載入（一步一步）。
      5. commonJS就是nodeJS的匯入module方式。
      6. Webpack是比較後來發展出來的，他讓瀏覽器支援commonJS，也就支援require()的匯入module的方式了，原理和babel一樣，就是把新版的標準編譯成舊版的javascript。
      7. 最常用的3種Module：
         1. 'fs'：和檔案處理有關
         2. 'http'：和網路有關
            1. 先用require('http')建立一個http網路物件，再用.createServer來建立一個server物件，並用.listen來設定聽取的port。
            2. .createServer()時，argument是一個arrow function，這個arrow function有request和response兩個arguement可以用，而response有.setHeader和.end兩個method可以用。
            3. 所以架站的時候，要先把response的.setHeader設定成browser要get的格式：
               1. 是text/html
               2. 或是application/json
            4. 再把response的.end設定好，其實這就是內容，如果是html，直接傳string格式，如果是json，也需要用JSON.stringify()轉成文字才能傳。
            5. 比express初階難用。
         3. 'nodemon'：
            1. 是用npm安裝的。
            2. 安裝前npm init -y，以記錄安裝版本。
            3. 安裝時加上--save-dev的參數，代表開發者自己用在自己開發環境用的library，並不是必要library。別人npm install時，就不會裝這個了。
            4. 可以即時看到script run的結果，不用每次打指令。
            5. nodemon script.js，他就會開始監聽script.js了。
   4. express：
      1. 先得npm install express，注意，package.json裡的name和灌express的資料夾不可以叫express，否則要改名。
      2. 和http一樣的地方在於需要先require()，再來讓require出來的物件加括號執行，並指定為物件，而且也需要用.listen來監聽port。
      3. express比http不同的地方在於：
         1. 不用記得createServer保留字，直接括號就好。
         2. 直接用.get、.post、.push、.delete等method來呼叫。
         3. 可以直接router。
         4. res.send可以不用.setHeader()，不用管content-type是text/html或是application/json，他自己會判斷。
      4. 可以用.use()在.get、.push...之前解碼，裡面的參數通常是放exprss物件的method。
      5. static server：.use(express.static(__dirname + '資料夾'))，就可以了。
   5. RESTful API：
      1. 對某個router的位置做get、push、post、delete，把四個動作當作動詞，router當作名詞，就可以了解這種文法了。
      2. 每個request進來，server就有足夠的資訊處理，而不用記得是誰傳過來的。
      3. request包含有四個attribute可以用：換句話說，就是browser在向server送出request的時候，可以在這四種方式中裝載資訊。(section24, 280堂)
         1. querry：也就是URL加上?後面的參數，在postman裡可以在params填入。
         2. body：通常是post的時候用的，例如summit一個form，在postman裡body那一頁可以設定。
            1. postman設urlencoded的話，server端也要設.use(express.urlencoded({extended:false}))。
            2. postman設raw的JSON時，server端也要設.use(express.jason())。
         3. headers：postman裡可以在Headers那一頁設定。
         4. params：當server沒有符合的router，而其中一個router有冒號，這時候冒號後就是params，server就會把browser的router當成這個params的值。
      4. router後面對應到才會做指定的arrow function裡的事。
      5. 更複雜的API，可以改用GraphQL，功能比RESTful更強大。
   6. fs檔案module：
      1. 最基本的是四個動作：readFile、apendFile、writeFile、unlink
      2. argument都會有一個是function，這個function都會有一個err讓你可以處理error。
      3. 文字資料要.toString()才會以文字方式處理，裡面可以加上參數'ascii'或'utf8'。
      4. .readFile有2個parameter，第二個parameter是一個function，function的第二個parameter是讀到的data。
      5. .apendFile有3個parameter，其中第二個是要加入的內容，writeFile和apendFile的用法相同。
      6. .apendFile的第3個
      7. sync async：
         1. 那四個動作，後面加上Sync就會變同步了，否則預設async非同步。
         2. 除了readFile和readFileSync之外，其他的function都是有err的時候才會run。
         3. 同一個檔案被開啟和寫入，或被不同程序開啟，會error。
      8. 主要是關於輸入和輸出，不見得一定是檔案。
      9. 可以用console.time()和console.timeEnd()來算程式執行時間。
   7. 建立一個具有RESTful API的server：
      1. 注意postman裡的value都是string，注意number和string的轉換。
      2. get,post,put,delete裡面的第一個argument是要被連接的end point，第二個argument是個function，而這個function只有兩個參數，一個是request，另一個是response
      3. router '/signin'的建立：分辨帳號和密碼是否符合
         1. 先在server.js裡建個小資料庫
         2. 讓postman post object過來，要注意的是postman裡面key和value都要是用雙引號括住的字串。
         3. 用.use將string parse成objcet。
         4. 然後判斷密碼是不是符合，如果符合就回傳一個字串回去。記得要用.json傳。
      4. router '/register'的建立：把輸入的資料，存入資料庫
         1. 把資料push進臨時資料庫中。
         2. 臨時資料庫的array的.length-1就是最後一個。
      5. router '/profile/:id'的建立：看profile裡找不找的到id
      6. router '/profile/image'的建立：用來計算使用者用了多少次服務
         1. 因為是更新某個人的資料，所以是.put()。
      7. bcrypt-nodejs：
         1. 雖然這個版本已經不maintain了，但這個版本跟mac相容，比較不會出問題。
         2. bcrypt特別的是不同次hash，會得到不同的hash，但是卻可以.compare()判斷是不是相同的password，安全性很高。(這叫做加鹽)
         3. 但是.compare()不是synchronize的function，true或false的判斷值如果傳到function外面來的話，會來不及用，Andrei還沒教，他的判斷都還不是用hash來做，之後看他怎麼解決。
            1. 後來andrei用的是.compareSync()。
         4. 在網路上看到bcrypt可能無法躲過GPU平行運算或是旁道攻擊，更佳的hash套件是argon2id。
         5. 密碼hash移轉：<https://ithelp.ithome.com.tw/articles/10213964>
   8. multer：
      1. 是一個nodeJS和expressJS官方指定的上傳檔案套件。<https://medium.com/@jedy05097952/node-js-%E5%AF%A6%E4%BD%9C-mysql-%E5%9C%96%E7%89%87%E4%B8%8A%E5%82%B3-f46298179329>
      2. 存檔：
         1. 設定express靜態網站：`app.use(express.static('public'));`
         2. 設定multer.diskStorage，處理存檔位置，和如何命名的問題。
         3. 設定multer的fileFilter，讓它接受clarifai接受的格式。
      3. 檔案格式：Blob轉BASE64用`.toString('base64')`就好了。
   9.  連接front-end和back-end：
      4. 解決port衝突
      5. chrome不允許不同server之間傳資料，所以要用cors，先npm install之後在.use()中用：.use(cors())。
      6. 在REACT app上，把sign in的component資料fetch過去，fetch裡的第二個parameter是一個物件，裡面可以有：method、header、body等屬性。
         1. method：字串
         2. header：object(裡面是'content-type':'application/json')
         3. body：字串(object的話就要用JASON.stringify())
      7. 值得注意的是fetch的content-type決定是'application/json'的話，那麼server端送回的response一定要是.json()，如果.send()，client端會對不起來，錯誤。
      8. catch到的error不要直接傳到前端去，可能洩漏資訊。
      9. debug：
         1. 善用chrome的networks來看front-end到底fetch出什麼，server丟出什麼response。
         2. 善用console.log：
            1. front-end：browser裡
            2. back-end：terminal裡
   10. node環境下設定預設參數(environmental variable)：
      10. 可以方便讓開發時和在部署到heroku上有不同的參數。
      11. environmental variable通常用大寫。
      12. fish指令env，可以看到目前環境變數的值。
      13. variable node server.js：
          1.  例如bash底下的用法：`PORT=3000 node server.js`，在server.js中process.env.POT就會是3000。
          2.  fish底下的用法：
              1.  `env PORT=3000 node server.js`或`env PORT=3000 npm run start:dev`
              2.  set -x key value
              3.  很多參數的話：
                  function setTESTENV
                     set -g -x BROKER_IP '10.14.16.216'
                     set -g -x USERNAME 'foo'
                     set -g -x USERPASS 'bar'
                  end 
              4.  刪除：set -e variable
7. deploy：(主要觀念是設定網址、PORT、參數，讓他們指向正確的地方。)
   1. hostgater: 適合放靜態網站，是用apache server供人放檔案。
   2. heroku：
      1. 註冊
      2. 讀文件，關於node和cli(command line interface)。
         1.  brew install heroku/brew/heroku：安裝command line工具
         2.  heroku login：登入heroku帳號
         3.  遠端的app：
             1.  heroku create：遠端開一個app，遠端會自動產生一個git檔。
             2.  如果遠端本來就已經存在的專案，連接遠端的app：
                  heroku git:remote -a Remote_App_Name
         4.  git push heroku master
           1.  git remote -v可以看遠端的名稱
           2.  原本heroku內設遠端都叫heroku，你可以改名
           3.  遠端新專案可以改名：git remote rename heroku newname
           4.  但是不需要改名，就像github的remote都一樣稱為origin
           5.  假如連接的遠端已經存在一個專案，可以把本地段關聯到遠端的專案：
                 heroku git:remote -a Remote_App_Name
         5.  開啟遠端的app，看看是否部署成功：
              heroku open
         6.  如果沒反應，可能部署失敗，可以查看遠端記錄的訊息：
              keroku logs --tail
      3. code參數設定：
         1. 連線的地方：
            1. code監聽的port：改成process.env.PORT
            2. code裡data base的url：改成process.env.DATABASE_URL
         2. package.json：
            1. "start": "node fileName.js"：修改，遠端是執行這一行
            2. "start:dev": "nodemon fileName.js"：增加，本地段執行這一行
      4. 其他指令：
         1. 看網址、設定：
            1. heroku config
         2. deploy到heroku後，可以上去看看檔案狀態： `heroku run bash -a quiet-retreat-05063`，其中quiet-retreat-05063是app的名稱
8. 安全性：
   1. 自己架站時，為了防止DDos攻擊，可以用cloudfare的服務。
   2. [csrf](https://blog.techbridge.cc/2017/02/25/csrf-introduction/?fbclid=IwAR0fkuEWoZuo2AidW-alU2eC_yXiE-inIBMSAZvcXgLgtLkhuVmnhja95Ag)：
      1. cors能解決瀏覽器的同源政策的問題，而同源政策又是什麼呢？它主要要預防的駭客攻擊是類似xss、csrf或類似的攻擊。
      2. csrf(Cross Site Request Forgery)是因為**不同瀏覽視窗具有相同的cookie**，所以駭客網站裡面可以藏銀行轉帳到駭客帳戶的連結，假如你剛好**別的視窗開著銀行帳戶的網頁**，就可能在駭客的視窗執行到駭客的連結，而對銀行帳戶進行轉帳。因為銀行網站認cookie，確認是你cookie相同沒錯，就會真得執行轉帳。
   3. [XSS](https://medium.com/hannah-lin/%E5%BE%9E%E6%94%BB%E6%93%8A%E8%87%AA%E5%B7%B1%E7%B6%B2%E7%AB%99%E5%AD%B8-xss-cross-site-scripting-%E5%8E%9F%E7%90%86%E7%AF%87-fec3d1864e42)：
      1. 看起來是你的網站被注入了惡意程式碼
      2. 或是被分享的網址可以讓你的網址被注入惡意程式碼
   4. 呼叫後端找資料時，最好用email，不要用id，因為id號碼連續的話，駭客可能用postman去亂猜id，取得自己以外別人的資料。