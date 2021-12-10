# React Conf 2021



## React 18 Keynote

1. <https://www.youtube.com/watch?v=8dUpL8SCO1w>
2. react是for design而不是for programing，讓programer和designer說同樣的語言，目的是great UX。
3. suspense
   1. 舊版`.lazy()`只支援client side render
   2. 新版支援server side render
   3. spinner應該放在jsx裡比較合理，不用管data從來哪來決定哪種spinner
   4. RC = rlease candidate
4. server components
   1. 直接讀server side的資料
   2. hydrogen (shopify推動的) 和next.js有使用
5. react native
   1. concurrent rendering手機可以多工
   2. web <=> native
   3. React 18 可以在 React Native 上用，因為Native原本是multi thread，被改寫成single thread但可以concurrency，這就和React 18相同了。
6. react 18
   1. automatic batching：不會每次都re-render了
   2. server rendering：
      1. 以前all or nothing
      2. 現在在比較慢的component可以用suspense包起來
   3. `useID()`：和accessiblility有關
   4. 示範用project jira_clone當範例升級到React 18，root的語法有改。
   5. 示範改善效能：
      1. `React.memo()`
      2. `useDefferValue`
   6. streaming rendering
      1. csr：js太大包會有白畫面
      2. ssr：對內容很多的網站特別適合，因為可以先看到html，但是一樣無法馬上interactive
      3. ssr的缺點是，需對整個application render。
      4. 新方法不需要全部一起load，不需要全部一起hydrate。
      5. hydrating不會阻止使用者互動了。
      6. 使用者點擊時，react會先暫停hydrate，互動完才繼續hydrating。
   7. working group
      1. experts, developers, library maintainers, educators
      2. 目前80人
      3. useOpaqueIdentifier => useId
7. tooling
   1. 新tool：timeline
   2. useDeferredValue()
8. memo
   1. React Forget
   2. auto-memoizing compiler，不用再再寫useMemo。
   3. 還沒開發出來。
9. [docs](https://beta.reactjs.org/)
   1. 互動
   2. hooks開始
   3. diagram
   4. recipes有很常用的程式碼
   5. ebird api
   6. astro也是static site generator
10. design
    1. figma的component和variant就是被react啟發的
    2. component的concept是一樣的
    3. playroom
    4. <unicornAcademy.xyz>
11. playground
    1. mdx
       1. docusaurus
       2. nextra
       3. next/mdx
       4. remark
       5. rehype
       6. remix
12. [Relay](https://relay.dev/)
    1. 用來處理GraphQL的frame work
    2. release rust compile to open source，速度超快
13. desktop
    1. window和mac都可以用
    2. ms很多軟體用native建的
    3. xbox的介面也是native建的
14. machine learning
    1. on-device ML models for React Native
    2. PyTorch Mobile + React Native => react-native-pytorch-core
    3. 示範手機辨識物品
15. external store libraries
    1. Libraries: Zustand, Jotai, Valtio, React Tracked
    2. internal state: useState, useReducer
    3. tearing：external state造成view不一致
    4. 解決方式useSyncExternalStore
    5. demo startTransition 造成inconsistancy
    6. demo useSyncExternalStore，這是一個library的功能
16. accessibility
    1. comobox library
       1. 少很多code
    2. japanese accessibility
       1. 人資表格複雜，而且只有日文
       2. SmartHR提供web解方
       3. 連盲人都可以用
       4. SmartHR UI library可以用
       5. pdf還是很難做accessibility
17. UI tool
    1. Netflix Studio UI
    2. Netflix Animation UI
    3. github project: lyle/react-conf-2021
18. Hydragen by Shopify
    1. server side? client side?
    2. 可以和server component相容
    3. index.js bundle可以超小
    4. open source