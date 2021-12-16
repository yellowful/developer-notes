# redux from junior to senior

## 上課

1. 概念：
   1. 可以減少rerender。
   2. state可以改變，props不可以改變，在React底下，當App愈來愈大，會有很多container component，也就是有很多state需要追踨，因為他們會改變，這造成追踨困難。而redux把state集中在一個地方管理，而且用sql的技術良好管理。
   3. 第103課state management：React很擅長處理view，但不見得很擅長處理state，處理state是redux的專長。
   4. flux pattern：
      1. action => dispatcher => store => view
      2. action就是event (value)，dispatcher就是reducer這個pure function，store會把state變成props丟出來，view就是react
   5. mvc：
      1. action => controller => model => view
      2. action就是event，controller就是javascript，model可以看做state，view就是結果。
      3. 其中controller會產生一大堆沒規律難以追踨的state，這些state會改變view，但是這些view可能又改變了state而去改變了view，很難追踨。 
   6. 總之redux就是用來取代this.state的。
2. 實作注意事項：
   1. 用redux toolkit可以協助快速建立redux，不用寫那麼多code。
   2. reducer：
      1. single source of truth：所以要從store拿state出來
      2. state is read only：所以用Object.assign()
      3. **chages** using pure functions：reducer這個function沒有side effect，只需要return新的state。
      4. return的`Object.assign({},state,{searchField:action.payload})`中，其中的state是整個store的大object，這一行程式，是對整個store複製一個大object，但是這個大object只有searchField這個attribute不一樣，有變動。
      5. default是return state，這就是原來的大object。
      6. rdeucer可以看做是一個controller，用來吃action吐state。
      7. redux API對reducer做的事：
         1. 丟state大object和action進去。
         2. 接收reducer丟出來的新state，把新的state存在store裡面。
      8. [一個state分給兩個reducers](https://chentsulin.github.io/redux/docs/FAQ.html#reducers-share-state)：
         1. 標準的flux pattern是多個獨立的store，redux是用多個獨立的reducer來處理。
         2. combineReducers**不允許**一個state分給不同的reducers。
         3. 官方提供幾個方式處理這種情況：
            1. 假如有一個reducer需要知道其它的state的資料，那麼state tree就必需被重新組織，讓一個reducer可以處理更多的資料。
            2. 可能需要自己寫function來處理這些actions，以取代combineReducers。
            3. 用thunk處理async
            4. redux鼓勵拆分reducers，拆成比較小的reusable的reducer composition。
            5. 也可以傳一個自定的argument進去parent reducer給自定的children reducers用
            6. reducers非常彈性，只要確定追尋基本的規定：
               1. `(state, action) => newState`
               2. immutably
            7. [reducer要拆嗎](https://chentsulin.github.io/redux/docs/FAQ.html#actions-reducer-mappings)：
               1. 小小的reducer處理少少的states是比較好的。
               2. 至於一個action可以不對上任何的reducer，透過applyMiddleWare自己丟進去store，或是對上很多個reducer。
               3. 這樣做等於是把action和reducer分隔，一個action可能受state tree不同部份的影響，但是component可能並不需要受影響。
            8. 心得：
               1. reducer是在處理view的部份，action在處理邏輯的部份。
               2. reducer把actions的payload轉成store的state，mapStateToProps是把store的state轉成component的props。
               3. 其中store的state並不需要所有的都要轉成component的props，因為有的state只是需要直接傳到後端去，這時候就不需要放進mapStateToProps，也就是說，只有view用得到的 state才需要放進去mapStateToProps。
               4. 我想dispatch應該也是，只有view要用到的actions才需要放進mapDispatchToProps，其它的放在actions裡供其它actions呼叫就好了。
      9. [structure business logic](https://chentsulin.github.io/redux/docs/FAQ.html#structure-business-logic)：不一定商業邏輯全部放在actions裡，reducer只做最簡單的update states，也可把一部份商業邏輯放在reducers裡，這是可以分配比重的，只要reducer遵守上面說的pure function的基本規定。
      10. 跟我原本直覺一樣，總之，不可能相同的state放到不同的key value pairs裡面，所以最簡單的方式就是拆分reducer時要注意拆法，不要把相同的state拆到不同reducer了，甚至可以在寫actions時就先把actions拆好。
   3. redux：
      1. redux：
         1. createStore：用來丟reducer進去
         2. store可以直接當成props傳給app印出來，裡面是個大object，用getState()可以得到state。
         3. Provider：但我們通常會把store放到Provider的props裡。
      2. react-redux：
         1. 主要用Provider和connect來取代.getState()和.subscribe()，.getState()可以取得要的state，.subscribe()可以取得要訂閱props的component。
         2. Provider：用來把store裡的state轉成props下去，給所有的子孫。
         3. connect：
            1. higher order function：第一個括號run完會回傳另一個function，這個被回傳的function才會把第二個括號run完。
            2. 主要是說，我要訂閱redux store所有的state變化。
            3. mapStateToProps和mapDispatchToProps是可以自己命名的，用來告訴connect，我要訂閱什麼state和什麼dispatch。
            4. 在flux裡面dispatch的意思是，觸發action的東西。
            5. action就是一個有名稱有值的object，需要被dispatch送去給reducer。
            6. mapDispatchToProps範例：

               ```js
                  const mapDispatchToProps = (dispatch) => {
                     return {
                        onSearchChange:(event)=>dispatch(setSearchField(event.target.value))
                     }
                  }
               ```

            7. 上面的範例裡：
               1. mapDispatchToProps是一個object，所以function要return一個object
               2. 這個object的一個attribute就會放一個function，這個function被觸發的時候會變成一個object。
               3. dispatch()裡面要放action的object，所以setSearchField就會回傳一個object。
               4. onSearchChange這個attribute放的就是onSearchChange這個callback，所以會被event觸發，所以parameter會是event。
               5. 被觸發後要把action丟進dispatch裡面去，所以需要import一個function叫setSearchField()，只要把event.target.value丟進這個function後，就會把一個action丟出來。
            8. 一個container component對應一個connect
      3. 可以只有部份state和redux連接，例如課程第109堂redux connect，就只有先把searchField的state變成redux，robots還是this.state，還是可以用，但是要注意的是，這就不符合single source of true，可能造成debug困難。
      4. database會遇到一堆read和write的要求，同時又要減少bug和錯誤的發生，所以redux就用類似database處理資料的方式處理state，就可以非常好的整理state。
      5. middleware：
         1. thunk：
            1. 一般function是return一個action object，async function裡面一開始只有dispatch不同的action，沒有return任何東西。
            2. mapDispatchToProps：
               1. 例如`onRequestRobots:()=>dispatch(requestRobots())`
               2. 其中，onRequestRobtos這個props會是一個function，執行的時候會dispatch一個function出去，這個function叫作requestRobots()。
               3. 而requestRobots()回傳需要是一個function，redux thunk才會等他，所以在action那邊，requestRobots()會需要是一個higher order function才能回傳一個function。
               4. 這個被回傳的function會被thunk丟dispatch進去parameter，然後這個function要把action放到dispatch裡面，thunk就會把這些action丟給reducer了。
      6. folder：
         1. 通常實務上會一個container一個folder，裡面裝了相關的representive component和相關的reducers和actions。
         2. 這樣會很容易分享這個component，也很容易思考，和找bug。
         3. 實務上也會有一個API的資料夾，裡面可以有function讓我們可以把url丟進去，就獲得資料，不用一直repeat .then這些。
3. [redux的performace改善](https://chentsulin.github.io/redux/docs/FAQ.html#actions-multiple-actions)：
   1. 避免一個sync action帶有一系統超多的dispatch
   2. [redux batched actions](https://github.com/tshelburne/redux-batched-actions)
   3. [redux batched subscribe](https://github.com/tappleby/redux-batched-subscribe)

## 實作心得

1. `actions.js`裡面的functions就是所謂的actions嗎？
   1. 這些function return出來(或是dispatch出來)的type payload組合才是action。
   2. 這些function叫作action creator。
2. 為什麼要分action和reducer呢？action creator不是就可以dispatch action了嗎？
   1. 把action creator和reducer分開主要讓action和reducer的邏輯可以分開處理。
   2. action負責modal和controller的邏輯部份，reducer負責view的state邏輯的部份。
3. 相同的state可以放在不同的reducers裡嗎？
   1. 不行。
   2. 不同的reducer有相同state的話，你在map state to props的時候，你的state要訂閱哪一個reducer的state呢？就無法選擇了。
   3. 各個reducers的states要相互獨立，所以initial state要各別分別設定，設一起的話會變成不同的reducer會有相同的state。
4. action裡相同的type，可以放在不同的reducers裡嗎？
   1. 可以。
   2. 同一個type可以影響到很多不同的states，這些states如果分佈在不同的reducers裡，那麼這些type就可能出現在不同的reducers裡面，然後把這些payload送給這些不同的reducer的state。
5. action.js裡的function一定要return或是dispatch actions嗎？
   1. 不用，這裡面的function可以是純邏輯，單純用dispatch去呼叫其它function。
   2. 需要觀察的地方才return或是dispatch對應的action就好。
6. 由上面幾點可以看得出雖然action creator需要把action丟出來給reducer處理，但是這兩者並不需要相互對應，是可以完全decouple的，所以可以不用把modal和controller的邏輯和view完全的綁在一起，卻又可以利用action把modal和controller的邏輯經過reducer和view相互對應在一起。
7. redux這樣做容易找到bug的原因在於。
   1. 我們發現view不如預期時，可以從redux的工具裡看到各個action和state的順序，發現是卡在哪一個state或是action，可以由type去找到相對應的function或是action creator的問題。
   2. `state正不正確 => 對應的action發生順序正不正確`
8. 我覺得主要邏輯放action比放reducer好一點，如果某一個payload或state的邏輯兩邊都放，這樣會造成以後改code的時候，兩邊要同時改，容易漏掉。
9. 把form和redux連接：
   1. 最複雜的地方是這樣流的，兩條路線：
      1. 把相關的action creator丟給mapDispatchToProps，丟進connect裡面。
      2. 把action creator丟出的actions丟給reducers，reducer會轉成state，然後到index.js裡丟給store，connect API會把state訂閱下來，變成這個component裡的props。
   2. 可以把App.js裡有關Form的部份的actions和reducers搬過來Form這邊了，原本要透過props傳給App.js再傳給Form的，就直接透過props傳給Form。
   3. 把App.js用不到的state和props，也就是都沒用到而是直接傳給form這個component的props，刪掉。
   4. 如果是App.js用得到的action creator，就import一份回去。
10. flux和人生一樣，我們常常會了提高效率，先一團亂的把事情做出來再說，簡單的事情或許可以達成，但是如果要做成非常複雜的大事，就需要用很笨的方法，把東西分類排好，才不容易出錯，或是出錯的話才容易找出錯誤。

## 固定流程

1. 把view會用到的state或是props挑出來，丟到mapStateToProps。
2. 把view會用到的function挑出來，丟到mapDispatchToProps。
3. 把這些function丟到actions.js裡，並export出來，讓container components可以import給mapDispatchToProps。
4. 把其它不是view相關的function丟到actions裡，**不需要export出來**。
5. actions裡處理這些functions之間的關係，**所有的setState，都變成return actions或是dispatch actions**。
6. actions裡的function呼叫其它function可以用dispatch來呼叫，thunk會去執行。
7. reducers可以進行分類，同一類的state可以放一個reducers。
8. mapStateToProps要依reducer的分類來訂閱。
9. reducer裡的state有三種：
   1. 是給view用的，就要用mapStateToProps來訂閱。
   2. 不是給view用，但是要給action裡面的function用的，類似global variable，要用`getState()`來要，thunk會從第二個parameter傳進action creator裡(第一個parameter是dispatch)。例如form裡的資料，就是這樣丟給fetch相關的function，讓他們可以拿這資料去fetch後端。
   3. 純粹記錄app狀態，沒有要給誰用，當作console.log來用。
10. 檢查`reducer.js`的initial state只能有負責的state，不能有別的reducer的state在裡面。
11. 檢查action creator所dispatch或是return的actions的payload，裡面的state不能相同的key value pair丟給不同的reducers。
