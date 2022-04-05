# April 2022

1. [不需要用js的地方](https://lexoral.com/blog/you-dont-need-js/)
   1. 動畫：改用svg動畫
      1. 效能更好
      2. 比較不會出錯
      3. 下載頻寬極小
      4. 更responsive
      5. 繪製工具：
         1. [inkscape](https://inkscape.org/)
         2. [svg path editor](https://yqnn.github.io/svg-path-editor/)
         3. [教學](https://developer.mozilla.org/en-US/docs/Web/SVG/Element)
   2. 可以用hover做的都用css來做：
      1. 例如側邊導覽menu
         1. 優點：
            1. 寫法比js更簡單
            2. 用`:focus-within`可以讓tab觸動，accessiblility方便。
         2. 作法關鍵點
            1. 沒hover時的位置是`right:100%`，並且`transform: translateX(1em);`露出一小截可以讓人hover或touch
            2. hover或touch之後用`transform: translateX(100%);`menu出現
            3. 可以加上`transition: 0.2s transform`有動畫效果
      2. 其它可以用hover的場合還有9種，這裡列幾種：
         1. hover預覽
         2. tooltips
         3. 預覽影片縮圖
      3. 善用`transition-delay`這個props
   3. 位置控制：
      1. 例如sticky效果用css來控制：
         1. `position: sticky`
         2. 和`position: fixed`很像，不同的地方在於，他是所在element出現之後之後才會出現在固定位置
         3. 優點在於，用js做的話，會有lag的效果
      2. 其它很多和css postion屬性相關的功能，儘量不要用js，而是用css來做，效果更好。
   4. 展開收合內容：
      1. 可以用html的`<details>`來做
      2. 展開的動畫可以用css的transiction設定，但是收合沒效果
      3. 收合要有動畫的效果的話要用[css的animation來做](https://stackoverflow.com/questions/38213329/how-to-add-css3-transition-with-html5-details-summary-tag-reveal/38215801#38215801)
   5. 黑暗模式：
      1. 在css裡可以用html的element的狀態來決定要顯示的style，例如：`#darkModeCheckbox:checked`，就是`id`是`darkModeCheckbox`，它的狀態是`checked`的時候，我要顯示的style。
      2. 要注意的是，browser可能不會記得這個`checked`的狀態，可能要用js和`localStorage`來記得state。
2. [a fundamental guide to react suspense](https://www.chakshunyu.com/blog/a-fundamental-guide-to-react-suspense/)：新的React 18的Suspense功能，讓你的component被Suspense的時候，js仍試圖把能render的children繼續render，不會因為parent被Suspense就block小孩的render。
3. [rerender的最佳化](https://alexsidorenko.com/blog/react-how-many-rerenders/)：
   1. 純jsx沒有計算的tree，render非常快，不會有render次數的問題。
   2. 不用寫code時就馬上要把rerender最佳化
   3. 因用chrome裡一個CPU工具，調成4X slowdown，很容易看出哪邊有render效能問題
   4. 用上述的cpu工具看出明顯有效能問題的話，才用以下工具去測量render次數：
      1. React Developer Tools Profiler
      2. Chrome DevTools Profiler
4. [socket.dev](https://socket.dev/)：用來檢查你用的open source libraries是不是安全。
5. [How to not mess up while using all the React Hooks](https://labs.factorialhr.com/posts/hooks-considered-harmful)：
   1. state：代表用一個變數把資料儲存一個預定的生命時間，讓我們可以預期在這個時間可以取用到他，而不受變動。
   2. class component對state的處理用的是class被instanciate之後的this所存的變數
   3. functional component對state的處理靠的是function的closure機制，儲存在closure裡的變數。
   4. 重要單字：
      1. implicit：隱含的
      2. implicit：寫明的
      3. corollary：可想而知、必然的結果
   5. closure不是deterministic