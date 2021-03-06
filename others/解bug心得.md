# 解bug心得

1. 用`console.log()`：看看程式跑的是不是照心理所想的，常常會程式跑的和心理想的不一樣，這個簡單的方法就能解80%的問題。
2. 限縮問題：正確用`console.log()`的方式，是將自己懷疑有bug的地方，確認沒有bug而排除，bug會落在無法排除的幾個地方。
3. google或stack overflow：
   1. 要小心不是什麼都google得到，通常不能太general，雖然要specific，又不能太specific，這樣的問題才找得到答案。
   2. 找不到答案或是找到沒用的答案的問題，例如：
      1. react app為什麼不會跑？問題就太general了。
      2. 怎麼在gatsby裡用tachyons讓東西置中？問題就太specific了，直接找tachyons置中就好了。
      3. 要用關鍵字而不要用句子搜尋，才不會出現不必要的限制，漏掉好解答。
4. 用過上面的方法還是找不到bug，或是找到bug而無法解決問題，這時對問題的了解已經有一定程度的深入了，可以去幾個地方提問看看：
   1. stack overflow
   2. github discussion
   3. 相關discord社群
   4. vue或是ant design可以去csdn問
5. 通常比較大的機會bug是自己寫出來的，但如果bug不是自己寫出來的，而是你用的工具或是frame work有bug的話，怎麼抓出來呢？
   1. 用這個工具或是frame work做出一個最小的project，看看是不是能讓你認為bug的地方重現？如果可以，因為變因很少了了，bug很有可能是他們造成的，很有可能不是你造成的。
   2. 發現如果是工具或是frame work的bug，解法可以去github的issue找看看是不是有人發生過，有人發生過的機率很大。
   3. 沒人發生過的機率很小，除非你用的工具或是frame work的版本非常新，這時候可以自己開github的discussion或issue詢問。
6. 找bug的心態：不是相同方法相同地方一直重複的試，也不是找bug的毅力不夠，而是還有不同的方法還沒試過。
