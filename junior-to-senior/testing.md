# Testing

1. library：
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
         1. jest是facebook弄的
         2. jest每一種tests都可以做，比較方便
         3. create-react-app已裝好jest
         4. snapshot testing是針對react的test
      3. mocha：
         1. 比較彈性，可以用其它的extension
         2. 比較難設定
      4. 其它library：
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

## unit tests

1. 概念：
   1. 測試functions或classes
   2. 最便宜，最常見，90%的時間都是寫unit tests
2. pure function：
   1. 最適合unit test
   2. 因為可以用functional programing，很方便
   3. stateless component就是pure function，給props就顯示views
3. 不要測試contract，contract例如：
   1. server和database之間
   2. function和function之間

## integration tests

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

## automation tests

1. 概念：
   1. 又叫UI test, end to end test。
   2. 用來測試、模擬browser真實場景，也就是從end user的角度來測試。
      1. 因為有各種browser、各種螢幕大小，人工測試太花時間了，所以才有automation tests
      2. 工具：以下以外，還很多
         1. nightwatch
         2. web drive io：documentation最好
         3. test cafe：只想快速測試，不太在意不同browser的情況
         4. nightmare：很簡單的模擬使用者行為，或是進行網頁抓取
         5. Cypress
   3. 介由控制browser，來確定在瀏覽器上的行為正如你所預期
   4. 最難實現，因為有太多情況，太多edge cases需要測試，只有大公司有錢的公司才有辦法花錢在上面。
   5. 很多公司是測試人員測試。
   6. 和另外兩個test的比較：
      1. automation test通常會獨立於integration tests和unit tests，因為automation test通常要花比較多時間。
      2. automation test通常久久才做一次，例如一整天做一次，或是一星期做一次，或是merge前才做一次。另外兩種測試可以很常做。

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
   1. `it('this is a test', function)`，就是一個test的code了。
   2. `test('this is a test', function)`和上面是一模一樣的。
4. expect：expect和expect()不一樣
   1. expect是個function也是個object
   2. expect是個object的時候
      1. expect有.any()的method，parameter裡面可以放型別，可以回傳型別是否相同。
      2. expect()當function回傳的object沒有.any()的method。
   3. expect()是function的時候
      1. 這個function的parameter是放你要測試的variable或function進去
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
         3. 通常用在判斷objective type裡面的所有key和value相等。因為objective type的值相等不代表他們相同，所以.toBe()通常無法通過檢查 .
6. `describe()`：
   1. 用在群組，可以把幾個tests集成一群
   2. 用法就是第一個parameter是名稱，第二個parameter是一個function，這個function裡再去執行各個小test。
7. async：
   1. [cheat sheet](https://github.com/sapegin/jest-cheat-sheet)
   2. 原來es6以後：props和value同名，可以只寫一個就好了，其實就是destructure。
   3. expect.assertions(number)：
      1. 做async的test一定要做這個。
      2. 意思是以下的expect會有number個值，不然會錯誤
      3. expect.assertions(1)，如果等一下expect(variable)裡面沒有值，那就會出錯。
      4. expect.assertions(1)，如果等一下expect(variable)裡面出現一個值，那也會出錯。
   4. 解決async的2個方法：
      1. `done`：
         1. 一個可以處理async的function
         2. 從要test的async function的parameter傳進去。
         3. 他會叫`expect.assertions()`等done()，done()執行了之後才確認test的結果。
      2. return：直接把promise 在要test的function中return，jest自動就會等promise跑完才確定test的結果。
8. mocks and spies：
   1. 目的：每次測試fetch很花時間，可以用mocks來簡省時間。
   2. `jest.fn().mockReturnValue()`：
      1. `jest.fn()`就會產生fetch的功能
      2. 在測試的function裡declare和call
      3. parameter裡面放`Promise.resolve()`，裡面的parameter裡面放一個json資料，這種資料長得很特殊：

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
      2. 因為mock是用來模擬fetch的，我們是在測式傳fetch進來的這個function，不是在測fetch，所以這個次數指的是，傳進來function後，這個fetch被call的次數是不是我們期待的。
   4. `.toBeCalledWith()`：
      1. parameter裡面放api的網址
      2. 因為jest.fn()原本是模擬fetch，而原本的fetch的parameter是URL，所以這裡面的parameter就要是URL，如此才能測式mockFetch是不是正確的mock fetch。
      3. 這裡有一個疑問，為什麼`.toBeCalledWith()`會知道哪個URL才正確？
      4. 以上疑問的發生是不了解這是在測試fetch還是測試傳進fetch的function，fetch並不需要測試，我們在測的是原程式碼有用到fetch的這個function，所以我們已經知道URL該是什麼，所以把它寫在`.toBeCalledWith()`裡面，這樣可以保證隨著開發繼續進行的時候，那個我們自己寫的function裡面執行fetch裡面放的URL不會被改錯。
      5. 當然以上還是會有trade off，這樣你URL需要改的時候，就要改兩個地方，一個是fetch裡的URL，一個是測試裡的URL，比較麻煩。
   5. 可以測試抓回來的array數量正不正確，mock的時候，就給一個正確數量的array就好了。
9. 總結：測試的是function裡面的邏輯正不正確，所以給的資料和答案都是假設的。
10. 實作project：
    1. 安裝jest
    2. 安裝node-fetch：
       1. 原來node沒有fetch，所以要另外安裝node-fetch。
       2. 3.0版的只支援es module，所以package.json要把`"type": "module",`設進去，否則會無法`import fetch from 'node-fetch';`
       3. jest預設不支援es module，要用es module要另外設定，如果不要另外設定node-fetch要安裝2.x版的。

## Enzyme

1. 新寫的程式最好不要用enzyme，而用React test library：
   1. 這是[這篇的觀點](https://dev.to/piotrstaniow/time-to-say-goodbye-enzyme-js-348c)
   2. 原因是官方只有一個人在維護，支援太慢了。
   3. 官方不支援hooks。
   4. 非官方的支援還是會遇到一些麻煩的問題，造成你的code需要用奇怪的方式來寫。
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

   3. jest v24以後會有問題，要另外安裝`enzyme-to-json`
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
      1. 大部分的時間都用shallow就夠了，對會用到render和mount。
      2. 用來render一個component，只會render component裡面的element，不render裡面其它的component
      3. 這就是unit test的義意，一次只測試一個unit
      4. `shallow().length`：render出幾個component
      5. `shallow().props()`：是指render出來的element裡面的props，而不是傳進這個component裡的props。
      6. mock props：要模擬傳進這個component裡的props的話，直接shallow的時候指定就好了，例如：`shallow(<CounterButton color='red' size='big' />)`。
      7. render結果會是個DOM tree，可以放進expect的parameter。
   2. `import { mount } from "enzyme";`
      1. 又稱為full dom render
      2. 有用到DOM API的時候可以測試，例如`querySelectAll`時。
      3. 或是有用到life cycle的時候，例如要測試componentDidMount的時候。
      4. 適合想要在DOM API下測試時使用，需要在有DOM API的環境下才能測試，例如在browser(太耗時了)、headless browser、或是jsdom。
      5. 很少用mount，因為通常我們要keep test as clean as possible
   3. `import { render } from "enzyme";`
      1. 用來render react component，但是不是和static html的DOM API互動。
      2. 底層是用cheerio的library來做的
      3. 做法介於mount和shallow之間
      4. 不需要full DOM API，但是會render component裡的component
   4. snapshot
      1. jest一般測試，也可以做component的test，但是裡面很多element，code寫起來很repeat沒效率，所以才用snapshot的觀念來解決這個問題，讓你可以讓測試直接比對是不是和snapshot相同就好了。
      2. 原理主要是靠enzyme來render，用jest來進行snapshot。
      3. 用法是jest的`expect()`加上了`.toMatchSnapshot()`就會自動建立了snapshot，以後開發如果component有更動，造成和snapshot不同就會測試不過。
      4. 測試不過時，按`u`就會update snapshot，以現在的snapshot為準。
      5. `npm test -- --coverage`：
         1. 可以看到.map()畫出來的component是不是有被cover到
         2. 可以看coverage report
      6. 如果被測試的component的props有被拿來做maps，沒有這個props的話會造成test的時候沒東西可以map，而發生錯誤，無法做snapshot，這時候就需要mock一個props傳進去來做測試，要記得，我們只是要測式component裡面的邏輯和做snapshot的時候相比有沒有被變動，而不是測試snapshot內容是不是和產品一模一樣。
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
   6. `.setState()`：可以設定component裡面的state。
6. container：
   1. 完整測試用redux用的套件：`redux-mock-store`
   2. 簡單測試：
      1. 方法1：加上export`export class App extends Component{}`，不要測試複雜的connect。
      2. 方法2：
         1. 把原來的`App.js`拆成兩個檔，一個是原來的`App.js`，另一個是新的`Main.js`
         2. 原來的`App.js`留下所有的redux相關的就好了，其它刪掉。
         3. 原來的`App.js`裡面redux以外的，移到`Main.js`。
         4. 這樣只需要測試`Main.js`就好了，redux的部份，超簡單，就不用測試了，因為redux官方已經幫我們測試過了。
         5. jest的`beforeEach()`：可以把mockProps放在裡面，這樣在shallow時，就可以用mockProps了。
         6. `.instance()`可以讓class component instanciate，以利來呼叫this.method或是this.variable。
         7. 技巧：
            1. mock props的時候，要傳進去當props的function可以用`jest.fn()`，告訴jest，那個props就只是一個functions。
            2. 要測試container component裡面function的邏輯的話，可以利用設計mockProps傳進去container component，然後利用`.instance()`去呼叫component裡面的method，然後測試return的值。