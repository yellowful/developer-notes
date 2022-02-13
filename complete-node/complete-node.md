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

1. 內部流程：js => v8 => node.js APIs => node.js bindings => libuv => OS
2. node.js APIs：
   1. fs
   2. http：處理request和response
   3. path、環境變數、參數
   4. crypto：內建對資料做一些加密解密處理，提高安全性
3. node內部有些是js寫的，有些比較底層的功能是C++和C寫的。
4. node.js bindings：
   1. 用來把node.js APIs被呼叫的功能，用來跟C++或是C寫的底層功能串在一起，讓node.js可以跨平台在各個OS上跑。
   2. 也可以說是c++ binding和c binding的結合。
5. libuv：
   1. 主要處理input和output，例如檔案或是網路。
   2. 比node.js更底層一點，用C寫的。
   3. 就像是os的delegate。
   4. node.js bindings會把資料傳給libuv。
   5. 各種OS處理async的方式完全不同，libuv會根據不同的OS來處理這些不同的async的方式，比較高層的node.js bindings就可以不用擔心這些不同的OS了。
6. 藉由node.js上面提到的內部流程和功能，我們寫js時就可以寫不只有v8有的功能，來做到很多事情，特別是async的部份會是重點。
7. 原始碼：
   1. 貢獻的有：NASA、Netflix等
   2. 最重要的是：`./src`和`./lib`
   3. `./lib`：
      1. 各種內建的功能，例如`fs`、`path`、`crypto`、`http`、`url`、`os`、`process`、`console`等等。
      2. 就放在裡面的檔案，例如`path.js`、`fs.js`、`crypto.js`、`http.js`、`url.js`、`os.js`、`process.js`、`console.js`等等。
   4. `./src`：
      1. 裡面主要是Node.js bindings，而不是`libuv`。
      2. 主要比較低階的C++的code，用來連接javscript世界和C++世界。
      3. 例如要了解：`fs.open(path[, flags[, mode], callback)`
         1. 我們可以到`./lib`裡面找到`fs.js`。
         2. 找到open這個function：`function open(path, flags, mode, callback) {`
         3. function程式碼的最後一行可以看到`binding.open(pathModule.toNamespacedPath(path), flagsNumber, mode, req);`，而這一行就是在呼叫`./src`裡C++的程式碼。
         4. 我們可以去`./src`裡去找到`node_file.cc`。
            1. 副檔名：
               1. c++的程式碼的副檔名有：`.cc`, `.cpp`等等。
               2. `.h`：代表header，
            2. 檔案裡面可以找到initilize的function：`void Initialize(Local<Object> target,Local<Value> unused, Local<Context> context, void* priv) {`
               1. 裡面有一堆`SetMethod(target,js function,c++ function)`，把js和c++連接起來。
               2. 例如：`SetMethod(target,"open",Open)`，就是在js裡面的`fs.open()`就會開啟c++裡面的`Open()`。
               3. 接著我們可以找到`static void Open(const FunctionCallbackInfo<Value>& args) {`
                  1. 裡面主要做一堆validation，但是有一個地方值得看：`uv_fs_open`。
                  2. 它是在`AsnyncCall(env, req_wrap_async,args,"open",UTF8,AfterInteger,uv_fs_open,path,flags,mode);`裡面。
                  3. 也在`SyncCall(env,args[4],&req_wrap_sync,"open",uv_fs_open,*path,flags,mode);`裡面。
                  4. 表示在sync或是async裡都可以用。
                  5. `uv_`代表的就是`libuv`要用的。
   5. 深入`libuv`：
      1. [有自己的官網](https://libuv.org/)，可以獨立在其它地方使用。
      2. libuv用在跟其它語言的binding連接，例如：
         1. ruby
         2. c++11、17
         3. python
         4. perl5
         5. php
         6. go
         7. r
         8. java
         9. lisp
         10. haskell
      3. [原始碼](https://github.com/libuv/libuv)：
         1. 大部份c的原始碼都在`./src`裡：
         2. `./src/unix`包括了unix、linux、macOS的程式碼。
            1. android-ifaddrs.c是給android用的核心。
            2. darwin-proctitle.c是macOS用的核心。
            3. open的功能是在`fs.c`裡面。
            4. 和node.js binding不一樣，libuv用的是c語言，而不是c++語言，所以副檔名是`.c`。
            5. 在`fs.c`裡面，我們可以找到`uv_fs_open`，例如這一行長這樣：`srcfd = uv_fs_open(NULL, &fs_req, req->path, O_RDONLY, 0, NULL);`
            6. `uv_fs_open`的定義在這一行`int uv_fs_open(uv_loop_t* loop, uv_fs_t* req, const char* path, int flags, int mode, uv_fs_cb cb) {`，不過這一行看起來實際上沒做什麼，主要是去做`INIT(OPEN)`。
            7. 這就是node.js的binding裡面真正呼叫的c的function。
            8. 然而注意一點，真正在實作開啟檔案的function則是`uv__fs_open`，是兩條底線，那一行長這樣：`static ssize_t uv__fs_open(uv_fs_t* req){`
               1. 這裡面才真得跟OS運作在一起，其中`r = open(req->path, req->flags, req->mode);`就是讓unix開啟檔案，讓檔案可以被操作。
               2. 這裡最後`return r;`，就是回傳給node.js的binding，然後最後讓js可以運作。
         3. `./src/win`包括了各種不同windows的程式碼，版本比unix的少，各個windows版本很像，所以程式碼會比unix版本簡單很多。
            1. `fs.c`一樣也有一個`uv_fs_open`
            2. 真正用Windows在做開啟檔案的function則是`void is__open(uv_fs_t* req) {`
               1. 一堆DWORD就是windows的structures。
               2. 然而這裡面的程式碼就比linux多得多了，因為：
                  1. 這樣的動作windows就是會涉入更多東西。
                  2. 更重要的是為了讓檔案格式能和unix相容，需要做很多的處理，如此才能跨平台。
               3. 關鍵在於`CreateFileW()`，也就是在這一行`file = CreateFileW(req->file.pathw,access,share,NULL,disposition,attributes,NULL);`。
               4. 這就是讓我們可以操作windows的檔案的方式。
               5. `fd = _open_osfhandle(file, flags);`，把檔案權限傳給`fd`這個變數，讓c語言可以用這個檔案。
               6. 最後`SET_REQ_RESULT(req, fd);`，才把`fd`回傳給node.js的binding。
         4. sync and async
            1. sync代表的是一行一行的執行，結果非常容易預測。
            2. async代表的是有些程式碼丟去背景執行，就下去執行別行程式碼了，結果並不是照順序顯示。
            3. 比喻：
               1. sync：先學完html、再學完JS、再學完node.js、最後找到工作
               2. async：先學完html、學一部份JS、再學一部份node.js、找到工作，工作同時繼續學JS和Node.js。人大部份情況是用async的方式處理事情。
            4. js非常在行這種async的程式撰寫
               1. call back function：`setTimeOut`的第一個parameter，event發生後，才執行。
               2. 常用來處理需要時間等待的時機，例如：檔案處理、處理使用者登入…。
               3. 這是node.js最強大的地方，它是**event driven**，同時接到好幾千個request也能處理，不需要管前一個request是否完成了。
            5. 範例：
               
               ```js
               setTimeout(()=>{
               console.log('🐇 finished!');
               },1000)
               console.log('🐢 finished!');
               ```
            6. blocking and non-blocking function:
               1. blocking function:
                  1. 一些很快不需要花時間的function，通常都是blocking的function，大部份的function都是
                  2. 例如：`JSON.stringify({name:'Richard'})`
               2. non-blocking function:
                  1. 一些比較花時間的function才會是non-blocking function。
                  2. 花時間不是把cpu的時間佔住了，cpu只是代理網路卡和硬碟，那個花時間其實都是網路卡和硬碟在忙，cpu其實大部份時間都在idle，還可以做其它的事情。
                  3. 非常適合non-blocking的，例如：時間到`setTimeOut`、網路要求`makeRequest`、檔案讀取`readFile`等。
            7. 面試考題：js是sync還是async？
               1. js事實上是sync，核心只有sync的部份。
               2. browser和node.js的`libuv`提供了async的object，讓js可以去操弄asnyc，讓我們的app能有async的效果，或是說能處理async的事情。
               3. `setTimeOute`、`readFile`這些都不是JS的核心。
            8. multi thread:
               1. lecture 27 multi-threading, processes, and threads `05:31`：
               2. multi thread的程式：
                  1. single thread主要執行順序是call stack
                     1. 順序是從最外層的function開始放入call stack的下方，最裡層的function開始放入call stack的最上方
                     2. 由最上方開始執行，執行完，記憶體就釋放掉
                  2. multi thread：
                     1. 例如c++和java，依不同情況不同thread會有不同的執行完的時間
                     2. 有併行處理的thread，每一個thread都他自己的callstack
                     3. 處理這些thread有兩種方式：
                        1. 單一顆cpu在這些thread切換
                        2. 多顆cpu一顆負責自己的thread
                     4. `09:04`實際撰寫multi thread的code其實比上面所說的更複雜許多，而且難以debug。至少需要了解以下兩個觀念，才能讓你data和程式碼可以在不同的thread裡面一致，才能讓這些一起做用出來達到共同的目標。
                        1. multual exclusivity
                        2. locks
                     5. 沒寫好常會發生所謂的**dead lock**，就是兩個thread堵住了，在等一個永遠不會發生的action，這是有經驗的開發者常需要面對的。
                     6. 而這也是JS創造者創造JS的初衷，所以寫JS從來不需要去處理所謂的dead locks，程式碼都是sync的。
                     7. 