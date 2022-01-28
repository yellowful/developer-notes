# Hooks

## ZTM web developer

Lecture 339: React Hooks
   說明作為一個developer，無可避免遇到工具演化，要如何面對呢？
Lecture 340: React Hooks 2
   第一要了解動機：
      1. 類似的component難以重新利用，所以才會寫wrapper component，來解決這個問題。但是一堆wrapper仍然會讓程式碼很難讓人了解。
      2. class component太複雜了，還有一堆life cycle，hooks的方式比較讓人容易了解。
      3. class不好用，hooks只需要function，讓做出來的component多了很多的可能性，可能和angular、可能和其他東西共用。
Lecture 342: React Hooks 4
   看著document自己改成hooks
Lecture 343: React Hooks 5
   把constructor刪掉
   把render刪掉
   最後帶出如何處理this.state？
Lecture 344: React Hooks 6
   hooks裡的array是javascript的新功能，是一種destructure，讓我們可以替state和setstate命我們喜歡的名字。
Lecture 345: React Hooks 7
   本身state的更新，會讓自己本身render，小心有效能的issue。
   效能部份參考[筆記react performance](https://github.com/yellowful/developer-notes/blob/main/junior-to-senior/performance.md#react-performance)
   useEffect：
      function component載入的時候會render，更新的時候也會render，而useEffect如果沒有設定第二個arguments的時候，一旦state更新，本身會rerender，所以useEffect裡面的function也會又render更新state，然後就變成無限迴圈了。
      第二個arguments
            如果第二個arguments有的話，第二個arguments有變動才會跑第一個arguments裡的function。
            設成[]，永遠都不會變動，所以只有component載入時會跑第一次。
Lecture 346: React Hooks 8
   證明了第二個parameter變動的時候，第一個parameter的function會run

## react和hooks / class component和stateful function component 的不同

1. 語法：
   1. react：
      1. 優點：class語法機乎就是javascript的class語法，可以同時練到javascript，而且這種用法在其它語言也用得到，但是有以下缺點。
      2. 比較麻煩，要注意綁定this。
      3. 將抽象的state邏輯抽出成component增加可重用性，可能產生wrapper hell，一層包一層，很多抽象層，不易找出bug。
      4. 同一個生命週期裡可能包了很多不相關的邏輯。
   2. hooks：
      1. 缺點：hooks獨有的語法，其它地方用不到，但有以下優點。
      2. 語法比較簡潔，不用注意this的問題。
      3. 用custom hooks抽出抽象的state邏輯，在不強迫tree加入更多component的情況下，讓其它component可以重用，可以很容易看出component的結構。
      4. 不同的邏輯可以用不同的useEffect來處理，較好讀。
2. setState：
   1. react：
      1. 部份state更新方便，是auto merge的方式，例如更新count2的state用`this.setState({ count2: this.state.count2 + 1 });`，count1的state就不會被動到。
      2. 即使要設定的值相同，只要this.setState被呼叫，就會觸發render，例如：state原本`{this.state.count1:0}`，執行`this.setState({count1:0})`，仍然會觸發render，對效能不利影響。
      3. `setState()`不能直接當成props往下傳，需包在eventHandler往下傳。
      4. setState不能用在render裡，不然會造成無限迴圈。
   2. hooks：
      1. 沒有auto merge的功能，是用replace的方式更新state，假設原本state是`{count1:0,count2:0}`，如果用`setState({ count2: count2 + 1 });`會造成count1消失，所以要用`setState({ ...state,count2: count2 + 1 });`。
      2. setState裡面的值如果和現有的state相同，就不會觸發render，會不會render是比較這次render時的state和上次render時的state是否相同。但要注意，如果state如果是referenctial type，仍會觸發render，因為reference會不同。
      3. `setState()`可以直接當成props往下傳，不需要包在eventHandler往下傳。
      4. `setState()`很容易造成無限迴圈，要用在event handler或是其它hooks裡面，例如useEffect裡。
3. 效能：
   1. 誰優誰劣很難說，react主要是class的原理，hooks主要是closure的原理。
   2. react的缺點：在轉譯成es5的時候，class instance比closure佔較大的記憶體，而且需要在constructor綁定event handler，所以這部份效能會較差。
   3. hooks的缺點：每次hooks render時，裡面的function都會被重新instanciate，會佔新的記憶體allocation，所以效能較差。
   4. 官網說現代的瀏覽器下，除非在極端情況下，closure和class的效能差異不大。
   5. 知乎上有人實測，大部份情形下似乎都看不出效能差異。

[進階觀念](https://kentcdodds.com/blog/write-fewer-longer-tests)
   To learn the more advanced React hooks and different patterns to enable great developer APIs for custom hooks.
   Use useReducer to manage state and avoid stale state bugs (and learn when it's preferable over useState)
   Optimize expensive operations with useMemo and useCallback
   Interact with third party DOM libraries with useLayoutEffect
   Learn when to use (and when not to use) useImperativeHandle and useDebugValue
   Create custom hooks for complex use cases

## useEffect

## useReducer

1. [什麼時候用useReducer](https://kentcdodds.com/blog/should-i-usestate-or-usereducer)
   1. 某些情況適合用來取代useState。
   2. 裡面舉例一個需要undo，redo功能的app，因為一個state會影響到另一個state的顯示，用useReducer會簡潔很多，把state的邏輯都移到了reducer裡了。
   3. app就剩很單純的view和listener的部份。
2. [state是object的時候](https://linyencheng.github.io/2020/02/02/react-component-class-based-vs-functional/)：
   1. 一般的useState會有各種不同的state要處理，通常state就是一個value或是array，或少少的object，改變狀態時，就是全部一起改變。
   2. 如果有一大堆不同的useState和邏輯要處理，想要像redux或是class component的setState一樣合併成一個大object來記錄狀態，每次改變卻只有要改變幾個state，這時候就可以用useState。

## useContext

1. useContext是用來解決composition無法解決的問題。
2. composition：
   1. composition的技巧就是，把component當成props傳到第18代子孫的技巧。
   2. 不直接用props傳遞到子孫，而是把component當成props傳到子孫的好處，是**如果props很多**要傳這麼多代會難以追蹤，所以在祖父這邊就把一堆props和一個component綁好，就只會佔用一個props，往下傳很好追蹤。
3. useContext：
   1. composition仍需要傳遞，所以適合傳給某個需要props的特定的子孫。
   2. useContext則可以用在一大堆component都會需要props的情況(locale、暗黑佈景)。

## custom hooks

1. [連結](https://zh-hant.reactjs.org/docs/hooks-custom.html)
2. 用法：
   1. 把共用的state邏輯抽出變成一個外部function，寫法和一般function相同。
   2. 這個custom hooks裡面會用到其它hooks，所用到其它的hooks依然需要遵守要放在最上層，不能放在判斷式裡。
   3. custom hooks被不同的component呼叫時，他們的state是相互獨立的，原理是即使在同一個component裡呼叫多次useState，每一個state之前也都是獨立的。
   4. 需要用use做為function的開頭，Hooks才會依照規則檢查是不是符合hook規則，正確的管理state。
3. [提示：在 Hook 之間傳遞資訊](https://zh-hant.reactjs.org/docs/hooks-custom.html#tip-pass-information-between-hooks)
   1. 意思是可以把一個state傳入另一個custom hooks裡。
   2. 例如：

      ```jsx
      const [recipientID, setRecipientID] = useState(1);
      const isRecipientOnline = useFriendStatus(recipientID);
      ```

4. 