# complete react developer

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
   2. 不是react的api，名稱可以自己取，但是用到某些life cycle就是error boundry，因為用到這些life cycle，所以要用class component。
   3. 這個life cycle是static getDerivedStateFromError和componentDidCatch
      1. static getDerivedStateFromError才能在children出錯之前，就先知道children是不是出錯了。
      2. 用static getDerivedStateFromError來處理出錯state，這個class component在state是error時丟出要顯示的component，在state不是error時丟出children。
      3. componentDidCatch通常只是用來記錄錯誤。
      4. 難怪之前用error boundry都沒有用，因為沒有用static getDerivedStateFromError，只用componentDidCatch，錯誤已發生，app就crash了，來不及顯示componentDidCatch要顯示的錯誤。
3. shouldComponentUpdate：
   1. shouldComponentUpdate用在class component，這個class component會放在別的地方，很可能每次都被render，加了shouldComponentUpdate可以決定這個class component要不要在那個地方被render，通常最常見的就是比較前一個state和這個state相不相同，不相同才就return true，這個component才會被render。
   2. shouldComponentUpdate有兩個parameter，第一個是爸爸傳下來的props，第二個是這個component的state，這都是還沒render的時候。props比states早，要render時，就是props變成states來render。所以通常這裡面就是比較即將要render的prop和原本的state相互比較，如果沒有不同就不用render。其實很常見的是只比較props或states裡面某一個特別重要的屬性而已。
   3. functional component用的就是React.memo(functional component)。
      1. memo就是memoization，類似快取起來。
      2. 一個要點要記得，React.memo是會花時間的，所以如果不是很大很花時間的component，如果裝上memo，反而會增加第一次loading的時間。
      3. 要使用的話，要用react dev tool去測，有比較快才用。
      4. 有一點要注意的是，如果傳給function的props是直接指定一個object，javascript會覺得是一個新的props，所以memo仍然會更新，但是用state傳進去props裡，就不會被認為是新的。
   4. 如果在class component裡面要.memo的用法，就要用extends React.PureComponent
4. useCallback：284堂
   1. javascript的`new Set()`只會把有新的東西放進array裡，這裡會用它來判斷是不是有新的object或是array(也就是新的reference)加進這個變數裡，如果只是object或是array裡的值變動，就不會有變化。
   2. 用Set測出某一個不需要新的function被一直initiate的時候，卻一直被initiate，這是因為這個functional component一更新就會被render，這時候就可以把這個function放入useCallback裡面。
   3. useCallback第一個參數放不要被重覆initiate的function，第二個參數和useEffect一樣，放dependency。
   4. 如果useCallback的第二個參數放空array，就是每次第一個參數的function都是被memoized，reference就不會變，所以用Set來測試有沒有新object的變數就不會不斷變大。
   5. 如果第二個參數有放某個state，就只有那個state改變時，這個function才會被initiate。
   6. 第二個參數放空的array的時候，useCallback第一次會看這個function還沒initiate，所以會把他做第一次initiate，如果是event handler，則是第一次被按的時候會被initiate，接著再按的時候，因為第二個參數沒有任何state變化，所以第一個參數的function就不會再被initiate了。
   7. useMemo和useCallback很像：
      1. useCallback是memoize一個function的reference，而useMemo是memoize一個function的value。
      2. 只有第二個參數裡的state有變化的時候，第一個參數裡的function才會重新計算。