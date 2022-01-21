# Database

1. 為什麼要學資料庫呢？
   1. back-end：要做出使用者能呼叫和搜尋的功能，就得會把前端的搜尋轉成資料庫的搜尋。
   2. data scientist：從資料庫裡搜尋出有用的資料，進行分析。
   3. project manager：從資料庫裡發覺使用者的使用資料狀況，進而修改服務。
2. 為什麼不用後端直接儲存資料而要用資料庫儲存資料呢？
   1. 如果後端寫了一個object或array來放資料，他們只有存在記憶體中，server重開機的話，資料就忘光光了。
   2. 非常簡單的資料當然也可以後端寫入一個檔案，要用的時候再讀取。
   3. 資料庫共用相同語言，這種語言就叫做SQL，但是後端語言很多種，有python、java、C#、nodeJS，所以最好是後端用自己的語法去連接資料庫就好，資料庫都用相同的語言給各種語言連接，不用各種語言自己造輪子。
   4. 很多資料的話，要自己去搞資料結構太麻煩了，所以當然要用別人寫好的資料庫系統，直接做存取最快。
   5. 資料庫是數學家設計的，後端是computer science的人設計的，邏輯很不同，所以需要KNEX來進行連接後端和database。
   6. 熟悉select的技巧，可以讓資料在資料庫端就算好才傳到後端，不會傳了一大堆用不到的資料到後端，影響傳輸和後端運算效率。
3. 種類：
   1. 關聯式資料庫
      1. 已經知道自己要的存取方式，預先設計好資料的存取結構，才能存取
      2. 結構：
         1. 每一個table包含很多columns，每一個column包含很多rows，例如：twitter有以下table：
            1. users
            2. tweets
            3. followings
         2. column儲存一些特定的資訊分類，例如users的table中，有name、address、phone numbers等columns
         3. rows儲存內容(entry)
         4. field：columns和rows
         5. schema：指的就是table和field之間的關聯，例如：users table中的column有一個是username，tweets的各種columns中，其中一種和users的username相同，那就把他們兩個之間關聯起來。
         6. foreign key是兩個table之間相同的column。
         7. primary key是用來區分同一個table內，不同的row所用的key。
      3. 牌子：
         1. PostgreSQL
         2. Oracle Database
         3. Microsoft SQLServer
         4. Microsoft Access
         5. MySQL
         6. MariaDB
         7. IBM DB2
         8. SQLite
      4. 關聯式資料庫相對於mongoDB，需要叫出所有某設計好的欄位，就很快。
      5. Database自訂每個欄位的格式，例如2 byte的smallint整數，容量很小，速度很快。
      6. 每一個table越小越好，然後再把每個table關聯起來，這樣速度快，擴充性好。
      7. 關聯式資料庫設計：
         1. 不要用array：<https://softwareengineering.stackexchange.com/questions/381460/is-it-ever-okay-to-use-lists-in-a-relational-database>
         2. 正規化：<https://ithelp.ithome.com.tw/articles/10229472>
   2. 非關聯式資料庫
      1. 還不確定自己要解決的問題，不是預先設計好的，可以任意存取。
      2. 牌子：
         1. mongoDB
            1. Document Oriented
            2. 一個使用者一個檔案，所以要叫出所有的某欄位，就得去把每一個檔案找出來，集結起來。
            3. GUI可以用PSequel，mac和pc都相容，免費。
         2. redis
4. 關聯式資料庫的大觀念：
   1. [資料庫課程影片](https://www.khanacademy.org/computing/computer-programming/sql/relational-queries-in-sql/a/splitting-data-into-related-tables)
   2. 某項column的某row如果在好幾個table中都有的話，以後那個row要更新的話，那個row在所有table裡都要更新。
   3. 上面那點很麻煩，所以最好相同資料不要出現在不同table中，才不會那麼麻煩，server速度也才會快。
   4. 一些很常出現的東西，也把他建一個table，並賦予id，這樣就更快速、有效、不容易出錯。
   5. 但是這樣的table，並不易閱讀。但是容易維護、查詢、快速比較重要。
   6. 資料多寡的搜尋策略並不相同：
      1. 例如很少筆資料的時候，直接對所有資料檢索最快，但是一萬筆資料而且某些詞的搜尋比例不同的時候，將一萬筆資料排序後用二元檢索，或建立索引，將會比所有資料檢索快非常非常多。
      2. 所有種類的database，在你下檢索指令的時候，會在內部評估要用那種檢索策略(query plan)，再下去檢索(execution plan)。
      3. 如果後端工程師需要提高檢索速度，就需要去研究檢索策略，用自訂的檢索策略優化檢索。（這可能是為什麼要學一堆演算法的原因吧）
   7. data scientist要熟練select，但是不太需要改動資料相關指令和技巧。
   8. 所有修改和刪除相關的指令(UPDATE、DELETE)都要非常小心，INSERT和SELECT比較沒關係。
   9. 有的後端並沒有進行DELETE，而是設定一個欄位，記錄那個row被DELETE了，SELECT再另外選取，如此較為安全，但是也較為麻煩。
   10. 用DELETE之前，用SELECT進行相同的查詢，才不會殺錯。
   11. transaction：
       1. 結合幾個動作一起做，如果database無法一起做這幾個動作的話，會退回(rollback)原本狀態，不會只做一個動作沒做另一個動作。
       2. tansaction還沒完成時，這些table還不會更新，才不會出現中間query到資料不一致的情況。
   12. 為什麼要分成兩個table呢？
       1. 因為安全性的問題。
       2. web app需要load使用者資料來做應用，所以backend需要database回傳使用者資料，而web app不能load使用者的敏感資料，例如password的hash，所以password要另外存在另一個table，如此才能回傳沒有password的table給web app用。
5. PostgreSQL：
   1. 安裝：
      1. 先安裝GUI：建議DBeaver(或是PSequel)
         1. brew cask install dbeaver-community
         2. 到launchpad執行dbeaver
         3. 無法執行，會被要求安裝JDK14，去官網下載安裝。
         4. 執行時，會被要求安裝PosgreSQL的driver
      2. brew install postgresql：安裝
      3. brew services start postgresql：啟動server
      4. brew services stop postgresql：關閉server
      5. createdb 'test'：用標準port 5432打開test資料庫
      6. psql 'test'：連接test資料庫
      7. 無法連線：
         1. 如果出現錯誤`Is the server running locally and accepting
        connections on Unix domain socket "/tmp/.s.PGSQL.5432"?`
         1. 代表連線程序被一個程序擋住了，可以用以下步驟解決：
            1. `rm /usr/local/var/postgres/postmaster.pid`
            2. `brew services restart postgresql`
      8. 安裝在樹莓派：<https://pimylifeup.com/raspberry-pi-postgresql/>
         1. 先更新樹莓派
         2. 安裝
         3. 啟動：`psql`
         4. 建立資料庫：`CREATE DATABASE test;`
   2. 命令、操作：
      1. 通常命令用大寫，名稱用小寫，比較容易讀，但是小寫一樣可以執行命令。但是**名稱或內容大小寫就有區別**，所以常都會全部轉小寫才存資料庫中。
      2. 字串要用單引號，雙引號行不通。
      3. 命令可以多行，讀到分號才會執行，所以結尾一定要加分號。
      4. 要會以下常用的操作：
         1. 刪掉資料庫：
            1. DROP database_name
            2. 如果有其他連線：
                SELECT pg_terminate_backend (pg_stat_activity.pid)
                FROM pg_stat_activity
                WHERE pg_stat_activity.datname = 'database_name'; 
         2. 建立table (column, row )
            1. CREATE TABLE table_name (column1 dada_type1, column2 data_type2, ...);
            2. [重要的data type有這些](https://www.techonthenet.com/postgresql/datatypes.php)：
               1. 不准空白：NOT NULL
               2. 內部唯一序號，可以達到快速檢索：PRIMARY KEY
               3. 可以指定長度的字：VARCHAR(長度)
               4. 不重複：UNIQUE
               5. 日期使用是要加單引號，是一種字串：DATE
               6. 年齡、分數等小整數：SMALL INT
         3. 列出tables：\d
         4. 退出database：\q
         5. 取消前面命令，不執行，重新輸入，快速鍵：^c
         6. 在table插入 column和row：(已存在的3個column裡插入3個值，也就是插入一個row)
            1. INSERT INTO table_name (column1, column2, column3) VALUES (value1, value2, value3);
            2. value如果是字串要用''
            3. VALUES記得要加S
         7. 找出來某個table或某個row：
            1. SELECT (colum1, column2) FROM table_name;
            2. SELECT * FROM table_name;
         8. 修改table：
            1. 增減column：
               1. 一次增加兩個column：
                    ALTER TABLE table_name
                    ADD column3 data_type3,
                    ADD column4 data_type4;
               2. 砍掉一個column：
                    ALTER TABLE table_name
                    DROP column3;
            2. 修改column的名稱：
                ALTER TABLE table_name
                RENAME old_column1
                TO new_column1
            3. 修改column屬性：
                ALTER TABLE table_name
                ALTER COLUMN column1
                TYPE bigint
            4. 修改值：
               1. 同時修改兩個column的值：注意只有一個SET，然後WHERE前沒有逗號
                    UPDATE table_name
                    SET column3 = value3,
                    column4 = value4
                    WHERE column1 = value1;
               2. 同時修改兩個row的值：注意要用OR
                    UPDATE table_name
                    SET column3 = value3
                    WHERE column1 = value1_1
                    OR column1 = value1_2;
            5. 刪除列：DELETE FROM是接table_name，WHERE是接條件
               1. 刪除一整列：1個條件
                    DELETE FROM table_name
                    WHERE column1 = value1;
               2. 刪除一整列：2個條件
                    DELETE FROM table_name
                    WHERE column1 = value1
                    AND column2 = value 2;
            6. 刪除一個table：DROP TABLE table_name;
         9. 搜尋：
            1. 找出A開頭的人的資料
               SELECT * FROM table_name
               WHERE column1 LIKE 'A%';
            2. 找出A結尾的人的資料
               SELECT * FROM table_name
               WHERE column1 LIKE '%A'
            3. WHERE很多條件的時候：
               1. 可以加上AND, OR等等。
               2. 可以用IN取代OR
               3. IN裡面的條件可以用其他table 選出來的值當條件。
         10. 分類：GROUP BY
         11. 改名：
             1. AS：搜尋結果的column改名
             2. HAVING：要搭配GROUP，等於依據搜尋結果，GROUP改名後的WHERE
             3. CASE：
                1. WHEN 條件 THEN 新值
                2. END AS 新欄位
                3. 實例：
                        CREATE TABLE student_grades (
                            id INTEGER PRIMARY KEY AUTOINCREMENT,
                            name TEXT,
                            number_grade INTEGER,
                            fraction_completed REAL);

                        INSERT INTO student_grades (name, number_grade, fraction_completed)
                            VALUES ("Winston", 90, 0.805);
                        INSERT INTO student_grades (name, number_grade, fraction_completed)
                            VALUES ("Winnefer", 95, 0.901);
                        INSERT INTO student_grades (name, number_grade, fraction_completed)
                            VALUES ("Winsteen", 85, 0.906);
                        INSERT INTO student_grades (name, number_grade, fraction_completed)
                            VALUES ("Wincifer", 66, 0.7054);
                        INSERT INTO student_grades (name, number_grade, fraction_completed)
                            VALUES ("Winster", 76, 0.5013);
                        INSERT INTO student_grades (name, number_grade, fraction_completed)
                            VALUES ("Winstonia", 82, 0.9045);

                        SELECT name, number_grade, ROUND(fraction_completed * 100) as percent_completed FROM student_grades;

                        SELECT COUNT(*),
                            CASE
                                WHEN number_grade > 90 THEN 'A'
                                WHEN number_grade > 80 THEN 'B'
                                WHEN number_grade > 70 THEN 'C'
                                ELSE 'F'
                            END AS letter_grade
                        FROM student_grades
                        GROUP BY letter_grade;
                    1. 'B'的條件其實包含了<=90
                    2. 顯示的欄位不只COUNT，還包含letter_grade
         12. COUNT：計數
         13. 比大小：會自動分組，不用手動寫出小於的指令，例如：
                 SELECT COUNT(*),
                 CASE
                    WHEN number_grade > 90 THEN 'A'
                    WHEN number_grade > 80 THEN 'B'
                    WHEN number_grade > 70 THEN 'C'
                    ELSE 'F'
                 END AS letter_grade
                 FROM student_grades
                 GROUP BY letter_grade;
         14. 依分數的順序列出資料：
               SELECT * FROM table_name
               ORDER BY column1 DESC(或是ASC);
         15. 運用function：平均AVG、總和SUM、計數COUNT、四捨五入ROUND
             1. SELECT AVG(column2) FROM table_name;
             2. SELECT SUM(column3) FROM table_name;
             3. SELECT COUNT(colum4) FROM table_name;
         16. 關聯兩個table：
             1. SQL：
                SELECT * FROM table_name1
                JOIN table_name2
                ON table_name1.column3 = table_name2.column2;
             2. 上面程式碼，只有相同名字的才會在新的表裡面
             3. 這個出現的表，並不是產生了一個新的table，而是顯示成新的table，難怪是用SELECT指令。
             4. LEFT OUTER JOIN：和JOIN用法一樣，但是讓table_name1有(LETF)而table_name2沒有(RIGHT)的欄位也必須顯示，就是LEFT比RIGHT廣(OUTER)的意思。但是RIGHT OUTER JOIN就很少人用了，習慣問題。
         17. 權限：
             1. GRANT FULL ON TABLE table_name TO super_admin;
             2. GRANT SELECT ON TABLE table_name TO analyzing_user;
             3. 有些公司不願客戶個資被query，那可以做一個匿名版的database。
         18. transaction：
                BEGIN TRANSACTION;
                一連串命令;
                COMMIT;
         19. 可以練習的網站：
             1. <https://www.khanacademy.org/computing/computer-programming/sql>
             2. <https://www.codecademy.com/learn/learn-sql>
             3. 有一種sqLite，可以直接在web page建立資料庫，但程式碼放在前端，我覺得有一定的危險性：<https://www.khanacademy.org/computer-programming/using-sqlite-inside-a-webpage/5618461699080192>
6. 後端用的library
   1. pg-promise：語法簡單，類似sql語法
   2. KNEX.JS：
      1. KNEX最好用：http://knexjs.org/#Installation-node
         1. npm install knex
         2. npm install pg
      2. 設定：
         1. require的地方，client要設為pg
         2. server送出給postgresql的connection
            1. user要設成database的創建者
            2. 要有password，即使是空字串也沒關係
      3. 語法：
         1. .slect()
            1. table：.from('table_name')
            2. 條件：.where({column1:value1})
            3. 回傳：.then(data=>{})
         2. .insert()
            1. table：.into('table_name')
            2. 回傳：.returning('*')、.then(data=>{})
         3. .increment()
            1. table：db('table_name')
            2. 條件：.where('column1',value1)
            3. 回傳：.returning('*')、.then(data=>{})
            4. update：每次加1的話，.increment('column1',1)
         4. transaction：
            1. 用法：
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
               })
            2. 意義：register的時候，同時讀寫兩個table的時候，要用transaction，防止同時access兩個以上資料庫造成inconsistency。transaction保證兩個動作之間是連續的，如果有插入其他動作，就會把兩個table的資料都退回(roll back)。
         5. 後端和資料庫之間不是http在傳送，所以不需要透過.jason()轉換。
         6. .then(resXXX=>{})一定要有這個格式，裡面必須是一個function statement。直接res.json()，並不是一個function statement，而是一個function express。 
7. Heroku部署postgresql：
   1. 連接資料庫：
      1. 去heroku的data那邊安裝postgresql，把postgre連接到backend app。
      2. heroku addons確認是否裝好
      3. 用command line連上資料庫：
         1. heroku pg:psql
         2. 如果有多個資料庫：heroku pg:psql 資料庫名稱
      4. 連接後的sql用法就和一般sql一樣了：
         例如：\d可以看database的owner是誰
      5. 看Add-on裡database的url：heroku pg:info
      6. 讀heroku的postgres的文件「連到node.js」
         1. 把後端knex連接到database的URL改成：process.env.DATABASE_URL
         2. 把後端knex連接的程式碼改成：
            connectionString: process.env.DATABASE_URL,
            ssl: {
               rejectUnauthorized: false
            }
         3. 雖然Andrei是用ssl:true，但是ssl:true要收費，Andrei用以下設定，不安全，正式商業化時不要用，而我是用heroku文件的用法，沒用以下用法：
            process.env.NODE_TLS_REJECT_UNAUTHORIZED = 0;
   2. command line連上資料庫之後，用sql指令去把相關欄位建立起來。
