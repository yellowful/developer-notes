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
      INSERT INTO sqlinjection(email) VALUES (''; DROP TABLE users; --);
      ```

   3. 上面的意思是
      1. 把email這個column輸入成empty，`''`可以刪除，injection仍然有效。
      2. `;`代表前面結束了。
      3. 然後下面開始新的指令`DROP TABLE`，用來把users這個table刪除，`;`代表前面指令結束了。
      4. `--`代表把後面的指令全部comment掉，不執行了。
   4. 網路上一堆人一天到晚在嚐試在email或是某些欄位貼上這種字串，看能不能injection成功。