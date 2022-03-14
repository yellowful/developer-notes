# Java Script 筆記

1. [repo](https://github.com/yellowful/vanilla-javascript-exercises)
2. javascript之所以放在下面，可以避免被javascript的程序卡住了，而可以先把*頁面*載入，讓使用者先看到頁面，再執行邏輯流程。
3. javascript這種script語言和python一樣，都會自動判斷型別，比較特殊的有：
   * prompt的輸入值，預設為string。
   * 一個文字和一個數字相加，會判斷為文字相加。
   * 文字不能相減。
4. 最難的是function，重點有：
   * function並不一定要有參數。
   * 定義某變數時同時定義function，可以用匿名函數，而該變數就是這個function的物件。
     * 這時如果function同時取一個名字，不會出現錯誤，但是沒有用，用這個名字呼叫function時，他會說沒定義，這個andrei沒有講到。
     * 這時的{}要加上;作為結束。
   * 另一個很像的情況，但完全不同的意義。若某變數放某個有引數值的function，則這個變數不是物件，而是function的回傳值。
   * arguement(引數)和parameter(參數)不同，parameter指的是定義函數時的變數、定義，而arguement是帶入該函數的參數的特定值。
5. data structure：
   * object太強大了，可以放任何東西：
     * 可以放index，list的index就是0,1,...，然而object可以任意取名index，就是所謂的屬性。
     * 可以放array在value裡面。
     * 可以放fuction在value裡面，而這個fuction會變成所謂的method。
     * object裡，要加入index和value用句點去指定就好了，而list則需要特殊的method。
     * {}和null不相同，在object裡，{}可以放value進去，null則不行。
   * list的特性：
     * 不需要再去想index要取什麼名字，而且index可以給予變數變化，可以用for迴圈存取，方便。
     * javascript的list裡可以有不同的型別在裡面。
     * list也可以放list在裡面，就變2維陣列。list裡有list裡有list就變3維陣列。 
6. 名稱：
   * expresion：表達示，通常會產生一個值，而且要以;為結尾。
   * function和object的共同點是，都有一個大括號包著，不同點是function在大括號外面會有關鍵字fuction。
   * 但object裡面可以有function的資料型態，這就稱為method。
   * fuction有兩種方式定義，一種是function declaration直接幫fuction取名，另一種是fuction expression，是把fuction放到變數裡。
   * 執行fuction叫calling或invoking。
   * 在變數裡面放值叫作assign，就是等號。
7. loop：
   * 共有四種：for、while、do while、for each
   * for：
     * 最常用，計數方式直接在參數的地方。相對於while，如果要做的事情的次數是用一個index來決定的話，用for loop寫比較方便也比較readable。
     * 一個小技巧，可以把for迴圈放在function裡，利用function的return來回傳值，並跳出迴圈。
     * 比while更加declarative，因為可以直接用i來決定圈數；比forEach還imperative，因為forEach不用自己設定圈數了，item幾個就是幾圈。
     * for (let index in array)：
       * 這種用法較佳，會忽略empty的item。
       * 這要注意一點，這裡的index竟然不是數字，而是string。
     * for (let item of array)：這種如果array有empty的時候，那個emty的item仍會跑，他的值會是undefined，比較容易出錯。
   * while：
     * 最適合「無線迴圈直到不符合條件才跳出」的情況。
     * 找不到一個index可以決定圈數的話，可以思考用while來做。
     * 比for來的imperative，for迴圈一定可以改寫成while迴圈，但是寫起來較麻煩，較不readable。
     * continue：取消當圈，其他圈會繼續跑
     * break：這一圈之後全部取消。
   * do while：無論如何都會做第一次的時候，比較好用。
   * forEach：list用forEach最快，這個方法的parameter一定是一個function，而這個function的第一個parameter就是list的值，第二個parameter就是list的位置。
   * iterable:
        array、string是iterable，而object不是iterable
        for of也是iterable
        item代表value
   * enumerable:
        array, string, object都是enumerable，因為array的key就是數字
        for in是enumerable
        item代表是key
8. DOM：
   * DOM不是全世界，原來document是一個和內容比較有關的物件，他的兄弟還有alert、scrollbars、navigator、statusbar…等和視窗有關的物件，他們的爸爸物件是window。
   * window的屬性：
     * .innerWidth和.innerHeight：是螢幕截圖的大小。
     * .screen.width和.screen.height：是browser的大小。
   * .querySelector("h2")和.getElementsByTagName("h2")[0]的效果相同。
   * .querySlectorAll("h2")[0]和.getElementsByTagName("h2")[0]的效果一樣。
   * 只要是一次會選到好幾個，一定要用中括號指定是哪一個，不然會跑不出來。
   * .querySelector比document.getElementsByTagName常用，原因是.querySelector不管選誰的都不用改method，只是arugument不同，例如id的話要加井字，如果class要加句點。
   * .getAtribute和.setAttribute可以操弄html的tag的屬性，注意，是只有tag的屬性，而不能用style的屬性。而且當原本tag沒有設定屬性時，就無法操弄。
   * 用.style就可以改變style的屬性了。
   * 用.className是attribute而不是method，可以放class的名字。
   * 用.classList可以得到class的名稱，再加上.add可以加上class，.remove可以移除class，toggle則每執行一次就加上或移除class一次。這幾個都是method而不是attribute，名字都要當作arguments放進method裡。
   * .innerHTML也是屬性，可以改變內文，內文也可以加tag。
   * 要選父親是.parentElement，要選子卻是用.children，不用Element。
   * 使用時儘量存在變數裡，不要一直select，可以加快chrome速度。
9. [shopping list心得](https://github.com/yellowful/vanilla-javascript-exercises/blob/main/ShoppingList/original/script.js)：
   * 作法：
     * 點按鈕就會新增項目(用creatElement())。
     * 將特定文字加入新增項目(把input轉成text node元件後，用.appendChild加入list當中)。
     * 將輸入區的文字帶入新增項目(用input.value的屬性)。
     * 判斷輸入區有文字才能帶入新項目。
     * 監聽輸入的是enter也能新增項目(用addEventListener("keypress",function)，enter的keycode是13)。
     * 監聽列表是否被點擊，並將被點擊的項目指定切換刪除線格式(用classList.toggle()的方法來作)。
     * 新增新項目時，將按鈕也一起帶入新增項目(建立delete button，然後加入list當中)。
     * 監聽條件修改，監聽到文字就切換刪除線，監聽到刪除鍵，就刪除項目。(刪除父項目用target.parentElement.remove()的方法)
     * 補上新增新項目時，要將新項目的刪除鍵為被監聽的對象。(重新querySelectAll所有項目，然後將最後一個element設為監聽)
   * 要注意的細節：
     * .appendChild的argument只能是element，所以文字要用.creatTextNode轉成text node的element才能被加入list裡。
     * 將按鈕也一起帶入新增項目會造成點前面的文字產生刪除線，也會造成button的文字一起產刪除線，解決方式是，將前面的文字用一個span包住，用if判斷，只有點到span才給他刪除線。
     * .addEventListener在監聽的時候，所有的母、子元件都會一起監聽，所以要用.target來選子元件，.currentTarget來選父元件。
     * 新增的list的刪除鍵沒有產生刪除項目的功能，主要是因為新增的按鈕沒被監聽，所以新增list時，要去將新的按鈕設定監聽。
     * 而設定新增按鈕被監聽時，會發現找不到新增的按鈕可以被設定，所以需要重新用.querySelectorAll更新選擇所有的按鈕。
10. BackgroundGenerator的心得：
    1. 重點在於知道是用background:linear-gradient(方向,第一顏色,第二顏色);這個function的用法。
    2. . 要用JavaScript改寫css，是用「.style.屬性」去改寫，後面的linear-gradient()是字串。
    3. 叫出顏色選擇器的方法是iput，type是color。
    4. 我random顏色的方式是用16進位，這是個井字號加0~F組成的七個字的字串，我讓後面6個字母random出現。
    5. javascript中，字串可以用類似array的方式顯示某個字母，例如：'#33EEFF'要顯現第三個字母就xxx[2]，可以顯現，這部份和python一樣。甚至var a = ['first','word']，可以用a [1][2]來叫出r這個字母。但是和python不一樣的是，這並不是真的array，如果用a[1][2]=‘z’;並不能讓這個字母變成z。
    6. 所以顏色的字串如果要random就要先拆成array來處理，最後面用.joint('')組合回字串。
    7. css在用first-child和nth-child()時，數目是包含不同tag的element，如果選不到可改用first-of-type，就只會挑那一類的。
    8. Math.random()是0-1的浮點數，所以要乘上要的範圍，再加上Math.floor()，才會變整數。
    9. 要頁面一載入就印出數值，就要在addEventListener之外，執行一次印出。
    10. 問題：多個p時，理論上可以用first-child和nth-child()來選，但是為什麼我卻選不到？
11. scope：
    1. 最根目錄是window，如果在某fuction內要宣告全域變數，可以用window.a
    2. 小心if裡面宣告var的scope會影響全域，因為if不是function。
12. ternary operator:
    1. 用在是非題，對指定一個值，錯指定另一個值，程式碼較精簡。
    2. true ? 一個值:另一個值;
13. switch:
    1. 大部份由4個關鍵字構成：
        1. switch:配合括號內的parameter。
        2. case:後面接對應的argument
        3. break:放在每個case最後，沒放的話，會執行下一個case，不會結束。
        4. default：避免沒想到的情形。
    2. 用來簡化一堆的else if。
14. coercion：
    1. type coercion：JS引擎會自動改變變數的型別，修正人類的錯誤，稱為coercion，但這能力太強，要非常小心，最好不要用。永遠用`===`，不要用`==`。
    2. `[]===[]`為false因為兩個value相同，但reference不同。class有同樣的效果。
15. ECMA6：
    1. var和let的共同點：
        1. fuction外宣告，fuction內沒宣告，就是全域變數。
        2. fuction外宣告，fuction內又宣告，fuction內的就是區域變數。
        3. 也就是同名稱的變數，var若在function外宣告一次，在function裡宣告一次，裡面和外面互相獨立不相關。
    2. var和let的不同點：
        1. 在if或是loops的block中，同名稱變數var若在if外宣告一次，在if的block裡又宣告一次，裡面的值會污染外面，這和function的經驗不同，不容易debug，易出錯。
        2. 在if或是loops的block中，同名稱變數let若在if外宣告一次，在if的block裡又宣告一次，裡面的值會被隔絕，和外面值會不同，這和function的經驗一致，不容易出錯，容易 debug。
        3. loop內若是有callback function，用var宣告算次數的變數的話，loop不會等callback function執行就會繼續往下跑，用let宣告算次數的變數的話，就會正常，等 callback function被call了才會往下跑，符合一般經驗。
        4. var還有一個缺點，他可以前面不宣告，最後再宣告，這樣就會變成windows的全域變數，這點非常奇怪 ,很容易造成大的bug。
    3. 可以用const就不要用let，可以用let就不要用var，用const在大型的合作案，這個變數比較不會隨便被人改掉。
    4. const一個object的話，這個object不能再被改成別的object，**但是**卻可以變動裡面的屬性。
    5. 可以到 <https://babeljs.io/> ，看如何將ECMA 6改寫成ECMA 5。
    6. template literals讓string裡，只要用${變數}，就可以處理變數，但要注意的是，他的括號不是括號，是用back-tick(重音符號)。
    7. 箭頭函式，用法就是把一般的函式取消function的保留字，直接用()=>{}來定義parameter和function，再把它放到一個變數裡，這種寫法讓程式較為clean。
    8. 新的型別symbol:宣告為symbol的目的是，即使兩個symbol的值相同，在if裡判斷時，也會被判斷為不同。
16. function進階：
    1. return：
        1. 可以用來回傳值。
        2. 可以用來中斷function的執行。
        3. 可以用來回傳一個function，這個function被回傳後，還不會被invoke，直到被加上括號後，或傳值進去時，才會執行。
    2. 兩層的箭頭函式，使用時會有兩個()，解讀技巧在於外層的變數會傳進內層的變數，也就是最左邊function的parameter會傳進最右邊function的變數裡。
    3. 三種常見名詞的函數，需要看得懂：
        1. closure：當我們需要的變數或參數希望是一個function時，我們可以利用兩層function，讓回傳值是一個function，這個回傳的function就是個closure，例如兩層的箭頭函式。closure外層的變數，會讓內層可以用，也就是執行內層function時，找不到這個變數的宣告，就會朝父function找。
           1. 可以參考WeirdParts.md、advanced concepts.md。
        2. currying：currying讓原本多個參數的function，利用closure簡化成多個單一參數的function的過程。例如：原本curriedSum(a)(b)，利用closure變成add5(b)的過程，目的是簡化輸入參數。
            1. 用來可以先丟入一個參數的之後，可以回傳一個可以丟入另一個參數的function。
            2. 使用方式是，原本定義有兩個參數括號的function的話，第一個括號要設計成先要放進去的變數，第二個括號要設計成要回傳的function中，要給人用的引數。
            3. 例如實作RESTFUL api時，用currying的方式，讓module回傳一個function，這個function是一個接收request和response兩個parameter的function。
        3. compose：原本有兩個function，現在定義一個compose把兩個function串在一起，輸入的參數就是兩個function，而且第二層會有一個function是另一個function的參數的情況，而變數引數的輸入是設計在第二層之中的參數。這個引數就會鑽入一個function後鑽出進入另一個function後再鑽出。
    4. pure function：
        1. deterministic：相同input，會獲得相同output。
        2. no side effect：
            1. 不會有變數傳值出去，產生意外的影響，例如在function裡設一個windows的變數。
            2. 要傳值出去，都是透過return傳值。也不是靠著外部的變數來產生結果，都是靠著input來算值。
            3. 沒有console.log，也沒有alert。
        3. 有輸入和輸出，比較不會出現undefine的情況。
        4. 寫function最好是pure function，比較不會出現無法預期的情況。
    5. indempotence：和pure function很像，但是有點不一樣。indempotence不一定沒有side effect，但是他的out put非常確定，每次都一樣，例如function裡面的console.log()。
17. array進階：
    1. .forEach：括號內做完，並不置換array內的元件，不return，要置換的話，就把一個新array.push放裡面，值就會在裡面了。
    2. .map：非常好用，括號內做完，會一個一個置換array內的元件，傳到外面來。
        物件的陣列，.map裡在傳的值就是單一物件，這個單一物件的屬性裡若有陣列要用.map，就可以用物件.屬性.map()。
    3. .filter：括號內放一個函式，這個函式能return true 或false，.filter就會把true的元件傳到外面來。
    4. .reduce：是把一個陣列裡的元素運算過，結果記起來，把結果和下一個元素繼續運算，最後就reduce剩一個元素了。.reduce裡面第一個參數是一個function，fucntion的第一個參數是會被記住的accumulator，第二個參數是元素。.reduce裡面的第二個參數是，accumulator的起始值。
    5. .forEach比較不pure。
18. class：
    1. constructor:語法和function一樣，差別在於屬性加上關鍵字this，用來設計這個class有哪些屬性。
    2. method:就是在constructor外定義的function。
    3. extends：宣告子類別時，用來連結父類別。
    4. super:宣告子類別時，要沿用父類別的屬性時，呼叫的函式。
    5. new：宣告物件時的保留字。
    6. instantiation：宣告新物件時，就會產生新instant。
    7. this：<https://blog.techbridge.cc/2019/02/23/javascript-this/>
        1. 觀念非常重要，指的是所在scope的父類別的物件，注意是物件，不是類別。例如，JavaScript內建的一些函數，前面可以加上this(`this.fetch`,`this.console.log`)，這個this指的是windows物件。
        2. 「所在scope的父類別物件」指的是，this寫在某個class中，用這個class可以instanciate出一個object，這個object可以用一些method，這些method是在class裡面定義的，所以this.method()的this指的是這個class實體化的object，這個method所在的scope的爸爸class的object。
        3. class在instanciate的時候，就是會先把constructor裡一屬性變成一個object，this指的就是這個object。
        4. React中，class component如果要把爸爸component的state或function傳進來，就要用props從super傳進來，在這個class裡要用的時候，就要加上this，例如this.props，這個this指的是這個class實體化的component object，而不是爸爸component。
        5. 有一種情況，爸爸class中有this，extends的小孩class中也有this，而且小孩引用了爸爸的某些attribute或method有this。
            1. 當用小孩的class去實體化一個object的時候，小孩class和小孩class裡引用到爸爸的this，全都是指這個小孩object。這一點是很容易弄錯的，要小心。
            2. 當用爸爸的class去實體化一個object的時候，爸爸class裡的this指的當然就是這個爸爸object。
            3. 好記得方式是`aaa.bbb.ccc.ddd.method()`的this是指小數點前面的object，也就是`ddd`。
        6. react無法把資料上傳到爸爸component，但是看起來很像把event上傳給爸爸，其實是從爸爸component下傳一個function作為property，這個function會在小孩component裡面成為property的一個attribute。讓這個function放在DOM的一個call back property裡面，當作call back function，所以在爸爸那邊，這個function就可以listen 小孩call back function的events了，爸爸這邊也可以根據聽到的event更新state。
        7. 如果code呼叫太多this，可以用destructuring，讓程式碼比較clean。
        8. [四種情況下的this](https://reactkungfu.com/2015/07/why-and-how-to-bind-methods-in-your-react-component-classes/)：
           1. function invocation：沒有`.`，直接call function，this通常就是window，以下的`fun()`的`this`仍然是window，所以`this.name`會是undefined

                ```js
                const unicorns = {
                    name:'oh no',
                    func: function() {
                    console.log(this.name)
                    }
                }
                unicorns.func(); // oh no
                var fun = unicorns.func;
                fun(); // undefined
                ```

           2. method invocation：一連串的句點，this指的會是最右邊的method所在的object

                ```js
                var foo = {
                   name:'foo',
                   bar: {
                      name:'bar',
                      func: function() { console.log(this.name) }
                   }
                };

               foo.bar.func();// bar
               const fun2 = foo.bar.func;
               fun2(); // undefined
               ```

           3. constructor invocation：
              1. function有兩種用法，其中一種是被`new` operator使用的用法，這種function稱為function constructor，用來建立物件，它是有[歷史淵源](https://pjchender.blogspot.com/2016/06/javascriptfunction-constructornew.html)的。主要是js沒有class，而當時為了模仿當時java的class很炫的用法。
              2. 這種用法下的function
                 1. 不能放return
                 2. new的時候，一開始會產生一個空的object
                 3. 然後function**會被執行**(invoke)
                 4. invoke後，constructor裡面的attibute才會被指定值，method也被執行
                 5. 最後這個function會自動`return this`，也就是等號左邊會被reference到一個object上，而這種object就叫作instance。

                    ```js
                    function dog(name){
                        this.dogname=name;
                        console.log('this.dogname',this.dogname);
                        console.log('inner this',this);
                    }
                    dog('wow'); // 直接呼叫的話，this會是指window，之後this.dogname會被指定成'wow'
                    console.log('window',dogname);
                    // 這裡非常容易搞混，這裡meow會變成一個object而不是一個function，{name:undefined}
                    const meow = new dog();
                    console.log('meow',meow);// meow dog { dogname: undefined }

                    // 因為不是直接呼叫function，是用new來操作，這時候的dog('worf')是個constructor
                    // 一開始的this會指向一個空的object，之後把attribute給值，然後return this。
                    const mouse = new dog('worf');
                    console.log('mouse',mouse); // {dogname:'worf'}
                    ```

              3. ES6之後react的class和js的class的比較
                 1. 相同的地方是arrow function會autobinding，也就是你寫的arrow function裡面的this，這個this就會是這個class被instanciate之後的object。
                 2. 不同的地方在於：
                    1. react的class裡的傳統function的this並沒有autobinding，在instanciate之後，this變成window。
                    2. 而js的傳統function的this仍然有autobinding，在instanciate之後，this就變成這個instance。
           4. apply invocation：用`.call()`、`.bind()`、`.apply()`來操控this，[參考wierd parts](https://github.com/yellowful/developer-notes/blob/main/WierdParts/WeirdParts.md)。
    8. scope：指的是所在位置的function或class裡，指的是variable做用的範圍。和context很容易被搞混，context指的是this指的是誰，只和this有關。
    9. 請參考演算法筆記「class」。
19. Object：
    1. pass by value：
        1. pass by value用法就和variant一樣，“=”是pass by value，意思是等號兩邊用不同塊記憶體，改變其中一值，另一值不受影響。
        2. array = [1,2,3]是pass by value，object = {key1:3}也是pass by value，因為javascript會判斷等號右邊是變數還是值，如果是值，就會把左邊安排一塊新的記憶體用來複製右邊的值，是變數的話就只會指向相同的記憶體(pass by reference)。
    2. pass by reference：
        1. 之所以javascript要有pass by reference的設計，因為array和object佔的記憶體較大，所以javascript 發現等號右邊是變數，會讓“=”兩邊佔同一塊記憶體，改變其中一值(裡面的element的值)，另一個值也會變。
        2. array1 = array2時，是pass by reference，若要pass by value來複製array2有以下幾個方法：
            1. array1=[].concat(array2)
            2. array1=array2.concat([])
            3. array1=[...array2]
            4. array1=array2.slice()
            5. .concat()是用來把兩個array接起來的，.slice()的用法是用來把array裡面的某幾個元素挑出來。
        3. object1=object2也是pass by reference，若要pass by value來複製object：
            1. 淺層複製：(只有一層)
                1. object1 = Object.assign({},object2)
                2. object1 = {...object2}
                3. 若object裡還有object，用Object.assign()會讓最裡面的object仍是pass by reference。
            2. 深層複製(兩層以上)：若要最裡面的object是用pass by value，就得用JSON.parse(JSON.stringify(object))來解決，但效能會降低。
        4. Object.assign()：
            1. 先下結論：
               1. 要pass by value，第一個arguement就要用空的object。
               2. 都只有第一層object的key value pair有用，深層的沒效，深層的要去深層呼叫Object.assign()，或用JSON.parse()。
            2. 主要用途：連接或合併多個object，例如要合併obj2和obj3
                * obj1=Object.assign({},obj2,obj3);
                    得到：
                        新的obj1：{key2:"value2",key3:"value3"}
                        原來的obj2：{key2:"value2"}
                        原來的obj3：{key3:"value3"}
                        這三者之間pass by value
                * 之所以前面要加空object，是因為沒有加空object的話，obj2會被更動，而且obj1和obj2直間仍會是pass by reference，而*這很可能不是我們想要的結果*，例如：
                    obj1=Object.assign(obj2,obj3);
                        會得到：
                            新的obj1：{key2:"value2",key3:"value3"}
                            被改變的obj2：{key2:"value2",key3:"value3"}
                            原來的obj3：{key3:"value3"}
                            obj1和obj2之間pass by reference
            3. 第二個用途：用來pass by value，一樣前面一定要加空object，否則要被複製的object會被更動到
                1. 把obj2複製給obj1：obj1=Object.assign({},obj2)
                2. 把某object裡面的其中一個value用pass by value的方式更新給另一個object：
                    obj1=Object.assign({},{
                        key21:"value21",
                        key22:"value22"},{key22:"newValue22"}
                    )
                        會得到：
                            新的obj1：{key21:"value21",key22:"newValue22"}
                            原來的obj2：{key21:"value21",key22:"value22"}
                            要更新的key value pair：{key22:"newValue22"}
            4. 混合用途：相同key的會指定value的效果，不同key的會有組合object的效果。
        5. spread operator的功用：
            1. 變數裡面已經有值的時候：
                1. 是把一個array、object或Set展開變成一串逗號相連的值。(es6的時候還只有array可以用)
                2. 有連接兩個array或是object的功能，用法是放在要連接的array或object裡面，這個功能和Object.assign()是一樣的。
                3. 有將array或是object用pass by value的方式複製的功能，這個功能和Object.assign()也是一樣的。
                4. 如第1點所說的，就是有將陣列展開變成一連串數字的功能：
                    1. 方便帶入function的arguments中。
                    2. 也就是把array的中括號去掉。
                    3. 例如：number是[1,2,3,4,5]，用Math.max(...number)就可以得出最大值。
            2. 變數裡面還沒有值的時候：
               1. 反過來，是把要放進來的值，這些由逗號分開的值，變成array或object。
               2. 例如：

                  ```js
                  const [a,b,...rest]=[1,2,3,4,5];
                  console.log(rest);//[3,4,5]
                  const {f,g,...restOfObj}={c:1,d:2,e:3,f:4,g:5}
                  console.log(restOfObj);//{e:3,f:4,g:5}
                  ```

               3. 也是pass by value。
            3. 字串：`[...string]`就是把 string 拆成 array。
    3. 把string或json轉object：(收到網路資料後)
        1. JSON.parse(資料)。
        2. 資料.json()
    4. 把object轉json或string：(送出網路資料前)
        1. JSON.stringify(資料)
        2. .json(資料)
    5. object的key一般來說是string，但是如果要讓key的type是interger或是variable，在key外面加上括號就可以了，例如{[key1]:value1}。object還有一個神奇的特殊功能，竟然可以讓**值**當成屬性的名稱，只要被包在中括號裡就好，這個也太奇妙了吧。
    6. destructuring：
        1. object可以很容易被destructuring，設定新變數時，只要變數名稱和object的key一樣，就會自動對應了。
        2. destructuring的時候，被宣告的新變數要被一個大括號包起來。
        3. destructuring很常用到，例如：
            1. React的component的argument有大括號，就是把props這個object destructuring了。
            2. React的component裡面，也常會把props destructuring。
            3. React的this.setState()裡面有大括號，就是把this.state這個object destructuring。
            4. 針對變數名稱和object的attibute名稱不同的shorthand是另一種寫法：let {name: NAME, age: AGE} = user，其中user是{ name: "Judy", age: 40};代表的是let NAME = user.name; let AGE = user.age;
            5. setState({a:'b'})，裡面放的是物件而不是destructure。
        4. 相反的short hand：
            1. 當要把一個object的key指定一個變數的value，而這個key的名稱和這個value變數的名稱相同時，可以用這個short hand，例如：{user}就是{user:user}的short hand，{user:user}的第一個user是object的key，第二個user是要當value的變數名稱。
            2. 這個short hand也是destructuring的一種，不一樣的是正面的short hand是設定新變數，這個short hand是指定value給key。
20. Prototype:
    1. <https://blog.techbridge.cc/2017/04/22/javascript-prototype/>
    2. <https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part4/prototype.html>
    3. ES6以前的語法，ES6以後已經用語法糖包起來了，用constructor和來取代prototype的用法。
    4. 每個object都有_proto_，指向自己的上一層，一般就是function的prototype()。
    5. 每個prototype()都有constructor，都是function本身。
    6. prototype()也是object，他是javascript設計的Object弄出來的instance，所以他的_proto_是指向Object弄出來的instance。
    7. <http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html>
       1. prototype()讓不同instance(object)，可以共用方法，不用多花記憶體。
       2. prototype()原來是用來解決JS沒有Class的問題，原本的function object屬性，每個instance的值都不同，但是如果要設定相同，就用prototype的方式設定。
    8. [也可以從物件導向的角度了解prototype](https://developer.mozilla.org/zh-TW/docs/conflicting/Learn/JavaScript/Objects/Classes_in_JavaScript)
21. ES7：
    1. .includes()在判斷array裡是否有括號裡的元素，有的話就回傳true，沒有就回傳false。
    2. .includes有另一個用法，是判斷這個字串裡是否有括號裡的字串，有的話傳true，反之傳false。
    3. 兩個星星的運算元，用來算次方。
22. ES8：
     1. .padStart在string前加字，第一個參數是要加幾個字，第二個參數是要加什麼字。.padEnd則是加在後面。
     2. 如果要處理一堆Object，要把object變array比較容易處理了：
        1. Object.key()得到一個key的array，要配合object[key]，才能拿到對應的value。
        2. Object.values()得到一個value的array。
        3. Object.entries()得到一個二維array，同時有key和value。二維陣列用map時，裡面的value是一個陣列。
23. ES10：
    1. .flat：可以把多層array去掉，沒放參數代表一層，全部去掉參數可以用Infinity。另一個功能是把空的element刪掉。
    2. .join：不是ES10的功能，是舊功能，可以把array去掉，得到一個組合的string。
        1. .flat和.join不同之處在於，.flat回傳的是array，.join回傳的是一個字串。
        2. .join功能和.reduce有點像，計算用.reduce，字串用.join。
        3. .join可以一次去掉多層array，但會把逗號保留住，解法是先.flat之後再.join。
    3. .flatMap可以去掉多層array後，會傳element成為array。
    4. 技巧：回傳後是array的話，還可以再.method()一次。
    5. .trimStart和.trimEnd可以刪string的字首或字尾的空白。
        1. 舊的功能只有trim，把所有的空白刪掉。
        2. 和.padStart及.padEnd功能相反。
    6. Object.fromEntries和Object.entries功能剛好相反，是把array變回object。
    7. try{} catch{}，try裡面如果不能跑，就執行cath裡面的東西，例如：發出錯誤訊息。catch可以有參數catch(error){}，用來傳遞系統的錯誤訊息。
24. synchronous和asynchronous有什麼不同？JavaScript怎麼運作？
    1. synchronous：browser會從最外層開始讀，然後往上疊，接著讀到最裡層，會疊在最上面，這就是在call stack。接著從call stack最上面開始一行一行執行，執行完成就從記憶體裡刪除，也就是first in, last out。如果在call stack中越來越多，超過分配的記憶體，就會stack overflow。
    2. asynchronous：
       1. 通常是multi-thread的程式設計，需要管各個thread的進程，較不容易理解，非常難寫。
       2. 例如我要同時監聽使用者是不是有按下button，同時監聽向別台server要的資料回來了沒，我要寫程式開兩個thread，兩個分別的loop不斷的監聽目標事件發生了沒，而且要寫發生後該怎麼做，做的順序怎麼按排，誰先發生誰先做嗎？
    3. JavaScript：是synchronous的語言，非常容易理解與好寫，但是執行環境卻提供一些asynchronous的功能：
        1. Web APIs：有DOM (addEventListener)、AJAX、Timeout...等等，JS 是single thread，async的部份就是靠Web API來達到到。
        2. JS sync的部份就是靠call stack來達到，外面的function在底下(firs in last out)，裡面的function在上面，最上面的會先執行，執行完就清掉。
        3. Callback Queue：用來放Web API丟回來的function
        4. Event Loop：會不斷檢查 call stack 結束了沒，結束了才會把 callback queue 裡的東西再丟回 call stack中
        5. 以setTimeout為例：web browser執行到setTimeout這行時，會丟給Web APIs，Web APIs會等時間到，再把要做的事丟給Callback Queue，event loop會等call stack跑完再把事情從Callback Que丟進call stack中。
        6. 我認為關鍵在於js是single thread很好寫，但是想靠著web browser能處理multithread的事情，就需要靠著瀏覽器提供的event loop來處理，Web的api是async的，需要FIFO，所以使用callback queue，JS是sync的，需要LIFO，所以使用call stack，然後利用event loop來監控這些事件，並移動它們，所以JS才能先把async的事情丟出，sync的做事情之後，又很正常的再接住剛剛丟出去，但是已經做完的事情。
        7. 參考演算法第114課。
        8. 參考[到底 Event Loop 關我啥事？](https://medium.com/infinitegamer/why-event-loop-exist-e8ac9d287044)
           1. event loop沒列在es規格書中。
           2. event loop監控call stack，檢查call stack是否清空了，已經清空了之後，才會把callback queue裡的東西丟回call stack中。
           3. [event loop視覺化模擬程式](http://latentflip.com/loupe/)
        9. 更細的地方還有分[macro task和micro tast](https://blog.csdn.net/jbj6568839z/article/details/117392166?utm_source=app&app_version=4.21.1&fbclid=IwAR09KW8PW0V3C8uSgW9tKEPqEpyXNtEtrr6hAI8foXCnCJCmrVR4z5YVXHI)
25. ES2020
    1. bigInt：例如1n, 3242n
    2. optional chaining operator：.?代表用多個object時，某個裡面的attribute，如果沒有這個attribute，就丟undefined，而不會出錯不能跑。
    3. nullish coalescing operator：||有一個用法，就是只要object的一個attribute的type coercion是false，那麼就會用||後面的值當值。 但這會出現一個問題，就是如果那個object的attribute的值是空字串，或是值是false，那麼我們希望的是object這個attribute的值，但是||卻給我們後面的值。??和||的用法一樣，只是只有在沒有那個attribute的時候，才會由??後面的值當成值。
       1. 沒有那個值可能是：
          1. 什麼都沒有
          2. undefined，其實和什麼都沒有一樣
          3. null

## 其它

### [event propagation](https://www.30secondsofcode.org/articles/s/javascript-event-bubbling-capturing-delegation)

1. 有三個phase
   1. capture phase：祖先朝子孫，要用時，在addEventListener的第三個參數設成true
   2. target phase：子孫
   3. bubble phase：子孫朝祖先
2. delegation
   1. 作法：
      1. 利用event propagation的特性，只需要在祖先的地方加上event listener，就可以聽到子孫的event。
      2. 用這個祖先的event handler來處理判斷，要對哪個子孫做反應。
   2. 優點：
      1. 只需要下一個listener，就可以監聽到大量的子孫，不用對每一個子孫下event listener，減少記憶思的消耗。
      2. 可以對動態的elements進行監聽，而且不需要對這個elements進去listener的註冊或是解除註冊。
   3. [由一個實例得到以下幾個原則](https://hsien-w-wei.medium.com/dom-event-propagation-ii-%E4%BA%8B%E4%BB%B6%E5%8F%AF%E4%BB%A5%E5%A7%94%E8%A8%97-event-delegation-ecccb019a48e)：
      1. 不是arrow function的情況下，event handler function的this指的是註冊這個event listener的element，註冊在祖先就是祖先，註冊在子孫就是子孫。
      2. 如果event發生在子孫
         1. 祖先的event.target指的是子孫。
         2. 祖先的event handler會比子孫的event handler晚執行，因為bubble phase發生在target phase之後。
         3. 如果capture phase設成true，那麼祖先的event handler會比子孫的event hanler早執行，因為capture phase發生在target phase之前。
   4. delegation實例參考React的[synthetic event](https://github.com/yellowful/developer-notes/blob/main/web-deveolper/react/%E5%AE%98%E6%96%B9%E6%96%87%E4%BB%B6.md#syntheticevent)

## ES新語法

Lecture 164: ES2020 Part 1
   big int
   nullish coalescing operator ??
   optional chaining operator ?.
Lecture 165: ES2020 Part 2
Lecture 166: ES2020 Part 3
Lecture 167: Exercise: ES2020
Lecture 241: ES2020: allSettled()
   取代Promise.all()，Promise.allSettled()讓resolve和reject都會跑。
Lecture 263: ES2020: globalThis
   globalThis用來統一window和global，讓node和browser的程式碼可以統一
Lecture 263 es2020:globalThis

## ZTM更新

### 2021更新未整理

251,253,254節課clarifai API
Lecture 253: Clarifai API Updates, Models and Troubleshooting
   介紹各種model

Lecture 315: Deploying Our Files
   說明為什麼要從local deploy到雲端
      不用開電腦
      前後端和database都可以在不同的地方

### 2022更新

-------------------------------
Junior to Senior Web Developer Roadmap: 🛤️ 
IMPORTANT: Thinking Like A Senior Developer
Angular vs React vs Vue
-CWD: React + Redux- section
Setting Up Jest
Updating to the Latest Version of Next.js
Static vs SSR vs CSR: Gatsby.js vs Next.js vs React.js
Opt-In Service Worker in CRA

-------------------------------
Complete React Developer in 2022: Zero to Mastery: :react~3: 
Note: we will be adding React 18 lectures when it is officially out!
Create React App - NPX
Create React App - React-Scripts 1
Create React App - React-Scripts 2
Create React App - Everything Else
Hooks vs Classes
React-Redux Hooks
useDispatch
useParams Part 1
useParams Part 2
useState Part 1
useState Part 2
useEffect Part 1
useEffect Part 2
useEffect Part 3
useMemo
useLayoutEffect

-------------------------------
JavaScript: The Advanced Concepts: :JavaScript: 
ES2021
ES2021: any()
