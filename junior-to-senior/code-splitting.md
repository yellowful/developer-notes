# code splitting

1. 改善performance兩大重點
    1. First meaningful paint
    2. Time to interactive
2. 實例：
    1. React的vender code意思是第一次load網頁時， 會有一些整個app都需要的code，例如第三方的library，這部分的code無法lazy load。
    2. Redux logger要設定build time不要有，因為build time不需要。
    3. 原生js已經支援lazy load了：
        1. [https://www.ksharifbd.com/blog/exploring-ES2020-dynamic-import/#Import](https://www.ksharifbd.com/blog/exploring-ES2020-dynamic-import/#Import)
        2. Script type要設為module，才可以用import的語法
        3. 可以event handler去執行dynamic的`import()`
        4. Import進來的module就是export default的東西，例如一個function。
    4. React的dynamic loading的第一種作法：[route level](https://github.com/yellowful/code-splitting/tree/route-level)
        1. `<this.state.component />`：jsx 竟然接受這樣的寫法，component這個state可以是一個component，這句可以弄出一個component
        2. React裡面的import基本上是webpack在處理的
        3. 配合上面的component state的設定，可以在event handler裡面藏一個判斷式，來決定要dynamic import哪一個component進來。
        4. [我練習的code](https://github.com/yellowful/code-splitting/blob/route-layer/src/App.js)
    5. React的dynamic loading第二種作法：[component level](https://github.com/yellowful/code-splitting/tree/component-level)
        1. 不是在event handler裡面去決定要不要import component，而是在render的時候，才去async render component。
        2. 作法是寫一個higher order function (higher order function)：
        3. higher order component或是higher order function的定義有爭議，一種是function return function，另一種是function傳入function。
        4. 這個component其實是一個function，我們把要載入的component在這個function的parameter import進來。
        5. 這裡要注意的是：
            1. import是async的
            2. 我們不想在App.js裡，直接執行import，所以放在這個function裡的import要放在一個function裡面
        6. 在這個higher oder function裡面可以去建立一個class component：
        7. 這裡面的class component其實和一般class component沒什麼兩樣
        8. 這個component有一個存放component的state，用一個async componentDidMount去把丟進這個component的component丟進state裡面，把不同步的import前面加上await。
        9. 我認為不一定要用async componentDidMount，這只是比較新的語法，直接用componentDidMount的話，讓`this.setState()`在.then()裡面去執行，也可以達到async componentDidMount的效果。
        10. 裡面也可以用functional component去hooks：
        11. 其中要async import的地方要用IIFE來處理，也就是在useEffect裡第一個參數的function裡面去定義一個async function，並且讓他立即執行。
        12. 我認為用.then()裡面來設定state也是行得通的。
        13. 最關鍵的地方，在於setComponentState的時候，因為這個state是一個component，其實也算是一個function，設定這種state需要改成用一個function來設定：<https://medium.com/swlh/how-to-store-a-function-with-the-usestate-hook-in-react-8a88dd4eede1>
        14. 另外一個易錯的地方是`<Component />`開頭要大寫，React才會視它為component而不是function。
        15. 然後這個higher order function去return這個建立在裡面的class component，也就是這個higher order function其實像是是一個high order component，high order component是指component return component或是component傳component進去。
        16. 被丟進higher order function的component其實是一個function return的一個async function，也就是import，，而且在async的componentDidMount設定這裡面的component state的時候，要加上await的關鍵字。
        17. 這樣就可以一旦要render這個async component的時候，可以先把render和higher order function的記憶體空間安排好，等要丟進來的component載入完才開始執行function，然後component才經由componentDidMount或是useEffect去async的import，然後function才丟出被載入的async component，最後才render載入的component。
        18. 這樣的寫法有個好處，要lazy load的component丟進去這個async的higher order function的parameter，就可以lazy load了，code較clean。
        19. 這是有trade off的，比起第一個方法，第一個bundle的檔案(main)較大，因為那個bundle的檔案會多import一個async的function和裡面的component。
    6. 兩種code splitting的方法：
        1. Route level：react router
        2. Component level：react loadable
        3. React官方已經有新的code split的方法，就是用lazy和suspense來做，可以參考ZTM的react課程。
        4. [andrei的練習](https://github.com/aneagoie/code-splitting-exercise)
    7. 解答：
        1. 用React的新功能`React.laz`y：<https://reactjs.org/docs/code-splitting.html#reactlazy>
        2. 如果是ssr用`@loadable/component`：<https://loadable-components.com/docs/loadable-vs-react-lazy/>
        3. react-loadable：已經沒有維護了，不要用了。