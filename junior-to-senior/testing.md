# Testing

1. libraries：
   1. 我們是可以自己寫js確認丟進來的function功能正常，就丟出success，但是目前有更好更方便的工具和library可以協助我們。
   2. testing的幾種功能：
      1. testing frame work 或是 testing engine(scalfolding、library、method、structure)：
         1. jasmine
         2. jest
         3. mocha
      2. assertion library：讓你可以測試variable是你期望的值
         1. jasmine
         2. jest：`expect()`、`.toBE()`、`.toEqual()`、`toHave()`之類的，參考[cheat sheet](https://github.com/sapegin/jest-cheat-sheet)。
         3. chai：
            1. 通常和mocha配合，變成assertion library
            2. 例如：`expect({a:1}).to.not.have.property('b')`
            3. 上面左邊小括號的條件和右邊小括號的條件做比較，所以是true。
            4. 語法看起來很像英文。
      3. test runner：讓我們可以run test
         1. jasmine
         2. jest：裡面顯示一堆passing
         3. mocha
         4. karma.js：
            1. 可以在browser裡面run test
            2. 其它的
               1. 都不是在browser裡面run，因為要call browser的api會比較慢
               2. 有的在puppeteer(headless browser)環境底下跑，非常快。實際上puppeteer還是可以使用很多browser的功能，例如截圖、模擬鍵盤輸入之類的。
               3. 有的在jsdom裡面跑
                  1. 不是真的瀏灠器的DOM，但是是在js裡面的dom tree跑，就可以不需要browser了。
                  2. Jest的`react-scripts test --env=jsdom`就是在jsdom裡面跑
                  3. 最快、最簡單
      4. mock, spies, and stub
         1. 工具：
            1. jasmine
            2. jest
            3. sinon.js：
               1. 用來和mocha配合的
               2. 例如利用stub的功能，可以不用向server發出request，我們就可以假裝有發出request而得到response或是throws error，然後加以測試。
         2. spies：提供functions的資訊，例如function被呼叫幾次，在哪裡被誰呼叫。
         3. stubbing：用來把一個function置換成另一個function，以確保相同的行為會發生。
         4. mock：假冒一個function或行為，來測試一個程式不同的地方，非常適合在integration tests。例如mock一個fetch function。
      5. code coverage
         1. 可以給我們一個報告，哪些code有做test了，做了的比例為何
         2. istanbul
         3. jest：底層也是istanbul
   3. 其它工具：
      1. Jasmine以前很流行
      2. 現在Jest比較流行：
         1. 前端常用
         2. jest是facebook弄的
         3. jest每一種tests都可以做，比較方便
         4. create-react-app已裝好jest
         5. snapshot testing是針對react的test
      3. mocha：
         1. 後端常用
         2. 比較彈性，可以用其它的extension
         3. 比較難設定
      4. chai：後端常用
      5. 其它library：
         1. ava：可以併行處理，速度非常快
         2. tape：低階、輕量化
         3. enzyme：
            1. 專為react app進行test的工具
            2. air bnb建的project
            3. 很受歡迎，快成為react生態的標準了
2. environment：
   1. 永遠在development的環境，在production的環境下，永遠不會有test的code。
   2. jest會在背景從scalfolding一直run到code coverage，從read the test => run the test => result => 顯示code coverage
   3. 可以選擇在DOM、Puppeteer、jsdom中執行。

## 測試的種類

1. unit tests
   1. 概念：
      1. 測試functions或classes
      2. 最便宜，最常見，andrei認為90%的時間都是寫unit tests
      3. 我倒是同意[kent的看法](https://kentcdodds.com/blog/static-vs-unit-vs-integration-vs-e2e-tests)，只有被隔開的獨立部份，才寫unit test。
   2. pure function：
      1. 最適合unit test
      2. 因為可以用functional programing，很方便
      3. stateless component就是pure function，給props就顯示views
   3. 不要測試contract，contract例如：
      1. server和database之間
      2. function和function之間
2. integration tests
   1. 概念：
      1. 用來測試各個程式碼之間是否正常運作
      2. 例如：
         1. express和database之間
         2. function和function之間
      3. 難度和成本
         1. 介於unit tests和automation tests之間
         2. 測試很脆弱而且難讀
         3. 太多要測試可以寫了，很難100%完成
         4. 很少看到有公司有很大量的integration tests，除非是超大公司
         5. 最好先寫unit test，然後才開始加入integration test
      4. 可能會利用spies或是mock的方式來確認side effect如我們所預測，而不是只有帶值進去測試。
      5. 也可能用stubs去模擬和修改某個部份程序來測式特別部位，例如模擬呼叫database，得到一個假的使用者資料來測試。
      6. browser或是相關環境可能協助提供window object來測試。
3. automation tests
   1. 概念：
      1. 又叫UI test, end to end test。
      2. 用來測試、模擬browser真實場景，也就是從end user的角度來測試。
         1. 因為有各種browser、各種螢幕大小，人工測試太花時間了，所以才有automation tests
         2. 工具：以下以外，還很多
            1. nightwatch
            2. web drive io：documentation最好
            3. test cafe：只想快速測試，不太在意不同browser的情況
            4. nightmare：很簡單的模擬使用者行為，或是進行網頁抓取
            5. Cypress：很常用，很有名
            6. Puppeteer：很常用，很有名
      3. 介由控制browser，來確定在瀏覽器上的行為正如你所預期
      4. 最難實現，因為有太多情況，太多edge cases需要測試，只有大公司有錢的公司才有辦法花錢在上面。
      5. 很多公司是測試人員測試。
      6. 和另外兩個test的比較：
         1. automation test通常會獨立於integration tests和unit tests，因為automation test通常要花比較多時間。
         2. automation test通常久久才做一次，例如一整天做一次，或是一星期做一次，或是merge前才做一次。另外兩種測試可以很常做。
      7. 以畫面截圖進行像素比對的測試：visual regression testing

## Jest

1. 安裝：
   1. `npm install --save-dev jest`
   2. 不需要react也可以用
   3. package.json的scripts要改一下：

      ```js
      "scripts": {
      "test": "jest",
      "jest": "jest --watch *.js"
      },
      ```

2. 檔名有test或是spec的js檔，他就會去看有沒有test的code
3. 單元測試：
   1. `it('this is a test', ()=>{待測的程式碼})`，把要測的東西，放在第二個parameter的function裡面，就是一個test的code了。
   2. `test('this is a test', function)`和上面是一模一樣的。
4. expect：`expect`和`expect()`不一樣
   1. expect是個function也是個object
   2. expect是個object的時候
      1. expect有.any()的method，parameter裡面可以放型別，可以回傳型別是否相同。
      2. expect()當function回傳的object沒有.any()的method。
   3. expect()是function的時候
      1. 這個function的parameter是放你要測試的variable或function進去，可以稱為一個assertion
      2. 會回傳一個object
      3. 這個object有`.toBe()`或是`.toEqual()`的method，這兩個method是expect所沒有的。
5. 相同或相等：
   1. `.toBe()`或是`.toEqual()`的共同點：
      1. 放期望的值
      2. 都可以判斷primitive type的值是否相等。
   2. `.toBe()`或是`.toEqual()`的相異點：
      1. .toBe()：
         1. 「相同」的意思
         2. 等同`===`
      2. .toEqual()：
         1. 「相等」的意思
         2. 等同deep equality
         3. 通常用在判斷objective type裡面的所有key和value相等。
            1. 因為objective type的值相等不代表他們相同，所以.toBe()通常無法通過檢查。
            2. array也是objective type。
6. `describe('群組名稱',()=>{ it(要做的測式1);it(要做的測式2) })`：
   1. 用在群組，可以把幾個tests集成一群
   2. 用法就是第一個parameter是名稱，第二個parameter是一個function，這個function裡再去執行各個小test。
7. async：
   1. [cheat sheet](https://github.com/sapegin/jest-cheat-sheet)
   2. 原來es6以後：props和value同名，可以只寫一個就好了，其實就是destructure。
   3. expect.assertions(number)：
      1. 做async的test一定要做這個，放在Promise外面。
      2. 意思是以下的expect會有number個值，不然會錯誤
      3. expect.assertions(1)，如果等一下expect(variable)裡面沒有值，那就會出錯。
      4. expect.assertions(1)，如果等一下expect(variable)裡面出現一個值，那也會出錯。
   4. 解決async的2個方法：
      1. `done`：
         1. 一個可以處理async的function
         2. 從要test的async function的parameter傳進去。
         3. 他會叫`expect.assertions()`等done()，done()執行了之後才確認test的結果，所以我們要把done()放在async裡面，例如.then()裡面，就可以成功處理。
      2. return：直接把promise 在要test的function中return，jest自動就會等promise跑完才確定test的結果。
8. mocks and spies：
   1. 目的：每次測試fetch很花時間，可以用mocks來簡省時間。
   2. `jest.fn().mockReturnValue()`：
      1. `jest.fn()`就會產生一個假的function的功能，我們可以用來模擬fetch
      2. 在測試的function裡declare和call
      3. `.mockReturnValue()`的parameter裡面放`Promise.resolve()`，裡面的parameter裡面放一個json資料，這種資料長得很特殊：

         ```js
         {
            json: () => Promise.resolve({
               total: 5,
               result: [1, 2, 3, 4, 5]
            })
         }
         ```

      4. 另一個更簡單的用法是用`.mockResolvedValue()`，parameter就不需要放`Promise.resolve()`，可以直接放json。
   3. `.mock.calls.length`：
      1. 指mock被call的次數
      2. 因為mock是用來模擬fetch的，我們是在測式丟fetch進來的這個待測的function，不是在測fetch，所以這個次數指的是，傳進來function後，這個fetch被call的次數是不是我們期待的。
      3. 用法：把mock的fetch丟進去待測function之後，`.then()`裡面可以去測式被呼叫的次數。
      4. [例子](https://github.com/yellowful/jest-tesing/blob/main/async-app.test.js)
   4. `.toBeCalledWith()`：
      1. parameter裡面放api的網址
      2. 因為jest.fn()原本是模擬fetch，而原本的fetch的parameter是URL，所以這裡面的parameter就要是URL，如此才能測式mockFetch是不是正確的mock fetch。
      3. 這裡有一個疑問，為什麼`.toBeCalledWith()`會知道哪個URL才正確？
      4. 以上疑問的發生是不了解這是在測試fetch還是測試傳進fetch的function，事實上fetch並不需要測試，我們在測的是原程式碼有用到fetch的這個function，而且以正確的方式call他，所以我們已經知道URL該是什麼，所以把它寫在`.toBeCalledWith()`裡面，這樣可以保證隨著開發繼續進行的時候，那個我們自己寫的function裡面執行fetch裡面放的URL不會被改錯。
      5. 當然以上還是會有trade off，這樣你URL需要改的時候，就要改兩個地方，一個是fetch裡的URL，一個是測試裡的URL，比較麻煩。
   5. 可以測試抓回來的array數量正不正確，mock的時候，就給一個正確數量的array就好了。
   6. React官網提到[mocking timer](https://zh-hant.reactjs.org/docs/testing-environments.html#mocking-timers)：可以模擬setTimeout來減少測試時間。
9. [實作project](https://github.com/yellowful/jest-tesing)：
    1. 安裝jest
    2. 安裝node-fetch：
       1. 原來node沒有fetch，所以要另外安裝node-fetch。
       2. jest預設不支援es module，要用es module要另外設定，如果不要另外設定node-fetch要安裝2.x版的。
       3. 3.0版的只支援es module，所以package.json要把`"type": "module",`設進去，否則會無法`import fetch from 'node-fetch';`
10. 心得：
    1. 測試的是function裡面的邏輯正不正確，所以給的資料和答案都是假設的。
    2. 寫測試像是寫規格書，測這些function的input和output符不符合規格，通常不是在對資料是不是正確。
11. redux的部份[實作](https://github.com/yellowful/enzyme-test-app)：
    1. 完整測試用redux用的套件：`redux-mock-store`
    2. 跟enzyme無關，是Jest的測試
    3. reducers：因為是pure function所以非常容易測試。
    4. constants：不用測試。
    5. actions：
       1. 牽涉到dispatch這個dependency，所以非常複雜，特別是async的部份。
       2. 需要import thunk。
       3. 需要一個模擬store的library：[redux-mock-store](https://github.com/reduxjs/redux-mock-store)
          - `import configureStore from 'redux-mock-store'`
          - `const mockStore = configureStore([thunkMiddleware])`：這個mockStore會是一個function，等一下可以用。
          - 測試sync時：
            - `const store = mockStore();`：parameter可以放初始值
            - `store.dispatch();`裡面可以放actions的function進去
            - 然後用`store.getActions();`來得到action，可以把它`console.log()`出來看看，會是個array，然後測試
            - expect**要放的是action**而不是function了
          - 測試async時：
            - App.js裡的dispatch和actions裡的function要改，App.js裡要dispatch一個function。**這不會造成app壞掉**，因為redux thunk只要收到dispatch過來的不是actions而是一個function，他就會等待，直到function被執行完actions出現。
            - actions.js裡的function要改，要return一個function，function裡面dispatch action出來，這樣得到的action就可以正確得到pending了。
            - 以上做法只能得到pending，還得不到正確的success，因為pending是立即可以得到，success需要Promise resolve之後才能得到。
            - 要得到success和failed的話：
              - `actions.js`裡面的async function**要return一個Promise**，這樣`store.dispatch()`的parameter收到一個Promise之後，自己才會變成一個Promise。
              - 然後因為`store.dispatch()`變成async了，jest對於async的處理就是**要放在return裡**。
              - 這時候`.getActions()`就可以完整得到一個actions的array，收到所有的dispatch了。
       4. [Andrei的解答不完整](https://github.com/aneagoie/robofriends-testing/blob/master/src/actions.test.js)
       5. [我的解答](https://github.com/yellowful/enzyme-test-app/blob/main/src/actions.test.js)
       6. async可以用的library：
          1. [nock](https://github.com/nock/nock)
          2. [supertest](https://www.npmjs.com/package/supertest)
          3. [msw](https://github.com/mswjs/msw)：用service worker來mock
       7. 用redux-saga在測試方面會比redux-thunk好寫。

## Enzyme

1. 新寫的程式最好不要用enzyme，而用React test library：
   1. 這是[這篇的觀點](https://dev.to/piotrstaniow/time-to-say-goodbye-enzyme-js-348c)
   2. 原因是官方只有一個人在維護，支援太慢了。
   3. 官方不支援hooks。
   4. 非官方的**對hooks的支援度還是會遇到一些麻煩的問題**，造成你的code需要用奇怪的方式來寫。
   5. 這個library仰賴react核心成員來支援，但react核心成員已經對enzyme沒興趣了。
   6. [移轉到React test library](https://testing-library.com/docs/react-testing-library/migrate-from-enzyme/)
2. 唸做：en zime
3. 用來測試react-16，安裝：
   1. `npm i --save-dev enzyme enzyme-adapter-react-16`
   2. 設定：新增一個檔案叫`setupTests.js`在src裡面

      ```js
      import { configure } from 'enzyme';
      import Adapter from 'enzyme-adapter-react-16';
      configure({ adapter: new Adapter() });
      ```

   3. jest v24以後會有和enzyme不相容的問題，要另外安裝`enzyme-to-json`
      1. `npm install --save-dev enzyme-to-json`
      2. 在`package.json`要加上：

         ```js
         "jest": {
               "snapshotSerializers": [
                  "enzyme-to-json/serializer"
               ]
            },
         ```

4. component裡：
   1. `import { shallow } from "enzyme";`：
      1. 大部分的時間都用shallow就夠了，不會用到render和mount。
      2. 用來render一個component，只會render component return或是render的component外表，不render裡面的children或children裡的element。
      3. 這就是unit test的意義，一次只測試一個unit
      4. `shallow().length`：指render出幾個component
      5. `shallow().props()`：是指render出來的children裡面的props，而不是傳進這個component裡的props。
      6. mock props：要模擬傳進這個component裡的props的話，直接shallow的時候指定就好了，例如：`shallow(<CounterButton color='red' size='big' />)`。
      7. render結果會是個DOM tree，可以放進expect的parameter。
   2. `import { mount } from "enzyme";`
      1. 又稱為full dom render
      2. 有用到DOM API的時候可以測試，例如`querySelectAll`時。
      3. 或是有用到life cycle的時候，例如要測試componentDidMount的時候。
      4. 適合想要在DOM API下測試時使用，需要在有DOM API的環境下才能測試，例如在browser(太耗時了)、headless browser、或是jsdom。
      5. 很少用mount，因為通常我們要keep test as clean as possible
   3. `import { render } from "enzyme";`
      1. 用來render react component，但是不是和static html的DOM API互動，反之RTL似乎就是直接和DOM API互動？
      2. 底層是用cheerio的library來做的
      3. 做法介於mount和shallow之間
      4. 不需要full DOM API，但是會render component裡的component
   4. snapshot
      1. 是jest的功能，原理主要是靠enzyme來render，用jest來進行snapshot。
      2. 理由：jest的一般測試，也可以做component的test，但是裡面很多element，code寫起來很repeat沒效率，所以才用snapshot的觀念來解決這個問題，讓你可以讓測試直接比對是不是和snapshot相同就好了。
      3. 用法是jest的`expect()`加上了`.toMatchSnapshot()`就會自動建立了snapshot，以後開發如果component有更動，造成和snapshot不同就會測試不過。
      4. 測試不過時，按`u`就會update snapshot，以現在的snapshot為準。
      5. `npm test -- --coverage .`：
         1. 可以看到.map()畫出來的component是不是有被cover到
         2. 可以看coverage report
         3. 小數點很容易被呼略，要小心
      6. mock props：如果被測試的component的props有被拿來做maps，沒有這個props的話會造成test的時候沒東西可以map，而發生錯誤，無法做snapshot，這時候就需要mock一個props傳進去來做測試，要記得，我們只是要測式component裡面的邏輯和做snapshot的時候相比有沒有被變動，而不是測試snapshot內容是不是和產品一模一樣。
      7. 必需和source control一起上傳github，因為別人下載你的code的時候，亂修改你的code的時候，他才知道他是不是改錯了。
   5. 原則：
      1. 寫test不應該很複雜
      2. 訣竅是寫app的時候注意程式的結構，讓這個結構很容易寫測式，也就是讓程式儘量是功能單純的pure function，如果app傾向寫了一個很複雜做很多事情的function，以後程式愈來愈多行的時候，會愈來愈難寫測試。
5. stateful components：
   1. 還是用shallow就足以測試
   2. `.find`：類似`.getElementsByTagName()`或`.getElementById()`
   3. `.simulate('click')`：可以摸擬`onClick`，其它事件例如：`'keypress'`、`'change'`。
   4. `.state()`：可以取得state值。
   5. `.props()`：可以測試props。
   6. `.setState()`：
      1. 可以設定component裡面的state。
      2. 可以配合snapshot，設定不同state之後記錄不同的snapshot。
6. container：
   1. 不含redux的簡單測試，redux部份請參考jest章節：
      1. 方法1：
         1. 我自己試不行，因為connect也會export一個App，而且需要放store的props進去。
         2. 加上export`export class App extends Component{}`，不要測試複雜的connect。
      2. 方法2：
         1. 把原來的`App.js`拆成兩個檔，一個是原來的`App.js`，另一個是新的`Main.js`
         2. 原來的`App.js`留下所有的redux相關的就好了，其它刪掉。
         3. 原來的`App.js`裡面redux以外的，移到`Main.js`。
         4. 這樣只需要測試`Main.js`就好了，redux的部份，超簡單，就不用測試了，因為redux官方已經幫我們測試過了。
         5. jest的`beforeEach()`：
            1. [適合的場景](https://medium.com/enjoy-life-enjoy-coding/unit-test-%E6%9B%BF%E6%B8%AC%E8%A9%A6%E8%A8%AD%E7%BD%AE%E5%88%86%E9%A1%9E-describe-%E5%8F%8A%E4%BD%9C%E7%94%A8%E5%9F%9F-scoping-2c5082266ca)
               1. 用`beforeEach()`是在每一次測試之前要執行的事情。
                  1. 例如測試之前需要login，就可以放在這裡，用RTL就是用fireEvent去觸發那些登入需要的event。
                  2. 另外，要注意的是在這裡面用const來宣告變數的時候，因為和`test()`或是`it()`在不同scope，所以無法使用，所以要在`test()`和`beforeEach()`外面用let來宣告，如此才能夠在`beforeEach()`去指定變數，給其它`test()`使用。
               2. 用`afterEach()`是在每一次測試之後要執行的事情。
               3. 可以裡面放`console.log()`就知道裡面發生什麼事了，[例如這邊](https://github.com/yellowful/enzyme-test-app/blob/main/src/components/Main.test.js)。
            2. 我自己使用過的場景是，把共同要做的事先放在裡面，例如：設定initial state先設定在裡面，這樣在shallow時，就可以用initial state了，但是某些test裡面可以再去改設定的state，[例如這邊](https://github.com/yellowful/enzyme-test-app/blob/main/src/components/ErrorBoundry.test.js)。
            3. 從react testing library的documentation裡面看到，afterEach()是用來清除每次shallow的或是RTL的render的component的，也就是`cleanup()`，而RTL號稱不用清，因為預設就會自動清楚，[React Testing Library Crash Course](https://www.youtube.com/watch?v=GLSSRtnNY0g&t=3228s)在1:15:58的地方有說明。
         6. `.instance()`可以讓class component instanciate，以利來呼叫`this.method()`或是`this.variable`。
         7. 技巧：
            1. mock props的時候，要傳進去當props的function可以用`jest.fn()`，告訴jest，那個props就只是一個functions，要functions回傳值的話可以用前面提過的`.mockReturnValue()`。
            2. 要測試container component裡面function的邏輯的話，可以利用設計mockProps傳進去container component，然後利用`.instance()`去呼叫component裡面的method，然後測試return的值。
   2. 心得：
      1. 要了解目前在測試的什麼？要測試container裡面的某個function的邏輯？或是某些特定props下的snapshot？或是某些props下，是不是某些function會被呼叫？或是某些props下，某些function被某些props呼叫？
      2. 看到要測試的component，先想到有什麼是可以測的。
      3. 依測試目的可能需要改原來程式碼，例如：
         1. [要測試的邏輯是某個function的邏輯](https://github.com/yellowful/enzyme-test-app/blob/580f93139cdada5f8b26097cea4f832c4eaf3a9f/src/components/Main.test.js)：那這個程式碼是pure的話就很簡單，放parameter進去，就可以測試return是不是正確。
         2. [要測試的邏輯是props當input和filter出來的結果的話](https://github.com/yellowful/enzyme-test-app/blob/main/src/components/Main.test.js)：就要把這個filter改成不是pure的，這樣才能直接測得這兩者之間直接的關係。
      4. 但也要適可而止，否則成本非常高。
   3. 問題：snapshot無法記錄children，因為children是要從props.children傳進來，要如何模擬呢？
      1. 目前想到的方法是用RTL的render，render完的component會有一個container的attribute，這個attribute有.firstChild的attritube，就會是children。
      2. 然後可以配合Jest的`.toMatchInlineSnapshot()`自己手動加上snapshot，[可參考範例](https://testing-library.com/docs/react-testing-library/api/#render)。

## React-testing-library

1. 官方文件：
   1. 其實他是叫Testing Library，React只是他可以測的其中一個功能，它還能測Vue和Angular和Svelte…等等。
   2. 作者是知名React生態圈網紅[Kent C. Dodds](https://kentcdodds.com/)。
   3. 比enzyme更直覺，例如：不用shallow，也不用指定給變數，直接執行render，就可以在expect裡取用screen了。
   4. [query](https://testing-library.com/docs/queries/about/)：
      1. RTL是用`getByRole`來找element，可以順便測試accessiblility。
      2. `screen`可以當成`document.body`來用。
      3. [可以在這裡試玩](https://testing-playground.com/)
   5. 直接模擬event：`userEvent.click(screen.getByText('Check'))`
   6. RTL不用`.instance()`或是`.state()`，而是直接用`fireEvent`因為RTL的測試精神是，不測試使用者不了解的功能，測試應該要測使用者觸發的功能。
   7. 要用jest的snapshot功能的話：
      1. 要用[asFragment的api](https://testing-library.com/docs/react-testing-library/api/#asfragment)，不過要注意，snapshot有它的缺點，有時候改不重要的東西的時候，也無法通過測試，理論上應該是要測試重要邏輯就好了。[可以參考這篇的觀點](https://typeofnan.dev/how-to-test-your-react-app-effectively-with-react-testing-library/)。
      2. jest還有一個`.toMatchDiffSnapshot()`可以用來儲兩個不同狀態下，相異的snapshot。
      3. 官方例子好像比較多是用`react-test-renderer`套件來進行快照。
2. [教學影片](https://www.youtube.com/watch?v=GLSSRtnNY0g)：
   1. 為什麼要寫程試自動test，而不用手動測試？
      1. 因為寫測試主要是要去避免寫了新功能造成舊功能壞掉。
      2. 寫測試只有一開始寫的時候比較耗時間，但是之後都變自動測試了，所以之後新增功能的測試都比手動測試節省時間。
      3. 和其它人合作較大的app時，比較不怕去弄壞自己不理解的功能，而且弄壞時，testing會跟你說哪裡有問題。
      4. 寫的好的測試，可以直接看測試碼知道程試在幹麼，而取代documentation。
   2. [這個練習](https://github.com/yellowful/test-driven-development)
      1. RTL：內建於CRA，不用安裝。
      2. 接下來這個練習會用Test Driven Development：理由是比較不會忘記測試
      3. 從testing-library 引入 [render](https://testing-library.com/docs/react-testing-library/api#render)
         1. `import {render} from '@testing-library/react'`
         2. render可以放component裡去，會得到一個render完的component。
         3. 每一個test要有一個render，不能只有在test外面render。
      4. 這個component有很多method，可以直接destructure出這些method，例如：`getByTestId`。
      5. import expect
      6. jsx裡面可以設一個property叫[data-testid](https://testing-library.com/docs/queries/bytestid/)來協助測試。
      7. 用`.getByTestId('input')`之類的method取得element之後要先放到一個變數裡，這個變數就會有一些attribute可以用來測試：
         1. `.value`
         2. `.textContent`
         3. `.className`
      8. 在`40:24`的地方已經用TDD做出只有用initial state 所render出來的components。
      9. TDD的思考方式在於寫程式時，先寫出一堆無法pass test的紅字測試，然後開始寫feature的時候，再讓紅字漸漸減少，最後變成0。
      10. `fireEvent`：`42:00`
          1. 其中一個常用的method`.change()`
             1. 有兩個parameter，第一個是所選的element，第二個是一個object。
             2. 第二個object是模仿event的結構，例如我們要的值一般是e.target.value，所以這個object就是長這樣：

               ```js
               {
                  target:{
                     value:'5'
                  }
               }
               ```

          2. 按+這個按鈕的測試`48:39`
          3. `.click()`一個parameter就夠了。
   3. 心得：
      1. 的確比較方便，不用管state，對使用者來說的確state不重要，只需要fireEvent，接著驗證DOM結果就好了，比較接近真人驗證。
      2. 教學的`.getByTestId()`作法並不被官方文件建議，官方不建議用id去query，因為你原本的app應該沒有data-testid，最建議的作法還是`.getByRole`。
3. redux：
   1. [用在Redux的教學影片](https://www.youtube.com/watch?v=vbvQzWDCuXU)：
      1. 這是2019的影片，現在作法可能不一樣了。
      2. 直接測試整個container component。
      3. 直接在`.test.js`裡用store和provider去提供render做出整個container component。
      4. 其它測試就和一般測式差不多。
   2. 另一個[用在Redux的教學影片](https://www.youtube.com/watch?v=8Qfz5EFG9gQ)：`3:18`：render不用指定在一個變數中。
   3. 其實直接用jest作redux測試就好了，請參考jest章節。
4. [比較新的RTL影片](https://www.youtube.com/watch?v=Vp_76zdHkV8)：`18:08`
   1. 如果一個component有state改變，要放在`act()`裡面，然後裡面要有fireEvent
   2. [Testing Library官方文件](https://testing-library.com/docs/react-testing-library/api#act)說act作用是用來傳遞引數給act function？
   3. [React 官方文件](https://zh-hant.reactjs.org/docs/testing-recipes.html#act)說明的最清楚，把狀態改變放在act裡面，這樣可以確保接下來的assertion是在狀態改變完了了，才進行assertion。
   4. 我覺得這一段影片把assertion放在act裡面有點怪，我覺得應該放在外面，因為他把assertion放裡面了，所以他必需要加了一堆async和await才能讓它正常運作。
5. [另一個比較新也比較完整的RTL系列影片](https://www.youtube.com/watch?v=7dTTFW7yACQ&list=PL4cUxeGkcC9gm4_-5UsNmLqMosM-dzuvQ)
   1. [structure of tests](https://youtu.be/SppbtlpPZu4?t=432)：
      1. 通常是引入render和screen來用。
      2. 通常在test block會做4件事情：
         1. render要測試的component
         2. 找出要互動的elements
         3. 和這些elements互動
         4. assert來後進行測試
   2. [Intro to Query Methods](https://youtu.be/Yghw9FkNGsc?t=231)：
      1. `3:51`有各種query的比較。
      2. `queryBy`或是`queryAllBy`不符合不會報錯，而是報null或是array，其它和`getBy`或是`getAllBy`是一樣的。
   3. [Priority](https://youtu.be/PLL5Pvuk-tw?t=67)：
      1. `1:07`顯示query的使用順序：accessible => semantic queries => test id
      2. accessible：
         1. [getByRole](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques#roles)
         2. getByLabelText
         3. getByPlaceholderText
         4. getByText
      3. semantic queries
         1. getByAltText (image的說明)
         2. getByTitle (說明)
   4. 實作[Using Query Methods](https://www.youtube.com/watch?v=l9qr3EuLE_8&list=PL4cUxeGkcC9gm4_-5UsNmLqMosM-dzuvQ&index=7)，常用的unit test會用的assertion：
      1. `.toBeInTheDocument()`
      2. `8:33`：
         1. `.getByRole("heading")`造成兩個符合的話，會出現錯誤。
         2. [解決方式](https://youtu.be/l9qr3EuLE_8?t=513)是加上特定條件，例如：

            ```js
            {name:"My Header"}
            ```

      3. `14:00`提到測試[沒有某個element的語法](https://youtu.be/l9qr3EuLE_8?t=840)
   5. [實作assertions](https://www.youtube.com/watch?v=3ugQRXRToFA)：
      1. `4:11`：
         1. 找某個element找不到，因為這個element被Link包住了。
         2. 原因我們測試的時候是測試一個隔絕的component，他沒有被最外面react router的BrowserRouter的component包住，無法處理Link把這個DOM包住的情況。
         3. 解決辦法是在test裡用一個function把component傳進去，然後用react router的BrowserRouter把他包住，return出來，再放到render裡。
      2. `10:13`：
         1. jest的`.toBeVisible()`用來測試這個DOM是不是看得見。
         2. 例如某個DOM的CSS的opacity被設成0了，那這個DOM就不會被看見，就會被報錯。
      3. `.toContainHTML('p')`：代表這個DOM裡面有p tag。
      4. `.not`：可以放在其它的method之前，這樣就會得到相反的結果。
   6. [實作Describe Block](https://www.youtube.com/watch?v=kVzw_f7TfCE)：原來可以巢狀結構，describe裡面還有describe。
   7. [Fire Events](https://www.youtube.com/watch?v=0Y11K7KSC80)：
      1. 主要是在做todo list有關[input和送出的部份](https://github.com/harblaith7/React-Testing-Library-Net-Ninja/blob/main/src/components/AddInput/AddInput.js)，注意todo和todos不一樣。
      2. `setTodos`這個props竟然只需要傳一個`jest.fn()`就可以正常運作？蠻神奇的，莫非RTL不是真的傳`jest.fn()`進去測試？是傳setTodos進去？應該這個測試只是在測input的change能正常顯示，並沒有要測送出後，這個input的value要傳給todolist裡，因為如果要測這個value是不是要傳給todolist裡，就會變成integration test，而不是unit test。
   8. [integration test](https://www.youtube.com/watch?v=6wbnwsKrnYU)：
      1. 要測試`<AddInput />`和`<TodoList />`之間的互動，就要測試他們共同的爸爸`<Todo />`。
      2. 要注意，某個小孫子component有用到Link，造成出錯，所以`<Todo />`要再用一個BrowserRouter包住了。
      3. 抓出要互動的DOM之後，用`fireEvent`去觸發。
      4. 先用`.not.toHaveClass()`測試event發生前，用`.toHaveClass()``測試event發生後 .
   9. [async](https://www.youtube.com/watch?v=V2wWLM8VX5k)：要用`.queryByText()`，配合jest的`.not`來測試，用`.query`而不是`.get`，就是不讓發生找不到element的時候報錯。
      1. 只需要在`it()`的第二個參數加上async和要get或find的DOM之前加上await就可以了。
      2. 如果沒有placeholder、textContent，會有點難選，這時候才用`data-testid`。
      3. 可以用Jest的`.toBeInTheDocument();`來測試
      4. 在await的情況下，用`.getByTestId()`會抓不到，要用`.findByTestId()`
      5. 用`.getAllByTestId()`parameter裡面放regex來過濾關鍵字就好，可以找出所有的要的DOM，這可以用來處理map出來的DOM，可以得到一個array，供我們算出數目。
   10. [mocking requests](https://www.youtube.com/watch?v=TBZy-Rc-xX0)：
       1. 原因是reqeuest api：
          1. 要花錢
          2. 花時間
          3. 要依賴外部，而我們只想測前端，並不想測後端或外部API是否正常。
       2. `5:09`要開一個資料夾叫作`__mocks__`，[參考這篇文章](https://titangene.github.io/article/jest-manual-mocks.html)：
          1. `__mocks__`必需要放在要mock的js檔旁，而且檔名要和要mock的檔名一樣。
          2. 因為要mock的是axios套件，它是個node module，所以要在module旁。
          3. 如果mock的是node核心功能如fs和path，要mock的時候要呼叫`jest.mock('fs')`，一般模組不用。
          4. `fetch`不是node的模組，是瀏覽器才有的功能，要mock的話要用一個套件[Jest Fetch Mock](https://www.npmjs.com/package/jest-fetch-mock)，[這篇文章](https://www.leighhalliday.com/mock-fetch-jest)有介紹。
       3. 特殊技巧：`9:00`
          1. 去module中的`react-scripts/scripts/utils/createJestConfig.js`去改設定
          2. 把`resetMocks:true`改成`resetMocks:false`
          3. 不過有人留言分享正確作法，是在package.json裡面增加jest的設定：

              ```js
              "jest": {
                 "collectCoverageFrom": [
                    "src/**/*.{js,jsx,ts,tsx}"
                 ],
                 "resetMocks": false
              }
              ```

       4. `screen.debug()`等於是`console.log`的功能。
6. RTL不適合做unit test，如果要用RTL來做unit test的話，可以善用mocks：
   1. [Component Unit Testing](https://www.youtube.com/watch?v=XDkSaCgR8g4)

## 總結

1. [關於testing的一個特別觀點](https://medium.com/@eugenkiss/lean-testing-or-why-unit-tests-are-worse-than-you-think-b6500139a009)：
   1. [Write tests. Not too many](https://www.youtube.com/watch?v=Fha2bVoC8SE)
      1. kent在工具或是library上code coverage可能可以達到100%，因為那很容易，但是在app或是產品上，並沒有什麼code coverage的數字可以參考。
         1. 例如新創新產品，code coverage就會較低。
         2. 成熟的小產品，code coverage就會較高。
      2. 如果測試的東西，你的使用者並不會用到，那就是測試implement details。
      3. implement details會造成implement困難，難以重構。
      4. 測試種類：
         1. static code analysis:
            1. lint
            2. @flow
            3. type script
         2. business logic：
            1. unit test: function logic
            2. integration test: api, server
            3. e2e test: cypress超讚
         3. 從static => unit => integration => e2e
            1. 成本愈來愈高
            2. 愈來愈慢
            3. 問題難度愈來愈難
            4. 問題愈來愈重要
            5. 所以integration是最好的balance
            6. shallow render snapshot沒什麼用
   2. 只有隨便瞄一下，大概是說unit test無法cover到很多的code，並不划算，所以有一篇文章是寫為什麼[大部份的unit testing 是浪廢](https://rbcs-us.com/documents/Why-Most-Unit-Testing-is-Waste.pdf)。
   3. lean testing的投資比較划算。
   4. lean testing是指一種integration tests，而且只做重要部份的tests，這部份使用者比較會用到。
   5. 我自己的心得：
      1. 或許寫test最大的功用，是讓自己由不同的角度看自己的code，可以更清楚了解和確認自己的code是在幹麼，確認是不是自己想的那樣。
      2. 另外可以避免自己或是其它人改code的時候改壞了。
      3. 但是這是有成本的，變成改code的時候，可能要連測試一起改。
      4. app的unit test小心不要寫太多，常用和重要的function要做，其它的snapshot可能不是很重要。
2. Andrei的總結：
   1. 寫tests不是為了獲得100%的code coverage。
   2. test讓你從另一個角度來看自己寫的code，有時候為了讓test能夠執行，需要重新改自己原來的code，所以有人在寫code的時候就同時寫test。
   3. 寫test在很多人合作很大的project上，能夠避免一些bugs的產生。
3. [PJ好文](https://ithelp.ithome.com.tw/articles/10280261)
4. [不要mock fetch](https://kentcdodds.com/blog/stop-mocking-fetch)：
   1. mock fetch仍然不保證fetch沒問題。
   2. 比較慢的feedback loop
   3. 很多重複的code
   4. 解決方式：
      1. [kent在paypal作法](https://kentcdodds.com/blog/stop-mocking-fetch)
      2. [msw](https://github.com/mswjs/msw)：
         1. service worker來模擬後端
         2. 不是測試時可以用而已，開發時也可以來模擬後端
         3. 甚致chrome的開發工具network頁看起來就像連往後端一樣
         4. 這種方式可以減少在各個unit test中重複的mocking fetch，也不用在test的時候特別去處理request和response的property
         5. 這種方式可以檢查到client端送出request的時候的property是否錯誤。
         6. [範例](https://github.com/mswjs/examples/tree/master/examples/rest-react)
         7. [教學文](https://tw.alphacamp.co/blog/learn-api-and-mock-service-worker)
   5. 好處：遠離了細節，所以你可以作重大重構，而且你有信心這樣的重構不會弄壞使用者體驗，而這就是test的目的。

## celerec筆記

1. 用integration tests先把主邏輯先確定好。
2. async的部份用msw來mock才方便又準確。
   1. 照官方的doc的star一步一步做很快。
   2. 因為test是用node環境，node底下的handlers必需用完整網址。
3. redux可以用一個wrapper包起來用，每個測試都要包一次。
4. event：
   1. fireEvent
      1. 功能比較多
      2. 速度比較快
      3. [種類](https://github.com/testing-library/dom-testing-library/blob/main/src/event-map.js)
   2. userEvent
      1. 比較逼真，例如：click就會包含mouseEnter，還有那些bubble。
      2. 功能較少
5. `within()`：縮小query的範圍。
6. unit test測試functions就好了。
7. 要注意async的測試：
   1. 外面要加async
   2. 裡面screen.findxxx前面要加await
8. `ByRole`：善用chrome工具裡面的accessibility，可以看到role, name, tile...等等。