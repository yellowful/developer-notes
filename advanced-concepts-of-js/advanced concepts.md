# Advanced Concepts of JS

functional programing：
    就是盡量把資料和邏輯分離。
    盡量composable和reusable。
    盡量pure沒有side effect。
    盡量predictable。
    盡量immutable。
    
functional programing的好處
    就是各個component的dependency較小，比較不容易出現bug。
    immutable可以記錄歷史。

idempotent：
    是指每次結果都一樣，就不用再跑一次了。
    例如：裡面有random，這樣的function雖然是pure function，但是並不是idempotent
    參考「演算法.md」裡面關於hash table的部份。

imperative vs declarative
    電腦比較imperative，人類比較declarative

can everything be pure?
    無法，只能盡量pure，這樣抓bug的時候就可以從input 和 output的地方或是side effect的地方下手。

higher order functions：
    function return function
    或是function的argument裡面有function

closure：
    布魯斯解釋的非常好：
        a function return b function，其中b function可以access a function裡面的變數，這個變數是會記得state的。
        呼叫a function的時候可以使用b function和a function裡面的state。
        但是最重要的功能是，把任兩個const指定為a function的時候，這兩個const就是b function，而兩個const用的a function的state是相互隔離的。
        非常好的例子是，一個帳戶的錢就是a function的state，領錢就是被a function return的b function，使用者f就是const f = function a，其實就是return的function b，使用者g就是const g = function g，也是return 的function b，重點是const f戶頭的錢state a和const g戶頭的錢state a是隔離的，而且是各自被記住的。
    有一個es6以前很常使用closure的地方可以是for迴圈，如果setTimeout或是任何eventListener裡面的其中一個argument，因為需要放function的定義，這時候就會出現closure，裡面的i會參照外面環境的for的i，所以當event發生的時候，i都結束了。
    解決方式是：
        另外定義一個closure的function。
        讓eventListener去「執行」這個function，因為是執行中不是定義中，所以這時候這個function不是closure的狀態，i就會把"value"傳給這個function。
        然後function裡面return的function就可以參照外面環境這個傳過來的值當作初始值的變數。
        而且因為這個function是一個closure，所以每一圈呼叫一次的closure的環境參照值都被隔離，不受影響。
    想像其他的使用場合：
        任何想要讓人很受限依照規定的取用function某個內部變數的時候可以用closure來解決，因為closure的特異功能有：
            可以在外面取得function裡面宣告的值。
            外部無法直接任意改變function裡面宣告的值。
            外部仍然可以用受限的方式改變function裡面宣告的值，受限的方式就是function return的內部function。
curring
    根據一個比較多argument的function去把argument拆開成higher oder function，這樣這個currying function就可以視情況先傳入一個變數而變成不同功用的functions了。
partial application
    和curring很容易搞混
    利用.bind(null,5)來先放入(partially apply)第一個argument，剩下的就是下次呼叫時要用完(apply)所有的arguments。

memoization (第124堂)：機器記得idempotent的function做的事情，已經放在cache裡面了，所以再次執行就不會再計算一次，而是直接顯示結果。

compose：要把f function和g function的功能組合在一起，就用一個compose function組合在一起：(data)=>f(g(data))
pipe只是順序相反：(data)=>g(f(data))

arity：就是function的argument的數目，arity越小，越容易和別的function做compose和curry。arity越大，越難和別的function做compose和curry，越不reusable。

is fp the answer to everything?
fb有很多優點，small and pure，所以不容易產生bug，很適合在distributed的system中。不過有著明確的資料結構的東西，例如遊戲，人物屬性明確，而且可以和這些屬性的state互動。例如，電商的購物車，state明確。

solution: amazon
    弄一個compose的function，這個compose的function負責將任兩個function compose在一起，而這個compose經過compose之後的arguments則是一堆functions。
    弄一個要執行所有功能的function，這個function的argument放進去所有的functions，然後return一個這些function reduce的結果。reduce裡面是放compose，是因為每一個function都要輪流進去上一次compose完的function。
    呼叫這個function時，是把各種方法依序放到這個綜合所有function的arguments裡面去，並放入data的arguments進去另一個小括號裡。
    好處是每一個小function都是pure的，都不會改變原來的user和item的資料產生bug，可以增加一個history的記錄。

[functional programing網路文章](https://medium.com/%E4%B8%80%E5%80%8B%E5%B0%8F%E5%B0%8F%E5%B7%A5%E7%A8%8B%E5%B8%AB%E7%9A%84%E9%9A%A8%E6%89%8B%E7%AD%86%E8%A8%98/javascript-functional-programming-%E4%B8%80%E6%96%87%E5%88%B0%E5%BA%95%E5%85%A8%E7%B4%80%E9%8C%84-95ff19d9892)

上面文章和marine date作業的心得，是把一堆function compose起來：

   1. compose = (...fns) => x => fns.reduceRight((acc,f)=>f(acc),x)
   2. 意思是我要把這一堆function，從最後右的function開始執行，最右邊的function先代x進去，return的結果朝左邊的function開始代進去，要用的時候就compose(...fns)(x)。
   3. 這些function要剛好右邊function return的格式要剛好是左邊function input的格式。
   4. 如果最右邊的function不用input，那麼就不需要上面的x，執行時也不用x，例如compose(...fns)。

## hoisting

1. global execution context有兩個phase：
   1. creation phase
   2. execution phase
2. creation phase的時候，var 或 function 的程式碼**看起來**會被提前，實際上沒有提前，只是先把記憶體安排好(memory heap)。
3. 如果是const，或是function被`()`包住要立即執行的話，則不會被hoisted，會報錯。
4. function declaration才會被hoisted，**function expression就不會被hoisted**，例如：

   ```js
   // 會出錯：sing不是function
   console.log(sing());
   // function expression
   var sing = function (){
       console.log('lalala')
   }
   ```

5. 上面的例子中，sing的var本身是有被hoisted的，例如：

   ```js
   // undefined
   console.log(sing);
   // var sing仍有被hoisted
   var sing = function (){
       console.log('lalala')
   }
   ```

6. 相同的兩個var或是function的declaration，後面會取代前面，因為在creation phase的時候，可以想像他們已經被設成undefined了，已經放到memory heap了。後來會進到executation phase的時候，會看到兩次declaration，會先忽略var或function關鍵字，然後先放第一個declaration的值，再放第二個declaration的值，第二個值自然的會覆蓋掉第一個值。
7. hoisting會在每一個execution context裡面發生，就是每次function執行的時候，都會發生creation phase和execution phase。
    1. 例如面試常考題：

        ```js
        var favouriteFood = "grapes";

        var foodThoughts = function () {
            console.log("Original favourite food: " + favouriteFood);

            var favouriteFood = "sushi";

            console.log("New favourite food: " + favouriteFood);
        };

        foodThoughts()
        ```

    2. 外面creation phase的時候，會先var favouriteFood和foodThoughts為undefined，function也會另外被hoisted把記憶體安排好。
    3. 外面execution phase的時候，會將favouriteFood指定為grapes，再將foodThoughts指定為function，然後才執行foodThoughts()。
    4. 在執行foodThoughts()的候後，又在這個function裡面產生兩個phase了。
    5. creation phase的時候，會先var favouriteFood為undefined。
    6. 然後到execution phase的時候，會先印出第一個undefined，才會指定favouriteFood是sushi，才印出sushi。
    7. 疑問：為什麼favoriteFood這個global scope的值grapes不會帶到function裡面？
       - 我想主要是裡面是creation phase的時候，因為有看到var，所以可以先把favouriteFood設定undefined了，所以第一個favouriteFood印出來是undefined
       - 如果裡面沒有var的話，沒有辦法先把favouriteFood設成undefined，這時候才會朝外面的scope找，這時才會找到外面的favouriteFood