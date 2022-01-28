# complete nodejs developer

## 基礎知識

1. netscape在1996年曾發展一套叫liveWire的web service，讓js可以在server上跑。
2. 關鍵年：2008年google發展出v8引擎，modern web的關鍵年。
3. 2009年Ryan Dahl讓node.js誕生，[當時的發表演講](https://www.youtube.com/watch?v=ztspvPYybIY)是一個重要的歷史時刻，有意思的是2019年Ryan Dahl又發表了一個重要的runtime：Deno。
4. javascript runtime：
   1. 讓javacript可以跑需要兩個條件：
      1. javascript engine
      2. javascript runtime：讓js可以跑
   2. 需要包含：
      1. call stack
      2. call heap
      3. event loop
   3. V8 Engine：
      1. 效率直接跟上google每次的改良。
      2. browser也算一個js的runtime，因為有v8 engine加上一些可以做asynchronous的 API。
   4. libuv：唸作lib-u-v
      1. 讀檔
      2. 操作資料庫
      3. C++寫的一些async的event loop
   5. 因為v8是open source，node.js主要是把這兩種library組合在一起，然後把js丟進v8，如果v8看不懂的，就丟進libuv。
   6. 什麼不是javascript的一部份：
      1. `console.log()`根本不是javascript，根本不在javascript的規格書裡，它就是browser讓我們可以做的一些function。
      2. `alert()`也不是javascript的一部份。
      3. 上面所提的是window object的一部份，也就是瀏覽器提供給我們可以做的事情。
      4. 在nodejs底下就是用global。
   7. 第5課的6:28有the node.js system的結構圖：
      1. js application <=> v8 <=> node api(node.js binding) <=> libuv
      2. 其中的libuv裡面有：
         1. worker threads
         2. event loop
         3. event queue
5. python或其它語言，也是要安狀類似的工具才能在電腦上跑，python的叫python interpreter。
6. 課程的[cheat sheet](https://zerotomastery.io/cheatsheets/node-js-cheat-sheet/?utm_source=udemy&utm_medium=coursecontent)和所有的原始碼的連結在第8課。
7. LTS (long term support)，用這個版本開發會比較穩定，node.js通常是偶數版，可以在官網releases看到各版maintenance的時間，偶數版都較久。
8. repl：
   1. 在terminal的狀態下，`node`指令沒加任何檔名，會是在一種`repl`的模式執行，意思是在一行裡，會進行read-evaluate-print-loop。
   2. read：
      1. read：讀程式碼
      2. parse：
         1. parse程式碼，把程式碼拆解開，並了解它們。
         2. 例如：`const cheer = "woo"+"hoo";`，拆解成`string"woo"`、`string"hoo"`、運算子`"+"`和`"="`。
         3. Eval階段會把右邊加起來，然後把等號右邊的值放到左邊。
         4. 然後結果會print出來：
            1. `"woo"+"hoo"`會直接print出`'woohoo'`，因為你把`"woo"+"hoo"`的結果指定給一個變數的話，這個變數的值就是`'woohoo'`。
            2. `const cheer = "woo"+"hoo";`不會回傳東西，所以會print出`undefined`，換句話說，把這一行指定給一個變數，這個變數會是`undefined`。
   3. 結束：
      1. contrl+c
      2. `.exit`
      3. 結束重開後，之前的變數會忘記。
   4. 不會用repl來寫大的app，通常會把js預先寫在檔案裡。
9. environment
   1. vscode：
      1. visual studio intellicode：寫code時會給很多額外的資訊可以看，autocomplete也比vscode內建的有效。
      2. docker
10. [練習](https://github.com/yellowful/node-argv)：
    1. browser裡：
       1. window這個object裡的attribute和method都不是javascript的一部份，都是用來支援一些讓javascript需要的功能。
       2. 例如：
          1. `window.alert()`
          2. `window.close()`
          3. `window.document`：
             1. `window.document.URL`：可以得到現在的URL。
             2. `window.document.body`：儲存html。
    2. node有他自己的API：
       1. node的api的東西，只能在node裡有用，在browser裡不能用。
       2. `process`：
          1. node api提供一個object，是`global`，可以用來控制和了解目前的Node.js的process。
          2. `process.argv`：
             1. 唸作`arg-v`
             2. 是一個array
             3. 第一個item是node這個程式所在的路徑，例如`/usr/local/bin/node`。
             4. 第二個item是執行的js檔所在的路徑
             5. 第三個item是執行這個js檔後面放的參數，如此就可以執行時給這個程式一個參數了。
11. `global`：
    1. node.js讓js可以在瀏覽器以外的環境跑，例如電腦，甚至可以在手機或機器人上跑。
    2. 瀏覽器以外跑，就沒有`window`這個object。
    3. 取而代之的是`global`這個object。
    4. `global`和`window`一樣可以省略，例如：`global.console.log()`就可以省略成`console.log()`。
    5. browser和node不能互用的功能：
      | browser   | node      |
      |-----------|-----------|
      | window    | global    |
      | document  | process   |
      | history   | module    |
      | location  | _filename |
      | navigator | require() |
    6. 其中node底下`require()`最重要和常見，因為可以用來載入其他檔案。
    7. 其它常用屬性：
       1. `__dirname`：唸作`dir-name`
       2. `__filename`
       3. `exports`
       4. `setTimeout()`：browser裡的window也有。
       5. `URL`：
          1. 可以用來處理URL相關的事情，完全follow瀏覽器提供的功能。
          2. 這是2018年node v10.0.0才加進來的功能，也就是現在新版的node和瀏覽器的寫法愈來愈接近。
12. 後端vs前端
    1. 前端：給使用者看的
       1. 瀏覽器
       2. phone app
    2. 後端：幕後在做的事(不適合或無法在前端做的事)
       1. 安全性
       2. 輸入驗證
       3. 大部份的商業邏輯
       4. 一些想要完全掌控的核心智慧財產：例如google如何決定搜尋排名，這些最高核心演算法機密都要藏在後端。
       5. 傳給前端的資料：
          1. response
          2. 可以是`html`、`javascript`、`css`
          3. 用ajax也可以是`json`、`xml`、`text`
       6. 後端可以不管用什麼語言，如：go、ruby、java，用這些語言寫的程式用來送以上資料給前端，這樣的程式碼就是後端的程式碼。

## 內部

1. 
