# celerec專案流程

1. `npx create-react-app celerec`，其中celerec需要是小寫，接著會出現專案資料夾
2. 進資料夾裡灌tachyons： 
   `npm install tachyons`
3. 弄出辨識畫面時的基本畫面
   1. tachyons的原則：
      1. 內設值是手機版，所以先把手機版位置固定好。
      2. 再看電腦版，把要設的位置class後面加上ns或m或l。
   2. 用一個最大的div，用flex-column，依序由上而下將以下component排列好。
      1. sign in, sign out的瀏覽列
      2. logo
      3. 搜尋列
      4. 圖片框
   3. 修飾各個元件位置和細節：
      1. 瀏覽列讓它靠右，並調一下margin
      2. logo調一下大小、外框粗細、陰影、margin
      3. 搜尋列用兩個元件（input和button）組成，用grid調大小，用flex調置中
      4. 圖片框也是用兩個component組成（預測結果和圖片），用flex-column上下排列
4. 串接clarifai的API：
   1. 讀API文件，google找到特定model的document，直接copy code來用，model有特定的代碼當呼叫時的arguement。
      1. <https://docs.clarifai.com/how-to/api/custom-model-walkthrough#predict-with-the-model>
   2. 找到authentication，copy驗證的程式碼來用。
   3. 找到自己的api key，把自己的api key換上。
   4. 把clarifai範例的相片網址送出，console.log收到的資料結構，找出預測的人名位置
   5. 這裏比較神奇的是，一般fetch得到的response需要用.json()來parse，但是clarifai竟然fetch到的直接就是object了。
5. 把search bar和API串接起來：
   1. 設計input和send鍵的邏輯，雖然有監聽input的event，也有更新state，但是只有在監聽到send的event的時候，才會把input的value抓下來
   2. 把input抓到的value，也就是網址，送出去給API
   3. 監聽到send鍵的event時，把input的value再清空，使用者才可以接著輸入下一個網址。
   4. 最後再把URL送給圖片框，讓圖片框能顯示輸入的相片。
   5. 把步驟四的人名顯示在圖片上方，這樣基本最重要的功能就有了。
   6. 第一版本可以deploy了：
      1. 先git init
      2. `git add .`
      3. `git commit -m 'v0.1'`
      4. `git remote add origin https://github.com/yellowful/celerec.git`
      5. `git push - u origin master` (以後都git push就好了)
      6. 去github上設定github pages設定為gh-pages的branch
      7. vscode裡改package檔，加上github pages的網址，和predeploy和deploy。
      8. 在terminal裡： `npm install --save gh-pages`
      9. 在terminal裡： `npm run deploy`
6. 登入和註冊：
   1. 先弄清楚登錄的邏輯
      1. 只要是已經登錄狀態，瀏覽列就是顯示sign out，而且會顯示下面的主要功能，搜尋列和圖框，可以把這個狀態設成sign in是true。
      2. 所有sign in的按鈕(除了submit按鈕不算)點了，都會出現sign in的component，點了sign out的按鈕，也是該出現sign in的component。這個狀態可以設成sign in是false。
      3. 註冊的component是在register鈕被按的時候才會出現，submit點出後就要消失，所以可以設一個register記住這兩種狀態。
   2. 去tachyon找sign in register的樣板來套用，要把class改成className，把連接的部分都改成onClick。 
   3. 可以設定到這個route的時候，先去fetch一次後端，把睡覺的後端先喚醒，讓體驗更好。
7. 製作人臉方框：
   1. 先試著從clarifai的範例，用chrome的select tool找到框框怎麼寫。
   2. 試著在照片上畫一個疊上去的方框。
   3. 方框是一個div的shadow，最關鍵的點是這個div的position需要是absolute，而他的爸爸div要是relative，這樣方框div才會蓋在爸爸身上。
   4. 不過一個困難點在於，圖片框這個component包含兩個被flex-columnbd包住的element，而被flex-column包住的element的寬度都會變成和螢幕同寬，所以包在相片外面的div的寬度會和相片不同，而抓回來的方框資料依據的是相片的大小，方框依據的是爸爸div的大小，所以會不能用。
   5. 把爸爸div大小弄成和相片一樣大小的方式，就是把爸爸div的flex特性取消掉，取消的方式是，把display設成inline-flex。（這也是inline-flex和flex不同的地方，連css tricks都不知道，以為兩個一樣）
   6. 但是爸爸的div把flex取消之後，會無法置中，只好再弄一個爺爺包住爸爸，爺爺的display用block而position用static，這時候爺爺就會被flex-column限制了，寬度展開和螢幕同寬，爸爸div就神奇的被置中了。
   7. 這時候爸爸div的範圍和相片會重疊，方框用absolute就可以疊在爸爸上，至於要疊在什麼位置，就得先抓image的大小才算的出來。
      1. 先用.getElementById或是.querrySelector都可以，先找到相片。
      2. 屬性是.offsetWidth和.offsetHeight。
   8. 把步驟四抓的資料找出方框的資料，換算成像素，再套用。
   9. 其他修飾
      1. Logo：
          1. 用npm react-tilt的library，套用在logo上。
          2. <https://www.flaticon.com/free-icons/brain>
      2. 背景：react particle js
8. 連接backend：
   1. 先弄好node.js環境。
   2. 用npm安裝:
      1. express：架站用，設定如下
         1. `app.use(express.urlencoded({extended:false}));`
         2. `app.use(express.json());`
      2. cors：讓chrome同意同一個web app連向不同網站。
          `app.use(cors());`
      3. bcrypt-nodejs：讓密碼轉hash的工具。
   3. 先在componentDidLoad先測試看看，能不能連接的上。
   4. backend可以先設定2個預設user，以方便測試連接。
   5. 先把/sign in連上。
      1. 把資料post在body中。資料格式是application/json。
      2. 因為其他component並不在乎sign in這個component是否登入成功，所以fetch就不寫在app裡了，以免app程式太長。
      3. 因為要將輸入的email和password傳到backend，所以要設定this.state來因應改變。
   6. 把/register連上，和sign in非常類似。
   7. 把/image連上，用put的方式進行更新entry（使用人臉辨識的次數）。
   8. 著重api是否連接ok，backend的判斷邏輯和預設user的判斷，不是太重要，因為之後都會被database的功能取代掉。
9. 連接database：
   1. PostgreSQL：先建立兩個table

        ```sql
                users
                id SERIAL PRIMARY KEY
                name VARCHAR(100)
                email TEXT UNIQUE NOT NULL 
                entries BIGINT DEFAULT 0
                joined TIMESTAMP NOT NULL

                login
                id SERIAL PRIMARY KEY
                hash VARCHAR(100) NOT NULL 
                email TEXT UNIQUE NOT NULL
        ```

   2. 在backend用KNEX連接database
       1. npm設定：
           1. 網址：<http://knexjs.org/#Installation-node>
           2. 安裝：

                 ```bash
                    npm install knex
                    npm install pg
                 ```

           3. 設定：
               1. require的地方，client要設為pg
               2. server送出給postgresql的connection
                   1. user要設成database的創建者
                   2. 要有password，即使是空字串也沒關係
   3. 用postman送'/signin'、‘/register’、'/image'、'/:id'等給backend，backend把request透過KNEX送去database。
       1. /register：
           1. 要用transaction，把frontend的request存到兩個users和login兩個table中。
                用法：

                ```javascript
                    db.transaction(trx => {
                        trx.insert(...)
                        .into(table_name1)
                        .returning('column1')
                        .then(column1 => {
                        return trx.insert(...)
                        .into(table_name2)
                        .
                        })
                        .then(trx.commit)
                        .catch(trx.rollback)
                    });
                ```

           2. 其中password的部分要先用同步的方式bcrypt.hashSync做好hash
       2. /signin：
           1. frontend收到email去調出database的login中的hash，然後用同步的方式bcrypt.compareSync確認密碼是不是相同
           2. 相同的話，要用密碼去調出database的users的所有資料，然後把那個email的user資料送去前端。
       3. /image：
           1. 收前端的user資科，用increment去增加那個email對應的entry。
           2. 叫database回傳increment後的entry，然後把更新後的user再回傳給frontend
   4. component的修改：
       1. register的component，判斷response的是object，就將response載入現在使用者資料中。
       2. signin的component，判斷response是object，就將response載入現在使用者資料中。
       3. searchbar的component，send之後，要將response，也就是更新使用次數後的使用這資料，載入現在使用者資料中。
       4. 更新使用次數的function是先傳目前使用者給後端，後端的KNEXT用increment去將database加1，然後再把database更新後的使用者傳給frontend，這時候frontend要整個user的object的state一起更新，不要只更新裡面使用次數的properties，因為語法會超級麻煩。
10. 這樣前後端就連接起來了，記得code先commit起來。
11. code review
    1. frontend:
        1. 把this.state拷貝成initial state，在sign out的狀態下，需要先恢復成initial state，initial state要放在global。
        2. 所有.then最好都要有.catch，沒收到promise時，可以知道發生了什麼事。
        3. 因為signin和register共用大部分元件，所以把兩個component整合成一個新的component：
            1. 主要用register的component為主。
            2. 爸爸component app.js就不用判斷是不是在register或是signin的狀態，少了一層if，這個判斷就丟給這個新的component內部去做。
            3. parents component裡刪掉sign in的submit觸發的function，因為已經沒有signin的component了。
            4. 在做signin的時候，this.state.name是空字串，看起來怪怪的，但是並不會有任何不良影響，因為傳到後端的只有email和password，沒有name。而且後端可以回傳完整的user object，來供current user載入，裡面就有name。
        4. 一些複雜的component，有好用的部分可以拿出來，當作單獨的可以被重複利用的component。
    2. backend：
        1. dependency injection：讓主程式簡單、clean
            1. 每個end point的內容分別放進獨立的檔案中，利用`require(檔案名稱)、module.exports({function名稱:function名稱})`傳來傳去。
            2. 個別的檔案分別是個別的module，用currying的方式，讓這個mudule回傳一個function，這個function的request和response還沒被fullfill，等於是一個等著接收request和response兩個parameter的function。
            3. 在原來的server的js檔中，各個end point之保留一個function，這個function把dependency的module當成argument傳進去，然後得到一個接收request和response的function，因為還沒被執行，所以不用放reques和response。
        2. 很重要的一點，前後端各自檢查是不是有自己的驗證機制，不可以相信frontend一定會送來正確的資料。
    3. 把api key移去backend：
        1. 在backend建立一個新的end point，裡面copy前端的code。
        2. 在frontend的圖片url送到backend：
            1. 要用JSON.stringify(資料)，來轉成json的字串送出。
            2. 接收的response，要用「資料.json()」來parse。
        3. clarifai傳的是object，所以在backend接收到可以直接用，不要再parse了。
        4. backend要回傳給frontend時，記得要用「.json(資料)」來轉成json傳送。
        5. 到介面中去點reveal config vars，把api key換成：clarifaiApiKey。
        6. 程式碼裡面用`process.env.clarifaiApiKey`來呼叫。
12. deploy到heroku：
    1. 註冊
    2. 讀文件，關於node和cli(command line interface)。
        1. brew install heroku/brew/heroku：安裝command line工具
        2. heroku login：用browser登入heroku帳號
        3. heroku create：遠端開一個app，遠端會自動產生一個git檔。
        4. git push heroku master：上傳到heroku的master
        5. heroku open：用browser連去deploy的網址
        6. keroku logs --tail：顯示遠端記錄command line發生的事。
    3. 修改一些遠端會發生的錯：
        1. npm的script要改掉：
            1. "start": "node fileName.js"：遠端是執行這一行
            2. "start:dev": "nodemon fileName.js"：本地段執行這一行
        2. 連線的地方：
            1. 監聽的port：改成process.env.PORT
            2. data base的url：process.env.DATABASE_URL
    4. database：
        1. 去heroku的data那邊安裝postgresql，把postgre連接到backend app。
        2. heroku addons確認是否裝好
        3. 用command line連上資料庫：
            1. heroku pg:psql
            2. 如果有多個資料庫：heroku pg:psql 資料庫名稱
        4. 確認後端knex連接的程式碼已經改成：
            connectionString: process.env.DATABASE_URL,
            ssl: {
                  rejectUnauthorized: false
            }
        5. command line連上資料庫之後，用sql指令去把相關欄位建立起來。
    5. 把react app當成靜態網頁deploy到heroku：
        1. deploy react app為了避免錯誤，記得先git merge到master。
        2. 先安裝npm serve package：
           npm install serve --s
        3. package.json：
            1. 加這行，讓在local端測試可以用npm run dev：
                  "start:dev": "react-scripts start"
            2. 改這行，讓遠端用："start":"serve -s build"
            3. homepage也可以改掉，沒改掉好像也沒影響。
        4. 在heroku網頁介面中開啟一個新的app
            1. 連接遠端已經存在的專案：heroku git:remote -a 遠端app名稱
            2. 依document的git指令deploy到這個app中
                1. git push heroku master
                2. 如果失敗，可以改用：
                     git push heroku HEAD:master
13. 增加上傳image的按鈕：
    1. 弄一個按鈕把圖片上傳：用input的tag配合file的type來建立upload的按鈕
    2. 顯示在圖片component中：用fileReader.readAsDataURL來建立browser顯示的圖片。
    3. 把檔案傳到後端：用formData.append來把image包成mime的格式傳給後端，
    4. 後端存檔：後端建立一個end point
        1. 灌multer
        2. 存檔：
            1. 設定express靜態網站：`app.use(express.static('public'));`
            2. 設定multer.diskStorage，處理存檔位置，和如何命名的問題。
            3. 設定multer的fileFilter，讓它接受clarifai接受的格式
    5. 把後端存檔的檔名傳到前端
    6. 前端依檔名算出url，依原來的流程把url和臨時相片的檔名傳到後端
    7. 後端把url送給clarifai後，，在得到後端送回的clarifai答案後，把臨時的相片檔砍掉
    8. deploy到heroku後，可以上去看看，是否有刪除成功： heroku run bash -a quiet-retreat-05063
    9. 把按鈕外型由input改成button，比較好看
        1. input的style改成display:'none'
        2. 把input設定`ref={(fileInput)=>{this.fileInput=fileInput}`
        3. 把button設定`onClick={()=>this.fileInput.click()}`
14. 辨識多人：
    1. 把原來要預測的人名和方框都改成array來處理，初始值都設成空陣列。
    2. 在算方框資料的地方，用for loop把回傳的資料都存進陣列裡面，存完了再更新state，要用.concat()或.slice()來更新，才不會發生pass by reference，讓this.state發生不預期的被改變的狀況。
    3. 在render的地方加上curly bracket，然後用map來return dom元件，就可以畫出多個框和人名。
    4. 人名的位置要用techyons的v-top來對齊上方，用chrome的selector來調整margin，使得兩個dom的margin一致高。
    5. 方框號碼要用css的transform: translateY(-100%)，使得文字可以跑到div以外，並可以把back ground設顏色，字設白色，使得字更清楚。
    6. 可以多個判斷，陣列為一個和陣列是多人時，顯示不同的文字和畫面。
    7. 用上面預測人名的方式加上顯示機率的功能。
    8. 機率可以先乘以10000四捨五入後再除以100，就可以得到小數點2位的百分比。
15. pop up video：
    1. 在FormSubmit裡弄一個state來管理youtube影片是否跳出來播放，用兩個button來觸發改變這個state。
    2. 用iframe來播放youtube video
        1. youtube的網址用embeded的。
        2. 網址加上`?autoplay=1&mute=1&enablejsapi=1`才能自動播放。
        3. youtube不支援手機自動播放。
        4. 在react底下，iframe要設title才能跑。
    3. 用一個按鈕，點下去可以出現一個div
        1. 設成absolute
        2. fixed
        3. 放最左上角
        4. 大小設成螢幕大小`windows.innerWidth`和`windows.innerHeight`
    4. 把iframe放上面的div裡面
    5. 關閉鈕：
        1. 弄一個div在上面的div之後
        2. 也要設成absolute，才能蓋在youtube的影片上。
16. 多國語言:
    1. 灌react-intl：
    2. 在app.js裡：
        1. `import {IntlProvider} from 'react-intl';`
        2. 用`<IntlProvider locale={規定的locale碼} messages={對應locale的翻譯JSON檔}>`把App包起來
        3. 把要翻譯的component都`importFormattedMessage`，把要翻譯的字都換成`<FormattedMessage id='send' defaultMessage="Send URL" />`，對應json檔裡的`{"send":"Enviar URL"}`。
        4. 其中要翻譯的json檔通常放在`./src/lang`裡面，要先匯入App內，通過一個變數確認locale後，才放入IntlProvider裡。
        5. 有DOM的話要用value要用以下語法去把DOM的tag取代掉：

            ```javascript
                values={{
                    code:(text)=>
                    <a  href="https://github.com/yellowful/celerec" title="Richard Huang">{text}</a>
                }}
            ```

        6. 偵測browser預設的語言：`navigator.language`，放入state當中。
        7. 用hooks弄一個drop down menu，來改變language的state。
           1. 弄一個unorder list來放選項
           2. css設定list成不顯示bullet
           3. list的position要設成absolute，才不會menu出現時，把其他component往下擠開。
           4. list的postion要設成靠右，才不會按鈕大小隨語言變化時而亂跳。
           5. menu按鈕可以加上三角形圖示的文字，可以少很多文字說明。
           6. 按鈕click就打開或縮起來，list一按就縮起來，並且改變language的state。
        8. 根據state來改變IntlProvider要傳的資料。
        9. Clarifai雖然支援多國語言翻譯，但是不支援人名的翻譯，所以名人辨識只有英文的功能，不用再internationalization了。
17. 相片輪播：
    1. 用react做輪播圖片功能效果會不好，會lag，因為重新render和load圖檔很耗資源。
    2. 用css3耗的是browser的資源，一次把圖全部load進去，靠opacity的變化達到輪播的功能。
    3. 有關動畫，最好多利用css3。
    4. 其中的trick：
       1. child都要設定成absolute，全部疊在一起，靠opacity的變化達到切換圖片的效果。
       2. 因為child的position都是absolute，所以footer會縮上來和圖片重疊，而設vh高度很難調整，因為不responsive。最佳作法就是把其中一個child設成ralative，其他child都設成absolute所以仍然會疊在一起，而且footer會被relative的child推到下面去，達到responsive的效果。
       3. 共8張圖片的話：
          1. 2秒一張就是播一輪共16秒，每兩秒大約就是16秒的13%。
          2. 單個keyframe就設定這13%裡面多久要顯示，多久要半透明，多久要隱藏。
          3. 每一個child讓他們有不同的delay時間開始。
          4. 所以每16秒，每一張照片只跑一次顯示和隱藏，然後就又開始新的16秒。
18. 改redux：
    1. 在App.js加上mapStateToProps和mapDispatchToProps。
    2. 把所有state都放進mapStateToProps，所有的function都轉成mapDispatchToProps。
    3. 用connect把那兩個放進去第一個argument的小括號裡，再放入App進去第二個小括號裡面，然後export。如此connect就會把App的props改成redux可以用的App。
    4. 去actions.js裡面把function都改成return一個type和payload。如果function原本不是單純的設定state，就要用dispatch放入一個type和一個payload。
    5. 如果原本的function裡面有呼叫其他function，就用dispatch來呼叫。
    6. 任何想要儲存在store的type或state，都可以弄一個dispatch來傳。
    7. 把actions export給App.js用。
    8. 弄一個reducer.js，裡面放一個switch用來判斷是什麼type，並且把payload的格式轉成state的格式。
    9. 到index裡面，把相關library引入，把createStore裡面包住reducer和middleware，然後用Provider把App包住，就可以用了。
19. 後端加上截圖功能：
    1. 安裝`capture-website`這個library。
    2. 依照document所寫，async的把req的url放在第一個parameter，把pid和date結合當作第二個parameter當作檔名，然後存檔放在public裡面。
    3. 把檔名傳回client端
    4. client端在送url去後端之前先經過一個判斷的function，這個function會進行check裡面是不是有jpg等關鍵字。如果有的話，就依一般的情況傳到後端；如果沒有的話，就把網址傳給capture這個end point。
    5. 後端會把網站截圖，存在public的資料夾裡面，然後把檔名傳回來。client端會在收到檔名之後，把檔名加上後端的url當成一般的圖片網址傳給後端。
    6. 傳給後端的同時，還要把檔名傳給後端，後端向clarifai fetch完才會跟上傳檔案一樣，刪除檔案。
    7. 把訊息state更新，否則會等很久。
    8. deploy的時候
        1. heroku的app的settings的buildpacks要去加上<https://github.com/jontewks/puppeteer-heroku-buildpack>，因為puppeteer的size太大，這些太大的library應該都有heroku自己的buildpack可以灌。
        2. 另外heroku的環境和mac不同，所以程式碼裡面要加上node sandbox的arguments：

            ```js
                {
                    launchOptions: {
                        args: [
                            '--no-sandbox',
                            '--disable-setuid-sandbox'
                        ]
                    }
                }
            ```

20. 未來還可以再改進或增加的功能：
    1. 用jwt和localstorage再次到訪不用再login
    2. 用PPWA
    3. email verification：
        <https://stackoverflow.com/questions/39092822/how-to-do-confirm-email-address-with-express-node>
    4. 寫test
    5. email改form
    6. reducers分3個