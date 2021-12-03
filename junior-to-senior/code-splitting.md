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
    5. React的dynamic loading第二種作法：[component level](https://github.com/yellowful/code-splitting/tree/component-level)
        1. 不是在event handler裡面去決定要不要import component，而是在render的時候，才去async render component。
        2. 作法是寫一個higher order component (higher order function)：
           1. higher order component或是higher order function的定義有爭議，一種是function return function，另一種是function傳入function。
           2. 這個component嚴格來說其實是一個處理import的function，我們把處理import的這個function在這個higher order function的parameter傳進來。import就是去import別的component或是page。
           3. 這裡要注意的是：
               1. import是async的
               2. 我們不想在App.js裡，直接執行import，所以放在這個function裡的import要放在一個function裡面，這個function因為還在declaration的階段，所以可以被傳來傳去
        3. 在這個higher oder function裡面可以去建立一個class component或是functional component：
           1. 這裡面的class component其實和一般class component沒什麼兩樣
           2. 這個component有一個存放component的state，用一個async componentDidMount去把丟進這個component的component丟進state裡面，把不同步的import前面加上await。
           3. 我認為不一定要用async componentDidMount，這只是比較新的語法，直接用componentDidMount的話，讓`this.setState()`在.then()裡面去執行，也可以達到async componentDidMount的效果。
        4. 如果用functional component去hooks：
           1. 其中要async import的地方要用IIFE來處理，也就是在useEffect裡第一個參數的function裡面去定義一個async function，並且讓他立即執行。
           2. 我認為用.then()裡面來設定state也是行得通的。
           3. 最關鍵的地方，在於setComponentState的時候，因為這個state是一個component，其實也算是一個function，設定這種state需要改成用一個function來設定：<https://medium.com/swlh/how-to-store-a-function-with-the-usestate-hook-in-react-8a88dd4eede1>
        5. 另外一個易錯的地方是`<Component />`開頭要大寫，React才會視它為component而不是function。
        6. 然後這個higher order function去return這個建立在裡面的class或functional component，也就是這個higher order function其實像是一個high order component，high order component是指component return component或是component傳component進去。
        7. 被丟進higher order function的component其實是一個未執行的function，這個function return一個執行的async function，也就是import，所以這一個未執行的function被執行的時候也會是async的，所以在async的componentDidMount設定這裡面的component state的時候，要加上await的關鍵字。
        8. 當使用者click page2或是page 3要開始load page2或是page3的時候：
           1. 包在外面的async load function先被執行，然後這個function把未執行的import的function傳進去了我們的通用要回傳的async component。
           2. 這個function把這個async的component丟出來，然後這個async的component就被App給mount了，這時候這個async的component會先render初始值。
           3. 然後因為async的import在async的component的useEffect裡面，所以當import完成之後，這個async的component就會正確的載入page2或是page3了。
           4. App.js也可以正確的顯示page2或是page3了。
        9. 為什麼要這麼複雜？主要原因是import是async的，如果在App這裡直接import，因為async的關係，所以還沒載入就會render錯誤，所以把import的事情透過asyncLoadFunction丟給一個component，在那個component裡面就可以利用life cycle或是useEffect來處理async，然後react或是hook就會自己處理async完成後rerender的事了。
        10. 這樣的寫法有個好處，要lazy load的component丟進去這個async的higher order function的parameter，就可以lazy load了，code較clean。
        11. 這是有trade off的，比起第一個方法，第一個bundle的檔案(main)較大，因為那個bundle的檔案會多import一個async的function和裡面的component。
    6. 兩種code splitting的方法：
        1. Route level：react router
        2. Component level：react loadable(已經沒有維護了，不要用了)
        3. React官方已經有新的code split的方法，就是用lazy和suspense來做，可以參考ZTM的react課程。
        4. [andrei的練習](https://github.com/aneagoie/code-splitting-exercise)
    7. 解答：
        1. 用React的新功能`React.lazy`：<https://reactjs.org/docs/code-splitting.html#reactlazy>
        2. 如果是ssr用`@loadable/component`：<https://loadable-components.com/docs/loadable-vs-react-lazy/>
