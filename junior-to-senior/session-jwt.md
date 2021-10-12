# sessions+JWT

1. cookie：
   1. 過程：
      1. server確定密碼正確後，會丟一個cookie給browser，並在server端儲存一個cookie和一個session，用來記錄browser的state，如果logout就把這個session砍掉。
      2. session開啟後，往後browser如果要跟server要資料，就會丟cookies出來給server，server就會檢查cookie是不是正確，如果是正確就會去找出這個user的session的資料，把資料丟給browser。
      3. 如果browser點logout，server就會把session關掉，browser就算還有cookie，也要不到資料了。
   2. 缺點：
      1. server需要記錄browser的state，不容易擴充。
      2. cookie有大小限制，不能包太大的payload，後端資料要再向database來要。
2. JSON Web Token：
   1. 現代網站用的方式。
   2. 過程：
      1. 和cookie很像的是，server會丟資料給browser，這個資料就是token。browser會把資料存在session storage或是global storage當中。
      2. 當使用者丟資料給server的時候，是丟token給server，server驗證token的正確性之後，不需要去找對應的cookie和session，不用動用資料庫，就可以把資料丟回給browser。
      3. logout的時候，client端直接殺掉session storage就好了，server端不用去管session，不用花資料庫記錄cookie和session。
   3. 優點：
      1. 比較單純，server端不用記錄browser的state(session)。
      2. 由於server是stateless，所以可以擴展多台server，不會有session衝突的問題。
      3. 可適用各種不同的API，後端可以多種不用的API都共用。
      4. 也可適用於手機app，像是cookie就很難在手機app上使用。
   4. 缺點：
      1. 資料量較大，每次傳輸都會傳遞整個token，token常是一大包JSON，裡面可能包含了使用者profile之類的訊息。
      2. token若存儲敏感資訊，被駭客取得，將較危險。
   5. cookies vs tokens相關文章：(還沒讀過)
      1. <https://dzone.com/articles/cookies-vs-tokens-the-definitive-guide>
      2. <https://stackoverflow.com/questions/17000835/token-authentication-vs-cookies>
      3. <https://scotch.io/bar-talk/why-jwts-suck-as-session-tokens>
      4. <https://auth0.com/blog/refresh-tokens-what-are-they-and-when-to-use-them/>
      5. <https://security.stackexchange.com/questions/108662/why-is-bearer-required-before-the-token-in-authorization-header-in-a-http-re>
   6. Andrei的課是用兩種混用的方式來做authentication。
      1. server端會用redis存儲對應的token，然後比對之後才回傳資料，一般做法不儲存token。
      2. redis是儲存在記憶體裡面，而且可以擴張，所以速度很快。
      3. token decode之後就直接是JSON資料了。
      4. 這個token是使用者資料和secrete一起hash的。
   7. portal：
      1. 讓modal和app之間有props可以互傳，而且不受其它components結構限制，可以讓其它component可以出現在背景。
      2. html裡面，除了root還要加一個modal-root的node
      3. [html有一些特殊符號碼可以用](https://www.w3schools.com/charsets/ref_html_entities_4.asp)，例如：x符號<https://www.w3schools.com/charsets/ref_html_entities_4.asp>
   8. jsonwebtoken套件：<https://www.npmjs.com/package/jsonwebtoken>
      1. 第352堂課 sending the JWT Token
         1. 在node裡面，signin的end point要做authentication。authentication判斷如果失敗的話，表示還沒登入，應該要做sign in。如果判斷通過的話，就回傳id，讓前端去要求profile的end point。
         2. 如果sign in比對帳密成功的話，就回傳user id和token就好了，不要回傳整個user給前端，前端再用user id和token向後端要整個user，這樣每個end point就可以單獨只做一件事情，很清楚的定義end point。
         3. 用jwt.sign()來產生token，token用email和一個secrete來產生，還不會太敏感，千萬別放使用者密碼在裡面。
            1. secrete是server端自定一個不能讓別人知道的東西，所有人都用一樣的secrete。
      2. redis client：
         1. 用來快取session，不用一直去煩postgresql
         2. 將redis server裝進docker裡面
         3. docker不需要管local host的網址，只需要設定port就好了。
         4. docker可以讓backend、postgreSQL、redis在下一個指令下，全部啟動
         5. 可以下一個指令，就可以透過docker，去到redis的shell裡面。
         6. 用setToken去把id和token設進redis的key和value。
         7. 用Promise.resolve()和Promise.reject()去把setToken改成Promise。
         8. 後端用以下程式碼下令redis比對token，authorization是前端傳過來的token，.get()，可以得到key(也就是id)是否存在，假設是error就告訴前端，未授權。

            ```js
            redisClient.get(authorization,(err,reply)=>{
               if(err||!reply){
                  return res.status(400).json('Unauthorized')
               }
            })
            ```

         9. 我猜流量小的話，token可以不用redis來做：
            1. 做法1：token存postgreSQL，後端比對id的token是否正確。
            2. 做法2：後端用id呼叫postgreSQL要email，並呼叫jwt比對token，比較是否正確。
            3. 做法3：一般常見做法應該是這樣，直接呼叫jwt比對token裡面藏的secrete是不是正確，正確的話，就直接取用postgreSQL進行後續處理。
               1. 因為token是server端給client端的，所以雖然看起來這樣做的風險非常大，因為每個人的token裡面的secrete相同，但是client端並無法解出secrete是什麼，所以被駭的機會不是非常大。
      3. 安全性：
         1. JWT不能被取得，所以使用http的話有風險，因為無法防範中間人攻擊，要使用https可以利用https來預防被攻擊。
         2. JWT訊息是可以被逆推回來的，所以如果client端的電腦被取得了，可以得知JWT，就可以連上server取得這一位使用者的更進一步的資料。
         3. 也因JWT訊息是可以被逆推回來，所以裡面不能藏私密資料，以免server被駭的時候私密資料外洩。
         4. token加密可以選擇用非對稱的方式，server產生公私鑰，client端回傳的token可以被server端的公鑰解開驗證，只是速度稍慢，可以用快取解決。<https://ithelp.ithome.com.tw/articles/10231212>
            1. 我的理解是server端的公鑰也沒有外公開？只是用來驗證client端回傳的token，並解開得到id或email用來快速向database要資料。
   9. 參考文章：<https://blog.kennycoder.io/2019/12/14/JWT-JSON-Web-Tokens-%E5%8E%9F%E7%90%86%E4%BB%8B%E7%B4%B9/>
   10. client端儲存資訊：
       1. `window.sessionStorage.setItem('key',value)`：
          1. 視窗關掉就沒了。
          2. 要取值：`window.sessionStorage.getItem('key')`
          3. logout要刪除：`window.sessionStorage.removeItem('key')`
       2. `window.localStorage.setItem('key',value)`：比較久一點，視窗關掉重開還在。
       3. headers：
            標準做法：`Authorization:'Bearer ' + token`
            偷懶做法：`Authorization: token`
   11. Authorization Middleware：第361節課
       1. 為了讓每個end point檢查是不是有權限
       2. arguement有三個`(req,res,next)`而且return next()，有了return next()，這個function就變成middleware了。
       3. client端每一個fetch記得header要加上`Authorization:'Bearer ' + token`