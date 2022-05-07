# redis

1. 是一種nosql database。
2. nosql：
   1. redis：
      1. key value pairs
      2. 記在memory裡，速度超快
      3. 可以用來記錄處理session，頁面點擊次數等資料。
      4. 不適合儲存超複雜欄位的資料
      5. 因為是存在記憶體中，要接受當機時，最新的資料可能會掉
      6. 要設計把資料存到硬碟的機制
      7. 非常scalable
   2. mongoDB或是CouchDB：適
      1. 合不斷的記錄各種不同的欄位，像是linkedIn的profile一樣的欄位
      2. 或是facebook只以使用者會單位，把該使用者的資料都記在該使用者下。
      3. mongoDB用的不是SQL而是mogoDB query語言。
   3. cassandra：Wide Column的資料型態
   4. neo4J：graph的資料型態，類似social media互相連來連去連成graph
3. 實作：
   1. [練習app](https://try.redis.io/)
   2. 沒用過的基本指令：
      1. exists
      2. expire
      3. incrby
      4. decr
   3. 重要基本指令：
      1. set
      2. get
      3. mget
      4. mset
4. data type：
   1. 預設就是string
   2. hashes
      1. 一個key配一個object
      2. 例如：`HMSET user id 45 name "Johny"`，要叫用時用`HGET user id`、`HGET user name`，或是`HGETALL user`。
   3. lists：
      1. 其實是linked list而不是array
      2. 可以左邊加：`LPUSH`
      3. 可以右邊加：`RPUSH`
      4. 左邊取得：`LRANGE`
      5. 右邊刪除：`RPOP`
   4. sets
      1. 和array很像
      2. 和array不同的地方在於，不會有相同的string，重複的只會留一個
      3. 加入：`SADD ourset 1 2 3 4 5`
      4. 有沒有在SETS裡：`SISMEMBER ourset 2`會回傳0或1
      5. 列出所有的值`SMEMBERS ourset`
   5. sorted sets
      1. 加入：`ZADD team 40 "Cavaliers"`、`ZADD team 50 "Wizards"`、`ZADD team 1 "Bolts"`
      2. 其中sort是依據50、40、1這些數字來sort
      3. 叫出來：`ZRANGE team 0 2`
      4. 取得排名：`ZRANK team "Wizards"`，會得到`2`(從0開始)。
5. 第336堂課2分50秒，出現的圖，我才了解為什麼Redis可以處理nodejs的cluster的session不一致的問題，因為redis和database一樣，是獨立於express之外，和database一樣是另外安裝，另外執行的，所以不同的node server都是打在同一個redis cluster上，所以資料就會是一致的。而Nodejs裡，有關於redis的程式碼，都像end point或是fetch或是knex一樣，只是用來連接外部的redis而已。