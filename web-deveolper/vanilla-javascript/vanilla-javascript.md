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
   * .innerHTML也是屬性，可以改變內文，內文也可以加tag。
   * 要選父親是.parentElement，要選子卻是用.children，不用Element。
   * 使用時儘量存在變數裡，不要一直select，可以加快chrome速度。
9. shopping list心得：
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
        1. 觀念非常重要，指的是所在scope的父類別的物件，注意是物件，不是類別。例如，JavaScript內建的一些函數，前面可以加上this，這個this指的是windows物件。
        2. 「所在scope的父物件」指的是，this寫在某個class中，用這個class可以instancia出一個object，這個object可以用一些method，這些method是在class裡面定義的，所以this.method()，的this指的是這個class實體化的object，這個method所在的scope的爸爸class的object。
        3. React中，class component如果要把爸爸component的state或function傳進來，就要用props從super傳進來，在這個class裡要用的時候，就要加上this，例如this.props，這個this指的是這個class實體化的component object，而不是爸爸component。
        4. 有一種情況，爸爸class中有this，小孩class中也有this，而且小孩引用了爸爸的某些attribute或method有this。
            1. 當用小孩的class去實體化一個object的時候，小孩class和小孩class裡引用到爸爸的this，全都是指這個小孩object。這一點是很容易弄錯的，要小心。
            2. 當用爸爸的class去實體化一個object的時候，爸爸class裡的this指的當然就是這個爸爸object。
        5. react無法把資料上傳到爸爸component，但是看起來很像把event上傳給爸爸，其實是從爸爸component下傳一個function作為property，這個function會在小孩component裡面成為property的一個attribute。讓這個function放在DOM的一個call back property裡面，當作call back function，所以在爸爸那邊，這個function就可以listent 小孩call back function的events了，爸爸這邊也可以根據聽到的event更新state。
        6. 如果code呼叫太多this，可以用destructuring，讓程式碼比較clean。
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

                  ```JS
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
            2. 這個short hand不是destructuring，destructuring是設定新變數，這個short hand是指定value給key。
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
    2. asynchronous：通常是multi-thread的程式設計，需要管各個thread的進程，較不容易理解。
    3. JavaScript：是synchronous的語言，非常容易理解，但是執行環境卻提供一些asynchronous的功能：
        1. Web APIs：有DOM (addEventListener)、AJAX、Timeout...等等。
        2. Callback Queue：用來放Web API丟回來的function
        3. Event Loop：會不斷檢查 call stack 結束了沒，結束了才會把 callback queue 裡的東西再丟回 call stack中
        4. 以setTimeout為例：web browser執行到setTimeout這行時，會丟給Web APIs，Web APIs會等時間到，再把要做的事丟給Callback Queue，Callback Que會等call stack跑完再把事情丟進call stack中。
        5. callback queue負責 async 是 FIFO，call stack 負責 sync，是 LIFO。
        6. 參考演算法第114課。
25. ES2020
    1. bigInt：例如1n, 3242n
    2. optional chaining operator：.?代表用多個object時，某個裡面的attribute，如果沒有這個attribute，就丟undefined，而不會出錯不能跑。
    3. nullish coalescing operator：||有一個用法，就是只要object的一個attribute的type coercion是false，那麼就會用||後面的值當值。 但這會出現一個問題，就是如果那個object的attribute的值是空字串，或是值是false，那麼我們希望的是object這個attribute的值，但是||卻給我們後面的值。??和||的用法一樣，只是只有在沒有那個attribute的時候，才會由??後面的值當成值。
       1. 沒有那個值可能是：
          1. 什麼都沒有
          2. undefined，其實和什麼都沒有一樣
          3. null
26. ZTM可能有更新的地方：
    1. 第1節course outline
    2. 77節課更新網址
    3. 84節property名稱改變
    4. 97節solution:Prettify
    5. 99節career of a web developer
    6. 144節es5和es6
    7. 146節advaced functions
    8. 164節課ES2020
    9. 第187堂 GitHub master => main
    10. 201節課latest node.js NPM
    11. 225節課excise react 17
    12. 230節課react hooks+redux
    13. 241節課ES220: allSettled()
    14. 251,253,254節課clarifai API
    15. 259introduction to node.js
    16. 263 es2020:globalThis
    17. 266 ES6 Modules in Node
    18. 269 Introduction to Express.js
    19. 304 setting up your database
    20. 315 deploying our files
    21. section 31 extra learning: react hooks
    22. 353 redux toolkit
    23. 360 popular tools for react + redux
    24. Andrea：
        Lecture 164: ES2020 Part 1
            big int
            nullish coalescing operator ??
            optional chaining operator ?.
        Lecture 165: ES2020 Part 2
        Lecture 166: ES2020 Part 3
        Lecture 167: Exercise: ES2020
        Lecture 201: Update to Latest Node.js and NPM
        Lecture 204: Why Update Packages?
            github security alerts會顯示需要更新的版本
            <https://semver.npmjs.com>
                可以看版本穩定度
                ^: 某一大版
                >=: 大於等於某一版
        Lecture 224: Keeping Your Projects Up to Date
            npm update
            手動
            --force
        Lecture 225: Exercise: React 17
        Lecture 226: Solution: React 17
        Lecture 241: ES2020: allSettled()
            取代Promise.all()，Promise.allSettled()讓resolve和reject都會跑。
        Lecture 253: Clarifai API Updates, Models and Troubleshooting
            介紹各種model
        Lecture 259: Introduction To Node.js
            有：
                global
                process
                process.exit()
            沒有：
                window
                global.fetch
                document
        Lecture 263: ES2020: globalThis
            globalThis用來統一window和global，讓node和browser的程式碼可以統一
        Lecture 266: ES6 Modules In Node
            可以用import了
                方法1：在package.json裡面加上`"type": "module"`
                方法2:副檔名改成mjs
        Lecture 315: Deploying Our Files
            說明為什麼要從local deploy到雲端
                不用開電腦
                前後端和database都可以在不同的地方
        Lecture 339: React Hooks
            說明作為一個developer，無可避免遇到工具演化，要如何面對呢？
        Lecture 340: React Hooks 2
            第一要了解動機：
                1. 類似的component難以重新利用，所以才會寫wrapper component，來解決這個問題。但是一堆wrapper仍然會讓程式碼很難讓人了解。
                2. class component太複雜了，還有一堆lif cycle，hooks的方式比較讓人容易了解。
                3. class不好用，hooks只需要function，讓做出來的component多了很多的可能性，可能和angular、可能和其他東西共用。
        Lecture 341: React Hooks 3
            npm update
        Lecture 342: React Hooks 4
            看著document自己改成hooks
        Lecture 343: React Hooks 5
            把constructor刪掉
            把render刪掉
            最後帶出如何處理this.state？
        Lecture 344: React Hooks 6
            hooks裡的array是javascript的新功能，是一種destructure，讓我們可以替state和setstate命我們喜歡的名字。
        Lecture 345: React Hooks 7
            本身state的更新，會讓自己本身render。
            useEffect：
                function component載入的時候會render，更新的時候也會render，而useEffect如果沒有設定第二個arguments的時候，一旦state更新，本身會rerender，所以useEffect裡面的function也會又render更新state，然後就變成無限迴圈了。
                第二個arguments
                    如果第二個arguments有的話，第二個arguments有變動才會跑第一個arguments裡的function。
                    設成[]，永遠都不會變動，所以只有component載入時會跑第一次。
        Lecture 346: React Hooks 8
            證明了第二個parameter變動的時候，第一個parameter的function會run
        Lecture 348: React Hooks 9