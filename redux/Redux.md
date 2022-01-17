# redux筆記

## 演變的歷程

1. 最早框架是MVC，後來是flux，然後redux是基於flux後的改良實作。
2. MVC：
    1. model：處理數據，有人認為相當於flux的store
    2. view：會改變，也會被觸發
    3. controller：處理資料流向，model和view的橋樑，有人認為相當於flux的dispatch
    4. 缺點：雖然大部分view改變後，是經過conroller去控制model，然後再控制view，但是因為沒有限制model和view之間、view和view之間的溝通，當project越來越大之後，會無法找出那個view觸發哪個model，去連動觸發到哪個view。
3. flux：
    1. 由facebook提出，用one way data flow解決MVC的問題，禁止view被觸發後直接和model溝通，也就是view被觸發後，要透過單一個dispatch把不同的actions分別送給不同的stores，然後才分別去改變不同的view。
    2. actions：不是真正的邏輯，只是一個名稱和payload(資料狀態)
    3. dispatch：控制真正用來控制要不要發出action給store
    4. stores：決定要訂閱那個dispatch
    5. view：顯現DOM
4. redux：
    1. 名詞：
       1. action：要送去store的資料，有type和payload，會記載了發生了什麼event和state是什麼。
       2. dispatch：用來把action送去store的method。
       3. reducer：用來整理資料，讓對應的action，來產生新的對應的state，如此不會去動到舊的state。
          1. 因為app裡面只有state，無法對應正確的type，而redux的action裡面有type和payload，所以reducer可以根據type分配給對應的state，才能給app使用。
          2. reducer裡面最好view邏輯和data邏輯分開。
       4. store：用來把reducer和middleware的資料傳進去，並提供一些method供app使用：
          1. .getState()：取得state
          2. .dispatch()：用來把event和state(也就是action)給store，來更新state。
          3. .subscribe()：用來註冊listener。
    2. 基於flux的one way data flow，但是有以下幾點和flux不一樣：
        1. 單一store
        2. 多個dispatches來送出actions
        3. 多了一個reducers來整合決定要送出哪一個actions
        4. 舊state不能更改，只能一直產生新的state。
    3. 這樣的好處是：
        1. 單一store單一來源，好管理。
        2. 舊state不能更改，所以可以回朔，容易找錯。
        3. 加上pure function，容易預測結果，所以容易管理。因為reducer被控制裡面必須是pure function，連random或date都不能有。

## redux基本觀念和原理

1. redux用來處理大量state
2. 原本是用來解決database的大量query的問題
3. 原理是把不同階層的state都存到state store中，然後靠著集中管理這些state，就可以控制整個app的顯示狀態
4. redux只管理container，不管裡component。
5. 所有的state都是不可變的(inmutable),狀態改變都是新產生一個新的state，比較不會去錯誤動到state，讓資料傳遞是個one way data flow。
6. redux是用傳統的javascript回傳state和action
    - action
    - state
7. 傳入redux的function connect，來控制app
8. 0305更新：
   1. react是decenterized的概念，component之間可以decouple，讓越多人合作，專案可以越快速，而程式也比較容易維護。
   2. 但是decenterized也有缺點，所以centerize的redux就可以解決這些缺點，例如橫跨多個component之間的state，需要傳好幾層到祖先那一層，再傳下來，所以狀態很難追蹤和管理。redux集中管理，狀態好追蹤管理。
   3. react的state無阻止改變state的機制，redux則不能改變原來state，一切都是immutable，所以比較不容易出錯，甚至可以歷史回朔。
9. 和redux一樣的其他狀態管理的函式庫，例如MobX。
10. 好文章：<https://ithelp.ithome.com.tw/articles/10187762>
11. redux的三大原則：讓state比較predictable
    1. single source of truth：所有重要在意的state的javascript都集中在同一個地方。
    2. state is read only：所有state都是唯讀的，這樣可以做到很多很酷的功能，例如回溯。像是時光機器一樣，在很大的很複雜的系統中，竟然可以回溯到某個時間點，去看看是發生了什麼錯誤。
    3. changes using pure functions：這樣不會產生side effect，容易偵錯。

## 實作最重要的原則

1. 要寫出一個reducer的function：
    1. 輸入一個舊state，和一個新action。
    2. 一個action就是一個object，這個object會包含一個type和一個payload，type可以想像成改變新狀態的名稱，payload可以想像這個新狀態的值。
    3. 會根據不同的action.type，輸出對應的舊state和對應的新state。
    4. 一個action.type可以對應多個state。
2. react-redux的connect會把mapDispatchToProps的dispatch去和redux的store的dispatch連在一起。
3. store的dispatch會把收到的action object丟給reducer，reducer會把對應的新舊state再丟回store。
4. react-redux的Provider會把store裡的new state轉成props傳給App。
5. async最重要的是，actions.js裡的function，要回傳一個function，而不是action object。

## 一般call back或sync function實作

### 先做一般javascript在做的事

1. 要安裝的library，都要引用在index.js裡的
    1. npm install redux
        要裡面的createStore,combineReducers,applyMiddleware
    2. npm install react-redux
        要裡面的Provider給index，和connect給App
    3. npm install redux-thunk
        要給promise用的
    4. npm install redux-logger
        要裡面的createLogger
2. 用傳統的javascript準備要給redux的資料
    1. 弄個constant.js的檔案來存strings
        1. 主要是用variable比直接用string容易除錯
        2. 用來記錄action的名稱
    2. 弄個action.js
        1. 定義setSearchField這個function用來回傳一個object
            type:import的constant,
            payload: 接受了這個function的arguement，（有可能會做一些處理後，）產生的新state
        2. 然後把這個function export出去
        3. action是用來記錄那些被event或promise或call back觸發，而接著動作狀態的名稱。
        4. 隨著action的增加，可以增加action和constant state
        5. store會儲存所有的state，然後負責更新view
        6. store配合dispatcher，把舊的state和action送給reducer，
        7. reducer會吐出新的states，再送回stores
        8. stores會再用新的props更新view
        9. actions.js
            1. 裡面主要要export一個object
            2. 這個object有action的命名，和state(payload)。
            3. 還可以放其他東西，參考：<https://github.com/redux-utilities/flux-standard-action>
        10. 在App.js裡要import actions
    3. 再弄個reducers.js
        1. 要有initialState
        2. reducer裡面主要要有一個function，這個function要能輸入兩個變數，一個變數是state(是舊的)，另一個變數是相對應的action。
        3. function裡有一個switch：
           1. 會依據輸入的action名稱，return相對應的新state。
           2. 事實上就是return一個object，裡面有原來的state，和要更新的property，也就是action.payload
           3. 其中return必須用Object.assign()，這很重要，因為state不能mutate，必須是read only，所以一定要用object.assign，或是object spread operator。
           4. default是原來的state

### 進入redux library在做的事情

1. 最重要的概念是，把原本app裡的state刪掉，改由爸爸傳來的props來取代state，而爸爸的props就是由redux來統一控制和下傳。
2. index.js
   1. 先import
        1. react-redux: {Provider}
        2. redux-thunk: thunkMiddleware
        3. redux-logger:{createLogger}
        4. redux：{createStore,combineReducers,applyMiddleware}
        5. 有大括號的意思是library裡面已經取好名字了，不能自己亂取。
   2. 用createStore(reducer)來放這些state
   3. 然後redux會把store當成props傳給app
   4. 原本有.subscribe來給container component訂閱props，和store.getState可以看到state的狀態，但是這樣手刻太累，現在都用connect和Provider來做這件事。
   5. 利用`<Provider>`包住`<App>`，可以讓store這個props自動傳下去
    所有要用redux的container component(又稱smart component)都要用Provider包住
3. app.js
    1. 要`import {connect} from 'react-redux'`
        是用來取代.subscribe，讓各個container知道自己的state變了。
    2. `export default connect(mapStateToProps,mapDispatchToProps)(App)`
        1. 把App丟進去connect裡，會得到一個需要兩個parameter的call back function，當這兩個parameter有值的時候，connect才會被invoke。
        2. 如果store裡面的state有變化的話，connect會通知App。
        3. mapStateToProps和mapDispatchToProps是讓connect知道哪些state和App有關。
    3. mapStateToProps
        1. 把store裡面的state轉成這個app裡面的props
        2. 會是一個arrow function
        3. 輸入reducer丟出來的state，也就是一個object
        4. property名稱: state.對應的reducer.state的名稱
            (如果只有一個reducer，reducer要省略)
    4. mapDispatchToProps
        1. 把store裡面的dispatch(event listener)轉成這個app裡面的props
        2. 也是一個arrow function
        3. 輸入dispatch，回傳一個包含所有function property的object。
        4. dispatch是store的一個method，用來把action object傳給reducer
        一個function property長成這樣：
            `{property名稱:(event)=>dispatch(action名稱(event.target.value))}`
        5. 也就是說每一個property的屬性也會是一個arrow function，這個arrow function會回傳一個dispatch，dispatch裡面又會有一個function，正常來說這個function會回傳一個action object，但是若這個function是一個async function，那就要讓他回傳一個function，然後丟給redux-thunk來處理。
        6. 這個property的arrow function，就是會把function傳給onChange的property
        7. event是一個onChange回傳的事件
        8. event.target.value會被當成text傳進actions.js的function裡，這個function會回傳一個action object放到dispatch裡。也就是說dispatch裡面的setSearchField最後會是actions.js回傳的object。
        9. 但是還沒fulfilled之前是一個function，一旦fulfilled了，才會回傳object
        10. 如何fulfilled？就是把event.target.value當成action裡的text的parameter傳進去。
        11. 接著，要把程式裡面用到state的部分，改成this.props.state
        12. 裡面的callback要觸發的function，改成this.props.function
4. 參考：
    <https://medium.com/4cats-io/%E6%B7%B1%E5%85%A5%E6%B7%BA%E5%87%BA-redux-7b08403c4957>

## async和middle ware的實作

1. 顯示action和state：
    1. redux logger
        1. `import {createLogger} from 'redux-logger';`
        2. import redux裡的applyMiddleware
        3. 把applyMiddleware(logger)放到createStore裡面。
    2. 另一個更好用的chrome extension：redux DevTools
2. 如果是async function的redux，就會有對應三種action，而且要用middle ware來處理：
    1. rudux thunk：
        1. 判斷傳的是值還是function，如果是function，等跑完才改state，然後才render
        2. 在applyMiddleware裡，放在logger之前
        3. 用在async
        4. 會有三個type：pending,succes,failed
        5. 在actions.js裡
            1. import這三個const
            2. 然後action需要傳dispatch進去
            3. 三種狀態之下，分別呼叫dispatch，dispatch的argument會是action，action就是一個function回傳的object
            4. 這個object裡面有type和payload
            5. 這個object需要放在dispatch裡面
            6. actions.js的function裡面，fetch之前先dispatch pending
            7. fetch成功就`dispatch({success，和payload(就是state)})`
            8. 錯誤就dispatch failed
        6. 接著reducers.js裡增加reducer，在這三種狀態下switch，並設定一個新的state，isPending。
        7. 在App.js中
            1. mapStateToProps放入{三個props:對應的三個state}
            2. mapDispatchToProps放入{props:arrow function}，這個arrow function要把dispatch注入action的function裡面。(注意，後面會補充要改寫成higher oder function的寫法)
            3. 原本放這個async function的地方就不用放了，可以直接放{this.props.這個新props}
        8. 其中index.js裡的thunk會發現actions裡面回傳的是一個function，而不是object，它就會先dispatch pending的object給redux，然後繼續listen，等到資料回來了，才把dispatch一個包含sucess和data的object，給reducer。

    ---

    **最難的地方**的補充說明

    1. Async原本寫法：
        1. mapDispatchToProps

            ```javascript
            property:()=>requestRobots(dispatch);
            ```

            這個property是一個function，回傳requestRobots(dispatch)，也就是把requestRobots注入dispatch，目前還不知道這個會回傳什麼，但是之後這個區塊要改成回傳一個dispatch function，而dispatch function的arguement不是一個object，是一個function，目前還沒改。
        2. actions.js

            ```javascript

            const requestRobots = (dispatch) => {
                dispatch(pending action);
                dispatch(sucess action);
                dispatch(failed action);
            }
            ```

    2. 要讓thunk用的話要改成這種寫法：
        1. mapDispatchToProps

            ```javascript
            props:(parameter)=>dispatch(requestRobots(parameter)));
            ```

        2. actions.js

            ```javascript
            const requestRobots = (parameter) => (dispatch) => {
                dispatch(pending action);
                dispatch(sucess action);
                dispatch(failed action);
            }
            ```

        3. 上面的code建立了一個higher oder function，也就是a function return a function。
        4. 這個arrow function就是原來react會造成state變動的function。改寫成props的function時，如果原本的function需要傳parameter進去，就照著傳進去。
    3. 這種寫法requestRobots會回傳一個function，redux thunk發現回傳的是一個function，就不會把function傳給redux，redux thunk就會先把dispatch丟下去，讓他回傳不同的action object回來，thunk再分別丟給redux。

    ---

3. 用redux的combineReducers來將兩個reducer合併到rootReducer，**argument需要用一個大括號包起來變成一個object**，再把rootReducers放到createStore裡

4. 多個container的時候，就用connect把這些container連接起來。

5. 結論：
   1. redux藉由這種方式，集中管理state和promise
   2. 不需要constructor來設定初值了，也不用在componentdidmount裡先跑function了，通通放在redux裡面去run

## 實際觀察內部過程(console.log出來)

1. app一load的時候
    1. 因為action還沒有任何動作，所以先直接跑reducers.js
        在switch裡面因為action.type還不是我們要監聽的值
        所以default state是初始值
    2. index.js
        store也就開始儲存初始值
    3. 藉由Provider和connect開始連接並向下傳到app.js
        mapStateToProps裡面的state會被放入props的object
        mapDispatchToProps派出action，這個action開始監聽event
    4. 第一次render
    5. 第二次render
        componentDidLoad之後沒有狀態更新，所以action,reducer,store都沒動靜
2. 狀態更新時：
    1. actions.js
        監聽到event
        text是新的
    2. 推測：
        dispatch把action丟給store
        store丟給reducers.js
    3. reducers.js
        輸入舊的state
        輸出新的object，裡面有payload
    4. 推測：
        reducers又把state丟給store
        store把state轉成props丟出來給app.js
    5. app.js
        mapStateToProps，裡面是新的state
        render新的state
3. 結論：
    1. 第一次跑時
        1. 先設初始值
        2. createStore、connect、Provider把app連接起來
        3. 把listener和初始state當props傳下去。
    2. 狀態更新時
        1. action先聽到新的state
        2. 由app傳舊的和新的state給index
        3. 再傳給reducer
        4. reducers再傳出新的state
        5. 然後傳到App變成props
        6. 然後才render
4. 實作celerec後的心得：
   1. 其實redux沒有那麼複雜，之所以那麼複雜，可以想像是我們要去每一個function裡面console.log出每一個function和這個function的state，來追踨所有的state，只是大神用了一個較有系統較有效率的方法來做這件事情而已。
   2. 繼MVC之後，最流行的方法是flux，flux就是 one way data flow，而redux就是讓react可以符合flux的library。
   3. react不符合flux嗎？react不是用state和props來處理，從parent到children地用one way data flow寫程式嗎？react已經盡量的flux了，至少component的props的傳遞上。但是就component內部的data邏輯(或是fetch啊之類的)，並沒有要求一定要pure function，而且function之間可以呼叫來呼叫去，已經可以造成一個event產生無法預期的多個state的變化了。
   4. redux如何讓react達到flux的設計方式?
      1. redux讓所有想觀察的state都和一個獨一無二的type綁在一起，成為一個action，然後經過一個reducer的整理。這種action的設計讓你想改變state的話，一定得通過dispatch action才能改變，這種dispatch action的方式，redux會保證state是read only。
      2. reducer的做用是把一個你想觀察錯綜複雜的function的state，轉換成App要用的state的pure function。
      3. 因為reducer是一個pure function，就算原來你的邏輯functions錯綜複雜，reducer也是一定把一個個functions dispatch出來的actions依順序的排列好，排好、轉成App要用的state才存入store裡面。
      4. 這個從action轉換成state的過程，不能更動原來的state，一直到reducer才更動state。
      5. 然後再加上single source of truth，也就是整個app只能有一個store，這時候從action被dispatch開始，到reducer排列轉換成state，到儲存到store裡面，到component向store取用props，就達成flux的設計。
      6. 程式碼
         1. 檔案：其實actions.js就是app.js的一部份，reducer.js就是index.js的一部份，app.js其實就是index.js的一部份
         2. 資料路徑：
            1. index.js：`redux(createStore)=>actions=>reducers=>state=>provider=>App`
            2. app.js：
               1. `redux(dispatch)=>actions=>mapDispatchToProps=>redux(connect)=>App=>props`
               2. `redux(store)=>state=>mapStatesToProps=>redux(connect)=>App=>props`
            3. connect和createStore是通過redux連在一起的。
      7. 即使原來的functions之間錯綜複雜，其於flux，我們仍然可以非常清楚的觀察到是哪一個action造成哪一個state的改變，順序和過程是怎樣，state好管理，程式就不容易有bug了。
   5. connect是一種curry和closure。
   6. connect()()，左邊先傳入state(要哪些props)給store，然後回傳一個記住state的function，再傳右邊app進去這個function裡面，會回傳針對這個state設定修改過的app回來。
   7. 要用curry分兩段的原因是因為要先決定要訂閱哪些props，用closure的特性記住狀態，才能決定要把原來的app改成有哪些props的新app.
   8. action：
      1. 把改變的state和造成state改變的原因綁在一起，這個原因就放在.type裡面。
      2. action是app.js的一部分，也就是給mapDispatchToProps的一部分，只是分開一個檔案會比較清楚明瞭。
      3. dispatch是用來把action(一種有type和payload的object)丟給store的，如果裡面不是action，而是function，就會丟給thunk。所以action會看到兩種不同的寫法，一種是直接return object，這就是丟給store的寫法。另一種是用dispatch來丟action的寫法，這種就是丟給thunk的寫法。
   9. reducer：
      1. 把所有state統整在一起
      2. 呼叫舊的state，放入新的state
         1. 之所以要從store拿舊的state出來，是因為才能從舊的state的結構中，去改變一小部份state，改變完就是新的state了。
            1. `Object.assign()`的語法，後面那個參數的object，如果key和前面object相同，就只會整個大object裡面這個key的value。
            2. `Object.assign()`的語法還能保證，雖然只有動一個key value pair，但是是產生一個新的object，所以符合state is read only。
         2. 新的state的改變，是根據action的type名稱(也就是改變state的原因)，來決定更新哪個state。
      3. return之後，新的state會存入store之中。
      4. 之所以要存入store當中，是因為這樣，store就存有所有的歷史記錄，而且是one way data flow，可以倒轉的。
      5. reducer是index的一部分程式碼
   10. 其實功能最強大的是thunk，redux只能做很基本的。(最重要的一點)
       1. action裡面任何想加dispatch的地方都可以加，可以當成console.log來用，只要附上獨一無二的type。記得reducer裡面，要可以加上對應的type的state。
       2. action裡面要呼叫其他的action，要用dispatch包住。
       3. action裡面沒有用dispatch，直接用return一個object，那就是沒有透過thunk，而是直接透過redux。
   11. 總結：
       1. 和以前會流行的mvc一樣，現在流行的是flux，flux就是用one way data flow來減少bug的發生。
       2. react就是用flux來解決view的問題，用props傳遞來達成one way data flow。
       3. redux就是用flux來解決view以外的邏輯問題，用action、reducer 、store來達成one way data flow。

## 官方文件

### split reducers

1. [網址](https://redux.js.org/usage/structuring-reducers/splitting-reducer-logic)
2. purefunction原則，將function拆分成小的function。
3. 可以拆分的function符合以下其中一種情況就可以拆分：
   1. 可在不同地方重複使用的邏輯。
   2. 用來需要參數來處理特定更新的，這個參數不是單純的state和action組合。
   3. 依一般的state和action組合
4. [影片](https://www.youtube.com/watch?v=JUuic7mEs-s)
   1. action的種類：
      1. event：notify of a change 如此才能對這個改變做出反應。
      2. command：invoke a procedure 例如一個handler或是等一個reply
      3. document：transfer data，不會有回覆，例如一個reducer把資料從payload轉到一個state
   2. routing patterns：
      1. filtering：篩選要的
      2. mapping：從一個function丟到另一個function
   3. transform patterns：
      1. translator：把action type改成人類較易懂的type
   4. 文章：[Why Senior Devs Write Dumb Code and How to Spot a Junior From A Mile Away](https://hackernoon.com/why-senior-devs-write-dumb-code-and-how-to-spot-a-junior-from-a-mile-away-27fa263b101a)
      1. 在junior的時候覺得很奇怪，為什麼senior dev code很容易懂？
      2. 雖然dumb code(寫了一堆看起來不是很必要的code)很容易懂，但是寫dumb code很難，作者曾努力去學寫出那種dumb code，但是都寫不出那種dumb code。
      3. 直到用上他所學的一堆理論，YAGNI、single responsibility、DRY、Single Level of Abstraction Priciple、low coupling…等等，才讓code變的dumb，這時他已經變成senior dev了。
      4. 他認為寫出dumb code事實上很難，需要花上加倍的努力才能做到。
      5. 如何在一英哩外嗅到菜鳥寫的code？裡面充滿了聰明的一行程式搞定一堆邏輯、不容易懂的抽象化、大量的語言特性。最後一個是特別常見的，它像是在說，看過來，我的創造者非常了解這個語言。
      6. 這樣的寫法是不合理的，因為他只考慮到電腦面的事情，沒考慮到人類這一面的事情。
      7. 程式碼原本是關於和其它人溝通以及給予電腦指令，但是現在前者比後者重要多了。
      8. 編譯器會搞定把程式語言翻譯成機器懂的語言，甚至會透過好幾層的翻譯，最後翻成0和1。
      9. 但是程式碼是人類語言，他必需去溝通任務的who、what、when、where、how、why，當然也需要命令電腦。它需要讓五年後沒看過這個code的人能夠了解、改進、修bugs。
      10. 作者愈來愈愛寫dumb code，有人code review之後對他說clean code，他會超開心。他知道做對團隊或是未來維護程式碼最棒的事，就是寫dumb code。
   5. `22:37`：可以看到他的type有分類，也就是string最開頭加上`[ui]`、`[order]`這些分類說明。
