# vsCode

1. extension：
   1. cmd+shift+p：或點左邊下面，可以安裝extension
   2. quokka：可以自動即時顯示JavaScript的語法錯誤
   3. night owl：布景主題好看
   4. indentica：可以顯現階層
   5. Babel JavaScript：完整保留字highlight
   6. Oceanic Next：最適合Babel的布景主題，是sublime的顏色。
   7. <https://scotch.io/bar-talk/22-best-visual-studio-code-extensions-for-web-development>
   8. Git Graph：可以看到git分支。
2. 左邊倒數第二個，可以直接debug JavaScript
3. debug視窗上面，可以直接開一個command line的視窗
4. 要找librarary裡面的檔案：
   1. 去你想找的地方，右鍵複製path。
   2. 去搜尋的地方，點V的符號。
   3. 再點...的符號。
   4. 把path貼上files to include，然後在search的地方打上你要搜尋的東西。
5. 快速鍵：
   1. mac通用：
      1. cmd+shift+箭頭：選取箭頭方向之後
      2. shift+箭頭：選取游標移動過的地方
      3. ctr+k：砍掉下一行，mac是砍掉這一段
      4. cmd+g：尋找下一個
   2. cmd+k 接著 cmd+ d：移動到下一個相同字的地方
   3. shift+cmd+\：跳到另一半括號上
   4. cmd+d：選取一個variable，第二次下一個相同字的地方加上游標
   5. shift+ctr+cmd+箭頭：可以擴張選取範圍
   6. shift+option+f：自動格式化
   7. com+k接著cmd+f：自動格式化選擇的部份
   8. cmd+x(不要選取)：剪下這一行
   9. shift+cmd+K：刪除這一行
   10. cmd+c(不要選取)：複製這一行
   11. option+箭頭：可以移動一整行
   12. option+shift+箭頭：可以複製一整行到箭頭方向那一行
   13. ctr+j：合併下一行到同一列
   14. Option + 滑鼠：多游標
   15. CMD+SHIFT+L：可以在選取的所有相同的字上加上游標
   16. Command + Option + 箭頭：向上或向下插入游標
   17. shift+option+滑鼠：可以一次插入多行游標，進行推進或退回一格
   18. Command + Shift + \：跳轉到對應的括號
   19. Ctrl + PgUp：滾動
   20. Command+g：搜尋下一個相同的字
   21. Command+d：選取下一個相同的字
   22. ctr+tab：切換tab
   23. cmd+K,cmd+s：叫出快速鍵說明
6. es7 extension的+tab相關縮寫：
   1. 最常用的：
      1. rce：react class component和export
      2. rafce：react function component和export
      3. rconst：constructor(props) with this.state
      4. clg：console.log(object)
      5. clo：console.log("object",object)
      6. nfn：const functionName = (params) => { }
      7. ren：render() { return( ) }
      8. sst：this.setState({ })
   2. 其他：
      1. !：產生html的format。
      2. imp：import moduleName from 'module'
      3. imd：import { destructuredModule } from 'module'
      4. exp：export default moduleName
      5. exd：export { destructuredModule } from 'module'
      6. enf：export const functionName = (params) => { }
      7. dob：const {propName} = objectToDescruct
      8. cp：const { } = this.props
      9. cs：const { } = this.state
      10. imr：import React from 'react'
      11. imrd：import ReactDOM from 'react-dom'
      12. imrc：import React, { Component } from 'react'
      13. imrs：import React, { useState } from 'react'
      14. redux：import { connect } from 'react-redux'
      15. rconst：constructor(props) with this.state
      16. est：this.state = { }
7. markdown：
   1. extention：
      1. markdown all in one：
         1. 可以用很多快捷鍵：例如option+c，就產生check的效果
         2. 可以自動產生toc-table of content
      2. markdownlint：
         1. 可以檢查markdown語法
         2. 可以設定哪些語法不檢查：
            1. `markdownlint.json`
            2. 可以找到設定某一段不檢查某項語法：`cmd+shift+p`
      3. markdown preview enhanced：
         1. 可以預覽
         2. 快速鍵：cmd+shift+v
      4. markdown PDF：轉markdown變成pdf
      5. mermaid：可以畫流程圖，不過有的markdown render系統不支援。
   2. indent：3個空白
   3. 要表格好處理，可能要另外灌extention，讓excel可以貼上。
8. 把某檔案的頁面維持開著，不會隨著點別的檔案而關掉：把檔案拉去上面的tab處，再點別的檔案就不會被關掉了。
9. 設定：
   1. Accept Suggestion On Commit Character：設成false的話，可以設定`.`不要自動選字。
   2. Accept Suggestion On Enter：設成false的話，可以設定按enter的時候，不要自動選字。
   3. [完整移除vsCode](https://stackoverflow.com/questions/42603103/how-to-completely-uninstall-vscode-on-mac)
