# complete react developer

## css in js

1. [BEM](https://cythilya.github.io/2018/05/22/bem/)像是一種CSS class命名的規範，以解決class name 衝突的問題：
   1. block：紀錄區塊，例如`menu`
   2. element：紀錄小孩，例如`menu__item`
   3. modifier：紀錄狀態，例如`menu__item--active`
2. style property：
   1. 設個object
   2. 缺點是有些selector不能用，例如：
      1. disable
      2. hover
      3. before / after
      4. child
3. 解決方式：styled-components
   1. 原理
      1. 其實和style property一樣，底下是將style透過js加到html上。
      2. 當一個styled components定義並使用的時候，他內部會產生一個特殊的style的script，然後內部再產生兩個unique的class name，其中一個代表要用在目標哪個component，另一個是style。
      3. 也因為他真的有產生style script，所以所有css的selector都有支援。
   2. 優點：
      1. 用js來處理css效能可能比較好。
      2. css可以和component綁在一起，更好處理，不會有style leaking的問題。
      3. 真的完全隔絕，不像BEM，仍可能衝突。
      4. component裡不會看到一堆class name了
      5. 用styled components作出來的components，可以像component一樣的重複使用這些style。
      6. 可以用props去操控css了，這是css檔難以做到的。
      7. 單獨一個component有單獨的style，不需要看他受哪個祖先影響。
      8. 支援sass語法
   3. trade off：多了一層complexity(不是time complexity或是space complexity)，更複雜，不簡潔。
   4. 用法：
      1. 對element來說，是把styled-components匯入成styled，然後用`styled.div`後面加上template string，內容是css，然後宣告成一個component。
      2. 對component來說，是用`styled(ComponentName)`後面接template string，然後宣告成一個component。
      3. 內建`{css}`
         1. 可以把重複的css style的string指定到一個變數，通常取名`XxxStyles`，重複放進其它styled component裡，如此以達到dry，而且可以改一個地方就改了所有重複的地方。
         2. 用法css加上template string後，指定給變數`xxXxxStyle`。
      4. 可以放到`component-name.styles.jsx`裡，然後export。
      5. 在jsx裡，想要用我們做好的styled component，但是想改成別的element或是component的話，只要把這個element或是component的名稱當成`as`的props傳進去，就好了，如此又多一種重用css的方式了。
      6. props：第172堂
         1. 在template string裡，可以放`${function_name}`
         2. 傳進這個component裡的props會當成那個function的props傳進去，然後可以return style。
         3. 這個return的style必需是內建css設定成的變數。
         4. 這種方式可以用props來操弄css。

## react performance

1. 279節lazy：把import改成一個lazy的function，用來import要的component，這個component的code就可以被分開了。
   1. 要解決的問題：bundle出來的js程式碼太大，會造成first load太久，所以要把code split。
   2. 需要搭配suspense的component和fallback的attribute把這個用lazy引入的component包起來，因為這個component被lazy load，所以沒被包起來會被當成一般component，會載入出錯。
   3. 這個fallback的attribute裡面會是一個component。
   4. 缺點：
      1. 寫程式較麻煩
      2. 如果不是大到影響效能，用的話，效果不明顯。
      3. 在載入被split的component時需要時間，不像一開始就載入時，切換非常快。
2. error boundry：
   1. 要解決的問題：lazy load的時候，網路問題造成無法load下一個component，會把整個app搞壞，所以要用error boundry來預防這種事。
   2. 不是react的api，名稱可以自己取，但是只要是用到**某些life cycle**的話，就是error boundry，因為用到這些life cycle，所以要用class component。
   3. 這些life cycle是static getDerivedStateFromError和componentDidCatch
      1. static getDerivedStateFromError才能在children出錯之前，就先知道children是不是出錯了。
      2. 用static getDerivedStateFromError來處理出錯state，這個class component在state是error時丟出要顯示的component，在state不是error時丟出children。
      3. componentDidCatch通常只是用來記錄錯誤。
      4. 難怪之前用error boundry都沒有用，因為沒有用static getDerivedStateFromError，只用componentDidCatch，錯誤已發生，app就crash了，來不及顯示componentDidCatch要顯示的錯誤。
3. 減少render次數：
   1. class component：二種用法選一種
      1. shouldComponentUpdate
         1. 用在class component，這個class component會放在別的地方，很可能每次都被render，加了shouldComponentUpdate可以決定這個class component要不要在那個地方被render，通常最常見的就是比較前一個state和這個state相不相同，不相同才就return true，這個component才會被render。
         2. shouldComponentUpdate有兩個parameter，第一個是爸爸傳下來的props，第二個是這個component的state，這都是還沒render的時候。props比states早，要render時，就是props變成states來render。所以通常這裡面就是比較即將要render的prop和原本的state相互比較，如果沒有不同就不用render。其實很常見的是只比較props或states裡面某一個特別重要的屬性而已。
      2. `...extends React.Component()`改成`...extends React.PureComponent{}`，就會自動幫你處理shouldComponentUpdate裡面在做的事了，也就是說shouldComponentUpdate就是單純的props和state，沒有需要對props和state要特殊邏輯的判斷，就直接用`PureComponent`比較方便。
   2. functional component
      1. functional component要小心效能的問題，因為只要parent有render的情況下，即使在傳進來的props沒有任何改變的情況下，都會被rerender，react不會去檢查props是不是有變。
      2. 要避免functional component被不必要的render，用的就是export的時候用React.memo(functional component)。
      3. memo就是memoization，類似快取起來。
      4. 一個要點要記得，React.memo是會花時間的，所以如果不是很大很花時間的component，如果裝上memo，反而會增加第一次loading的時間。
      5. 要使用的話，要用react dev tool去測，有比較快才用。
      6. 有一點要注意的是，如果傳給function的props是直接指定一個object，javascript會覺得是一個新的props，所以memo仍然會更新，但是用state傳進去props裡，就不會被認為是新的。
   3. 如果在class component裡面要.memo()的用法，就是要用`extends React.PureComponent`。
4. useCallback：284堂
   1. 要解決的問題：hooks裡定義的function，每次rerender，這個function都會被重新initialize，假如這個function根本就沒有dependency，每次都是一樣不會改變，那沒有必要每次都被initiate，這久了會造成memory leak，這時就可以用useCallback。
   2. 證明每次都被initiate：
      1. javascript的`new Set()`只會把有新的東西放進array裡，這裡會用它來判斷是不是有新的object或是array(也就是新的reference)加進這個變數裡，如果只是object或是array裡的值變動，就不會有變化。
      2. function也是一個object，用Set測出某一個不需要新的function被一直initiate的時候，卻一直被initiate，這會一直增加新的reference佔住的記憶體，最後造成memory leak。
      3. 上一點是因為這個functional component的state一更新就會被re-render，re-render就又造成function被initiate，如果有大量的even handler時就真的會出問題了，解決方式是把這個function放入useCallback裡面。
      4. 這裡要注意的是這個`new Set()`要設在function外面，才不會每次re-render的時候Set又被`new`了一次。
      5. Set的用法：
         1. `New Set()`放到一個變數裡
         2. 用變數.add()放入值
         3. 再用console.log(變數)檢查
         4. 用來檢查function裡面的initiate次數的時候，應該要把這個`New Set()`設定放在function component外面，才不會自身也被initiate了。
   3. useCallback第一個參數放不要被重覆initiate的function，第二個參數和useEffect一樣，放dependency。
   4. 如果useCallback的第二個參數放空array的時候
      1. 就是每次第一個參數的function都是被memoized，reference就不會變，所以用Set來測試有沒有新object的變數就不會不斷變大。
      2. useCallback第一次會看這個function還沒initiate，所以會把他做第一次initiate，如果是event handler，則是第一次被按的時候會被initiate，接著再按的時候，因為第二個參數沒有任何state變化，所以第一個參數的function就不會再被initiate了。
   5. 如果第二個參數有放某個state，就只有那個state改變時，這個function才會被initiate。
   6. 使用的時機有二個：
      1. 這個component很多不同的handler都會更新這個conponent的state，而這個component也會一直被rerender，這些不同的handler之間並不相互依賴，這樣會造成一堆不需要被宣告的handler一直不必要的被宣告。
      2. [useCallback和useMemo](https://medium.com/ichef/%E4%BB%80%E9%BA%BC%E6%99%82%E5%80%99%E8%A9%B2%E4%BD%BF%E7%94%A8-usememo-%E8%B7%9F-usecallback-a3c1cd0eb520)：function要被多個地方使用，但是只有一個地方會因state或是props改變，這時候可以用useCallback，把這個function放裡面，就不容易造成memory leak。
      3. 如上篇文章提到的，如果function要被多個地方使用，但是跟state和props無關，這時候就把function宣告在component外面就好了，就不會每次rerender都被initiate了，可以不需要useCallback。
5. useMemo和useCallback很像：
   1. useCallback是memoize一個function的reference，而useMemo是memoize一個function的value(或是一個object的reference)，這個function通常不是event handler。
   2. 只有第二個參數裡的state有變化的時候，第一個參數裡的function才會重新計算。
   3. 我認為useMemo使用時機在於兩個條件成立：
      1. 有複雜計算的function。
      2. 這個複雜計算的function只有特定state或是dependency觸發時值才會變動，其它都不會變動。
6. 其它參考文章：
   1. [useCallback](https://ithelp.ithome.com.tw/articles/10225504)
      1. ZTM課程提到`const function`會造成每次render會重新佔一次memory，造成memory leak，事實上下次render時memory應該就會被回收了。
      2. 用useCallback根據dependency來決定這個function和下一次render的function是不是要重新佔一次memory，一種玩法是把這個useCallback回來的東西放入useEffect的dependency的array裡面來決定要不要跑useEffect第一個參數的東西，其實和useEffect的dependency放一個空array是一樣的效果，因為useCallback回來的東西是reference到同一個function，等於是沒有dependency，但是可以解決eslint發出的warning，這個warning是擔心useEffect裡的第一個參數其實是有dependency的，我想真正這種作法意義不大。
   2. [When to useMemo and useCallback](https://kentcdodds.com/blog/usememo-and-usecallback)：
      1. 舉了一個例子說明為什麼inline function用useCallback反而效能更差：
         原本：

         ```js
         const App = () => {
           const initialCandies = ["snickers", "skittles", "twix", "milky way"];
           const [candies, setCandies] = React.useState(initialCandies);
           // dispense在onClick裡的callBack裡面會被呼叫，而且這裡出現一個closure
           // dispense在這個callBack還沒被呼叫的時候已經被帶進去了，所以已經丟對應的candy進去了
           // allCandies就是prevStates，filter裡面，如果目前所有的candies裡面，不是對應這個按鈕的，會return true，意思是會把不是對應這個按鈕的其它按鈕過濾出來，然後更新state。
           // 這個dispense真的會run是在button被click的時候
           const dispense = (candy) => {
              setCandies((allCandies) => allCandies.filter((c) => c !== candy));
           };
           return (
              <div>
                 <h1>Candy Dispenser</h1>
                 <div>
                 <div>Available Candy</div>
                 {candies.length === 0 ? (
                    <button onClick={() => setCandies(initialCandies)}>refill</button>
                 ) : (
                    <ul>
                       {candies.map((candy) => (
                       <li key={candy}>
                          <button onClick={() => dispense(candy)}>grab</button> {candy}
                       </li>
                       ))}
                    </ul>
                 )}
                 </div>
              </div>
           );
         };

         export default App;
         ```

         改成下面效能會更差(佔更多allocation)：

         ```js
         const dispense = React.useCallback(candy => {
         setCandies(allCandies => allCandies.filter(c => c !== candy))
         }, [])
         ```

      2. 因為：
         1. 原始版當第二次render的時候，第一次的dispense會被當成garbage清掉，然後把第二次的dispense放到新的allocation裡。而useCallback版第二次render的時候，第一次的dispense反而不能被當成garbage清掉了。
         2. 更不用說React還要記得dependency來做比較。
      3. 以下useMemo的例子根本不划算：

         ```js
         // 原本
         //　const initialCandies = ['snickers', 'skittles', 'twix', 'milky way']
         // 改成
         const initialCandies = React.useMemo(
            () => ['snickers', 'skittles', 'twix', 'milky way'],
            [],
         )
         ```

      4. 如果和props或是state無關的話，可以移到functional component外面，反而是更好的做法。
      5. 以下是個例用Referential equality來處理useEffect的例子，讓dependency不是比較reference而是比較value：

         ```js
         function Foo({bar, baz}) {
            const options = {bar, baz}
            React.useEffect(() => {
               buzz(options)
            }, [options]) // we want this to re-run if bar or baz change
            return <div>foobar</div>
         }

         function Blub() {
         return <Foo bar="bar value" baz={3} />
         }
         ```

         改成：

         ```js
         function Foo({bar, baz}) {
            React.useEffect(() => {
               const options = {bar, baz}
               buzz(options)
            }, [bar, baz]) // we want this to re-run if bar or baz change
            return <div>foobar</div>
         }
         ```

      6. Kent認為使用useCallback的時機就是用在[Referential equality](https://kentcdodds.com/blog/usememo-and-usecallback)：
         1. 第一個例子是給useEffect、useLayoutEffect、useMemo、useCallback的dependencies用的。

            ```js

            function Foo({bar, baz}) {
               React.useEffect(() => {
                  const options = {bar, baz}
                  buzz(options)
               }, [bar, baz]) // we want this to re-run if bar or baz change
               return <div>foobar</div>
            }
            function Blub() {
               const bar = () => {}
               const baz = [1, 2, 3]
               return <Foo bar={bar} baz={baz} />
            }
            ```

            改成：

            ```js
            function Foo({bar, baz}) {
              React.useEffect(() => {
                 const options = {bar, baz}
                 buzz(options)
              }, [bar, baz])
              return <div>foobar</div>
            }

            function Blub() {
            const bar = React.useCallback(() => {}, [])
            const baz = React.useMemo(() => [1, 2, 3], [])
              return <Foo bar={bar} baz={baz} />
            }         
            ```

         2. 另一個例子是給`React.memo()`、`React.PureComponent`、`shouldComponentUpdate`用的：

            ```jsx
            const CountButton = React.memo(function CountButton({onClick, count}) {
              return <button onClick={onClick}>{count}</button>
            })

            function DualCounter() {
               const [count1, setCount1] = React.useState(0)
               const increment1 = React.useCallback(() => setCount1(c => c + 1), [])

               const [count2, setCount2] = React.useState(0)
               const increment2 = React.useCallback(() => setCount2(c => c + 1), [])

               return (
                  <>
                     <CountButton count={count1} onClick={increment1} />
                     <CountButton count={count2} onClick={increment2} />
                  </>
               )
            }
            ```

      7. useMemo還有用在記得大量計算的時候才划算，而且這個大量計算是和state或是props有關的時候，例如：

         ```jsx
         function RenderPrimes({iterations, multiplier}) {
           const primes = React.useMemo(
             () => calculatePrimes(iterations, multiplier),
             [iterations, multiplier],
           )
           return <div>Primes! {primes}</div>
         }
         ```

## React.memo()

1. 和`React.PureComponent`、`shouldComponentUpdate`功能是一樣的，和他們不一樣的地方只是這是用在function component。
2. 用了React.memo()的話，functional component的props利用相同的state當成props傳進去就不會被rerender。
3. 比較的方式是用[shallow compare](https://medium.com/@xyz030206/react-%E6%95%88%E8%83%BD%E5%84%AA%E5%8C%96-purecomponent-f971fb56f90a)：
   1. 淺層比較。
   2. 以下兩點同時符合，會認為相同：
      1. 值相同
      2. reference相同
   3. 依上一點來比較，當一個object淺層相同，深層不同的時候，會被判為相同，因為淺層的reference和key和value都會相同，深層則是reference相同，就不再比較深層的value了。
   4. 所以react的this.state要小心不要設定value是個object，最好把attribute獨立成一個state，這樣state相同相異的判斷才不會出現非預期的情況。
4. 用之前要用profile檢查效能，有必要才用：
   1. profile可以錄起來，可以看到render花費多少時間，錄製的過程被render了幾次。
   2. mount最花時間，rerender還好。
   3. 現在的component dev tool還能告訴你為什麼component被render，[新的feature](https://reactjs.org/blog/2019/08/15/new-react-devtools.html)。
   4. [profiler component](https://reactjs.org/docs/profiler.html#usage)：可以包住要測量的component，console.log()出結果。
5. 有一種情況，即使用了React.memo()，這個functional component的props不是利用相同的state當成props傳進去，而是直接放一個object當成props傳進去，也就是放所謂的inline object或是inline array的時候，因為每次被render時的object的reference都是新的，所以他們的小孩還是會被rerender，如果小孩re-render很貴的時候，小心這種inline的方式放object type的東西。
6. 實際使用的時機：
   1. 購物車選取商品時，早就被選的商品，沒有再被選時，就不用被rerender了，但是購物車一直被更新，所以購物車裡的商品可以用.memo()包起來，當props沒變時，就不會被rerender，這樣使用者每點一個商品，先前的商品都不會被rerender，所以效能就可以有效的被提升。
   2. 我想到log的item也可以被`.memo()`包起來，因為log的item的props都是相同的，所以只要props沒變，就不會被rerender。
   3. 感覺這種和效能有關的，在使用者點選螢幕就必需有所更新的component，比較有可能需要用到。

## state更新就立即執行

1. 在react裡面叫作setState的callback function，就是把setState的第二個參數放一個callback function，這樣state更新完會立即執行callback function裡面的事情。
2. [hooks](https://stackoverflow.com/questions/56247433/how-to-use-setstate-callback-on-react-hooks)：hooks的setState沒有第二個參數可以放，但是可以用useEffect達成相同的功能，也就是在useEffect的第一個參數裡放上一個callback function，把要做的事情放在裡面，第二個參數放對應需要判斷的state，這樣state一更新，callback function就會被執行。

## HOC

Higher Order Component：
   [例如PrivateRoute](https://github.com/yellowful/dqhome/blob/main/src/components/PrivateRoute/PrivateRoute.js)：

   1. 先用`<Router>`把`<PrivateRoute />`包起來，在`<PrivateRoute />`設path並傳一個component進去。
   2. 這時候的`<PrivateRoute />`就是一個HOC，我們傳一個component進去，然後這個component會render東西出來，這邊就是null或是傳進去的component。

## 其他

node.js沒有預設gzipping，所以後端server要灌套件[compresstion](https://www.npmjs.com/package/compression)

## serverless function and stripe

1. stripe原理：
   1. stripe提供一個scret key
   2. stripe不允許把secret key從前端向stripe提出payment request，所以需要從後端進行。
2. 後端：
   1. 提供和使用者互動以外的邏輯，例如：
      1. database interaction
      2. cross service interaction
      3. data management、service management
      4. devOps
      5. infrastructures
      6. scaling up resources
   2. 基本上就是一台電腦，有著記憶體和硬碟，所有的code都是在利用這兩樣資源。
   3. 沒有任何的使用者介面。
   4. 用來提供前端來要的資源。
   5. 效能：
      1. 大部份一台server 100 req/min是沒問題的。
      2. 10K req/min
         1. 需要scale up，一台server無法處理這麼多request
         2. 而這時候團隊就要思考，如何手動寫code才能scale up到幾千幾萬台server上了
         3. 淡季時就要scale down
         4. 非常花錢也非常花時間
3. serverless service：
   1. 後端功能單純時，例如只是要
      1. make payment
      2. api request
      3. send email
   2. 不用scale up和down，用多少就付多少錢。
   3. 這些function住在server的end point上，只有在接到request時才會被丟出來執行任務。
4. stripe設定：
   1. 不用activate acount也可以做為開發使用，也有test mode可以用。
   2. 關鍵在於有一個publishable key和secret key。
   3. 前端要灌兩個stripe的library：`yarn add @stripe/stripe-js @stripe/react-stripe-js`
   4. 主要是要利用stripe的CardElement，之前先import `Elements`，然後把`<App />`包住，讓stripe知道這個app的request是和某個帳號有關，這裡要傳publishable key進去。
   5. `.env`：
      1. 已內建於`create-react-app`，所以不用灌library了。
      2. 裡面的變數一定要是`REACT_APP_`的前綴。
      3. `.gitignore`要把`.env`過瀘掉。
      4. 在react app裡面，透過`process.env.REACT_APP_XXX`來取得值。
   6. 182堂課的`07:30`用publishable key去instanciate一個stripePromise，然後export給App.js用，最後當成Elements的props傳進去。
   7. import stripe的CardElement，原來CardElement就是填信用卡的component。
   8. 接著做一個paymentHandler來處理submit button。
   9. 然後要import `useStripe`和`useElements`。
   10. 