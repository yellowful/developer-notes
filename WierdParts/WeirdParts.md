
1. pjchender的筆記：<https://pjchender.blogspot.com/2017/06/javascript-understanding-weird-part.html>
2. first class function：意思是function可以做任何其他東西可以做的事，例如：
   1. 把它assign給變數。
   2. 把它當成值一樣傳來傳去
   3. 可以當成object的value：就變成method。
   4. 可以當成array的method去呼叫它。
   5. 可以在要用的時候順邊定義。
   6. 其他有first class function的語言有LISP, Scheme 或 ML
3. function is object
   1. function property：
      1. name：
         1. function會有一個名字的property
         2. 也可以沒有名字
         3. 也可以用一個變數，把位址指向這個function，這個變數就像是這個function的名字
      2. code：
         1. 這個property裡面存有程式碼，所以當把function這個object傳來傳去的時候，其實是把這段程式碼傳來傳去。
         2. 最特別的是這個code是invocable，直到被呼叫執行，才會真的執行。
      3. 其他properties：你甚至可以設定任何properties給function，讓function像object一樣有property可以設定，其他語言沒有這個功能。
   2. function method：
         1. bind(obj1,para1)：
            1. copy程式碼，指定這個程式碼的this是obj1這個物件。
            2. obj1如果是一個其他物件，這個function就會把這個function裡提到的this，指定成那個物件。
            3. 可以用來currying：obj1如果是this，這個this用不到，但是一樣會把這個function的程式碼copy一次，然後把para1指定給這個function的第一個參數。會等於這個function的第一個參數拿掉，讓這個function少需要輸入一個parameter。所以currying的用途，也是copy function來修改功能的寫法。
         2. call(obj1,para1)：
            1. 和bind不同，bind是copy程式碼，並沒有進入execution context的狀態，但是用call的話，是立即進入execution context。
            2. 好用的場合：拿objectA的method來用，但是用objectB的property給這個method用，所以等於可以任意組合兩個object的properties和methods。
         3. apply：和call一樣，只是parameter要改成array的形式輸入。
4. function有兩個狀態：
   1. statement：沒有名字，也沒有指定給變數
   2. expression：有名字，或有指定給變數
   3. 以上這兩個狀態都還沒到execution context的狀態，只有去invoke之後，才會到execution context的狀態。
   4. 當是statement的狀態時，無法呼叫method，改成expression狀態時，就可以呼叫method。
   5. 可以在statement的狀態下，最外面加上括號，就會變成expression。
5.  call by reference and called by value
   6. call by reference：d是一個object，這時候c=d，這個運算子=發現等號右邊是object的話，就會把c指向d的記憶體位置。也就是如果d的值變動，c就會跟著變動。
   7. call by value：這邊c = {greeting:'howdy'}，這邊的運算子=，會看到右邊的物件沒有名字，這時候會讓c開一個新的記憶體位置，把右邊的物件複製進去。
6.  functional programming：
    1.  可以把function當成參數，傳進去另一個function裡，所以可以把各個重複動作抽象化成function，互相傳來傳去，更動一部份傳進去。
    2.  做functional programming的要訣是，盡可能不要動到裡面的變數，然後盡量抽象化成不同function的目的是為了盡量不用repeat yourself。
    3.  closure：當一個function return另一個function的時候，第一個function被invoke之後exection context會結束掉，這時候回傳的function有可能還沒被invoke，但是當這個function被invoke之後，function scope裡面的variable如果找不到declaration，就會向function外面的scope找。javascript最神奇的地方在於，外面的scope的execution context已經結束了，但是記憶體還留著，所以還找得到。
7.  重要名詞：
    1.  資料型態：
        1.  primitive type, primitive value：例如int、string、或「object的primitive type的property」...等等
        2.  object：相對於primitive type則是object，例如object、function、array等等。
    2.  primitive type的型態：
        1.  mutate、mutable：變數的值變動了。
        2.  inmutable：變數不能變，例如const。
    3.  function相關：
        1.  invoke、invokable：function可以懸在那邊不invoke，需要的時候才去invoke，這就是invokable。JavaScript特有的特性。
        2.  execution context： 不是在execution context的時候，只有把記憶體開著，沒有把變數代進去。當invoke之後，就會進入execution context。
        3.  execution stack：進入execution context之後，就會去execution stack等待，cpu來檢查時，就會執行了。
        4.  on the fly：當下宣告。在定義function或invoke的時候，直接在當下宣告變數或function。

