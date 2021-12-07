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
   useEffect：
      function component載入的時候會render，更新的時候也會render，而useEffect如果沒有設定第二個arguments的時候，一旦state更新，本身會rerender，所以useEffect裡面的function也會又render更新state，然後就變成無限迴圈了。
      第二個arguments
            如果第二個arguments有的話，第二個arguments有變動才會跑第一個arguments裡的function。
            設成[]，永遠都不會變動，所以只有component載入時會跑第一次。
Lecture 346: React Hooks 8
   證明了第二個parameter變動的時候，第一個parameter的function會run



[進階觀念](https://kentcdodds.com/blog/write-fewer-longer-tests)
   To learn the more advanced React hooks and different patterns to enable great developer APIs for custom hooks.
   Use useReducer to manage state and avoid stale state bugs (and learn when it's preferable over useState)
   Optimize expensive operations with useMemo and useCallback
   Interact with third party DOM libraries with useLayoutEffect
   Learn when to use (and when not to use) useImperativeHandle and useDebugValue
   Create custom hooks for complex use cases
[什麼時候用useReducer](https://kentcdodds.com/blog/should-i-usestate-or-usereducer)

1. 某些情況適合用來取代useState。
2. 裡面舉例一個需要undo，redo功能的app，因為一個state會影響到另一個state的顯示，用useReducer會簡潔很多，把state的邏輯都移到了reducer裡了。
3. app就剩很單純的view和listener的部份。

## useEffect

