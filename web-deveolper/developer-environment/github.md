# github筆記

## 為什麼要用github？

1. 不用自己管理、存檔、備份各種不同版本
2. 直接管理整個資料夾，而不是單一檔案的版本

## 平常建立新專案時常用步驟

- 最常用的會是步驟4、5、6

1. 在github上開啟一個新的repository(專案)，把網站上的資料clone下來：
   1. 取repository的名字，例如homeworkgit
   2. 點initialize this repository with a README
   3. 步驟1的右邊有網下載網址可以複製。
   4. 在本地下資料夾下指令：
    `git clone https://github.com/yellowful/homeworkgit.git`  
    (網址請替換成自己複製的網址。)
   5. 這種git clone的方式，在本機上就不用打git init，會自動追蹤版本。
   6. 後面可以加上自訂的資料夾名稱。
2. 要寫的程式、筆記、檔案存在這個資料夾中
3. 把更動的歷史儲存，只有一個句點代表這個資料夾的所有檔。
    `git add .`  
4. 在本機建立一個版本，並為版本寫下一行的說明  
    `git commit -m '一行說明'`  
    注意要有單引號
5. 若沒有下參數-m，則會進入多行的說明模式，這是vim的環境。  
    `i進入編輯模式`  
    按esc鍵會進入命令模式  
    命令模式下:wq，就是存檔離開
6. 上傳指令：  
   `git push`
7. 要更新github上的版本，就重覆步驟4、5、6，要看現在的更動狀態的話：  
   `git status`
8. 如果不是用git clone的方式開新repository，步驟1可以改成：  
   1. 開完新的repository後，把github提供的命令先複製起來放著  
    `git remote add origin https://github.com/yellowful/note.git`  
    `git push -u origin master`  
   2. 在本機開一個和專案名稱相同的資料夾，並到到該資料夾中，下命令  
    `git init`
   3. 步驟6之前下先前複製的命令： 
    `git remote add origin https://github.com/yellowful/note.git`  
    `git push -u origin master`

## main

現在github要求主要branch要改成main，不能用master：

1. 把local端改成main的指令是`git branch -m master main`
2. 把遠端改成main的指令是`git push -u origin main`
3. 登入github，切到main branch之後，把main branch設成default。
4. 把push的遠端master刪除：`git push origin --delete master`
5. 設定遠端的head：`git remote set-head origin -a`
6. 有時候遇到錯誤訊息：`error: refname refs/heads/main not found`和`fatal: Branch rename failed`
    1. 可能head出錯了，可以下指令查詢：`git symbolic-ref HEAD` 
    2. 如果是master要改成main：`git symbolic-ref HEAD refs/heads/main`

## 開branch

1. 開新branch，例如名稱設為branch_name  
   `git branch branch_name`
2. 把現在狀態移到branch裡  
   `git checkout branch_name`
3. 開始工作，改你的檔案，改完存檔。  
4. 將這個版本上傳github：  
   1. 進度存檔：  
    `git add .`  
    `git commit -m 'first branch update'`  
   2. 因為是這個branch_name第一次上傳，所以要下github端的起始指令：  
    `git push --set-upstream origin branch_name`或是`git push -u origin branch_name:branch_name`
   3. 之後這個branch要上傳都不用了下這個指令了，而是直接下：  
    `git push`
   4. 想把主線的工作，別人新加的功能加回來這個branch的話。
      1. 切到master：  
       `git checkout master`
      2. 抓回主線的進度：  
        `git pull`  
        有時候無法同步的話，改用：  
        `git fetch --all`  
        `git merge origin/master`
      3. 切回branch：  
        `git chechout branch_name`
      4. 把master整合進來：  
        `git merge master`
      5. 檢視衝突的地方，修改檔案，存檔。
      6. 儲存進度：  
        `git add .`  
        `git commit -m 'first merge'`
      7. 上傳：(假如這個branch第一次上傳，請參考第2點)  
        `git push`
      8. 到githb的web介面上點新的pull request，pull request(pr)的意思是，對那個branch做一個request，希望他把你的修改pull過去。
      9. repository的管理者就可以點同意merge，或點comment，要求修改。或在本地端：  
        `git checkout master`  
        `git merge --no-ff branch_name`  
        `git push origin master`
      10. `merge`的用法：
          1. git checkout到直線的地方
          2. 把分支merge回來：`git merge branch_name`
5. 刪除branch：`git branch -d branch_name`

## clone branch回來

1. [網址](https://stackoverflow.com/questions/67699/how-to-clone-all-remote-branches-in-git)
2. 選擇性：
   1. 一般的方式clone下來。
   2. `git branch -a`：可以看到還沒被local建立branch的幾個branch呈現紅字
   3. `git checkout branch-name`：會自動在local建立對應的branch了
3. 全部branch都要clone下來：
   1. 一般的方式clone下來。
   2. `git pull --all`

## 第一次使用的常用步驟

1. 首先下載git，並安裝  
    `https://git-scm.com/`
2. 到github上註冊  
    `https://github.com/`
3. 在本機上設定username和email (github已經不支援這種用法了) 
    `git config --global user.name "yellowful"`  
    `git config --global user.email "richenyou@gmail.com"`
4. 目前command的用法：  
    1. 先去github網站： Settings => Developer Settings => Personal Access Token => Generate New Token => click Generate token => Copy
    2. 到自己local的repo裡：`git remote set-url origin https://<token>@github.com/yellowful/<repo>`

## ssh

1. github將會只支援ssh上傳
2. 產生ssh key：
    1. 到terminal裡面輸入`ssh-keygen -t ed25519 -C "your_email@example.com"`，其中ed25519是演算法和id，可替換成舊的演算法`rsa -b 4096`
    2. 接著會產生`id_ed25519`的檔名，要改可以改。
    3. 想一個密碼輸入
3. 啟動一個ssh-agenet用來儲存ssh key和密碼：`eval "$(ssh-agent -s)"`
4. 編輯`~/.ssh/config`，讓ssh-agent可以自動啟動

    ```text
    Host *
      AddKeysToAgent yes
      UseKeychain yes
      IdentityFile ~/.ssh/id_ed25519
    ```

5. 把private key加到ssh-agent裡面：`ssh-add -K ~/.ssh/id_ed25519`
6. copy public key到剪貼簿：`pbcopy < ~/.ssh/id_ed25519.pub`
7. 到github設定public key：
    settings => ssh and gpg keys => new ssh key => 填入(title自己隨便取)
8. 測試：
    1. `ssh -T git@github.com`
    2. 出現`but GitHub does not provide shell access`正常，只是在說不提供ssh遠端控制，只提供ssh上傳
9. 如果登入其它網站不小心被ssh-agent加入已知網站
    1. 可以這樣移除：`ssh-keygen -R 10.0.1.35`
    2. 下次可以這樣不驗證公鑰登入：`ssh -o StrictHostKeyChecking=no richard@10.0.1.35`

## personal access token

1. github取消了密碼登入的api，所以git command會無法push，這時候原本的本地repo會無法push，解決方式：
   1. 先去github產生personal access token(PAT)，可以讓local的程式把PAT當成密碼
   2. 先把遠端移除連接：`git remote remove origin`
   3. 再把遠端連接上：`git remote add origin https://<personal access token>@github.com/<使用者名稱>/<repo的名稱>.git`

## 如果和線上版本衝突

下載  
`git pull`

git會做記號，刪除不要的，保留要的，解決衝突  
`git add .`  
`git commit -m '一行說明'`  
`git push`

## 版本恢復

1. 業界應該最常用revert：
   1. `git revert HEAD --no-edit`
   2. revert掉某個進度，就會新增一個commit，而且舊commit不會被刪掉。
2. reset：
   1. 用git reset hard才是回到commit的狀態了
   2. local端還沒commit的時候，可以退回進度起點：
      `git reset`
   3. 還沒push的時候，也可以用reset來倒退版本，一個^代表退一格，以下三種方式選其中一種：
      1. git reset master^^^
      2. git reset HEAD^^^
      3. git reset 目標HEAD
3. cherry-pick：
   1. 把要的版本挑回來
   2. git cherry-pick fd23e1c
   3. git cherry-pick fd23e1c 6a498ec f4f4442
   4. 通常是挑別的branch比自己前面的commit，因為自己branch舊的commit挑回來，有可能出現，沒有empty的警告。
   5. 如果是要回到自己舊的commit，是用git reset或git revert。
4. `git checkout .`：
    1. 檔案已經track了，也已經更新了(add了)，但是還沒commit的時候，讓所有存檔的更動回到最初還沒更動之前。
    2. 檔案已經track了，但還沒更新(還沒add)，但是還沒commit的時候，讓所有存檔的更動回到最初還沒更動之前。
5. `git clean -f`：檔案還沒有track(還沒add)，讓新增的檔案回到還沒加入之前。

## .gitignore

1. 通常project都是把node_modules設成不要track的狀態，如果要把node_modules設成不要track的狀態。
    1. 在project的目錄之內，.git之外加上一個.gitignore的文字檔，裡面加上`/node_modules`就好了。
    2. 第一行最好加個comment，因為git很可能不讀第一行。
2. 如果本來就有track node_modules，後來想要untrack，要多一個步驟，完整步驟如下：
    1. 如上面步驟一樣，先加上.gitignore，裡面加上/node_modules`，先不要commit，因為現在本地端的.gitignore還沒真的生效。
    2. `git rm -r  --cached .`：用來真的untrack `/node_modules`，.gitignore才算真的生效。
    3. `git add .` 和`git commit`。
3. 如果先前沒有git rm -r --cached的話，就不會untrack，.gitignore還不會真的生效，這時候如果剛好你的`node_modules`又超大，這時候可能會發生無法上傳git hub的錯誤，這時候解決的辦法就是用git rebase。

## git rebase

1. 這個command要非常小心用，特別是如果已經push上去的commit不適合，因為會影響github上面的記錄。
2. git rebase可以用來合併commits，有一種情況是，如果node_modules太大，會無法git push。這時候如果直接加上.gitignore，然後commit了，仍然會無法git push，因為他沒有更新cache，所以仍然是在track`node_modules`，即使後來你再更新cache了，依然會無法push，因為git push的時候會把上一個還沒清cache的commit上傳，而那個commit仍然包含過大的node_modules。所以解決的方法就是要把這個已經清cache的commit和還沒清cache的commit合併在一起。
    1. <https://gitbook.tw/chapters/rewrite-history/merge-multiple-commits-to-one-commit.html>
    2. `git rebase -i 上上上個HEAD`：
       1. 意思是我要調整或修改一連串個commits，一直到上上上個HEAD。
       2. 如果要修改第一個commits，就用`git rebase -i --root`
    3. 然後會進去vim，把這次的commits前面的pick改成squash，存檔跳出。squash的意思是「和前一個commit合併」。
    4. 然後會再進入vim，把commits內容修改成你要的內容，然後存檔跳出。
    5. 這時候就可以直接push了，因為中間沒有任何commits含有node_modules的內容了。
3. 如果是已經push上去的commit，要更動的話，要用`push -f`，如果是共用的專案，會導致變動到別人的commit，要小心使用。
4. [改commit messages](https://gitbook.tw/chapters/rewrite-history/change-commit-message.html)：上面提到的`squash`改成`reword`就好了。
5. [git rebase main](https://backlog.com/git-tutorial/cn/stepup/stepup2_8.html)：
   1. 主要是為了讓main在merge別的branch的時候，commit記錄不要有別的branch記錄了。
   2. 原來一般的作法是在main裡去merge別的branch進來，這樣commit記錄圖會看到一個分支出去的路徑，這個作法是去branch裡下`git rebase main`，這樣這個branch會消失，留下main裡多了合併這個branch的記錄。

## 其他常用command

- `git commit --amend --no-edit`：[追加檔案到最近一次的 Commit](https://gitbook.tw/chapters/using-git/amend-commit2.html)
- `git ls-tree -r master --name-only`：看所有track的檔案
- `git log --oneline`：看所有commit
- `git log --graph --decorate --oneline --all`：用文字方式畫出branch的圖
- `git config --local core.ignorecase false`：讓檔名大小寫的改變要被追蹤，才不會出錯。因為舊mac不是case sensitive，但是netlify是case sensitive，所以會出錯。
- `git remote remove origin`：例如遇到remote的repo有問題砍掉重開，這時local會push不上去，也無法設remote。解決方式是在local先把remote移除，就是用這行指令。

## 版控流程

1. [三種不同流程](https://medium.com/@lf2lf2111/%E4%B8%89%E7%A8%AE%E7%89%88%E6%8E%A7%E6%B5%81%E7%A8%8B-29c82f5d4469)
2. [git-flow](https://gitbook.tw/chapters/gitflow/why-need-git-flow.html?fbclid=IwAR3szmZqaERrFk2jt8iyLhpisfRN3-fsN1sOMGb0_XM87JHU3ZKVC7QUkV0)
   - [cheat sheet](https://danielkummer.github.io/git-flow-cheatsheet/index.zh_TW.html)
   - [develop和hotfix的差別](https://softwareengineering.stackexchange.com/questions/340047/where-does-refactoring-belong-in-gitflow-branch-naming-model/340056)
     - 差別不在於修bug或是新的feature，而在於new feature是從develop分支出去，修bug則是從master分支出去一個hotfix。
     - hotfix修完要同時合併回master和develop，才不會讓develop合併回master時又衝突。
     - refactoring雖然不是hotfix也不是new feature，但是因為沒有要從master直接分支出去，所以要從develop分出去，類似new feature的功能。
     - [用source tree操作](https://gitbook.tw/chapters/gitflow/using-git-flow.html)
     - 其中會出現一個錯誤`Fatal: Local branch 'develop' does not exist.`，這代表需要自己先手動去開一個branch叫develop。
   - develop branch上傳github：`git push origin develop`
3. [github-flow](https://blog.hellojcc.tw/the-flaw-of-git-flow/)：
   1. 文章提到gitflow的缺點：
      1. gitflow的develop branch有一個問題，就是所有new feature都要從develop分支出來，這樣有可能需要將develop的commit加以整理刪除，減少不同new feature卻在develop上相依的問題，維護develop是不必要的工作。
      2. 下面的討論有不同的觀點，但總之，讚同原作者的人認為gitflow的develop一直都會存在，不會結束回master，所以對於先前版本仍會繼續維護的開發方式是適合的。但對於有開發週期，也就是一個開發會有結束然後開啟一個新的開發的話，用github-flow就好了。
   2. [github-flow的作法](https://blog.wu-boy.com/2017/12/github-flow-vs-git-flow/)
      1. github-flow的重點是
         1. new feature都從master上分支出來，不用再多維護develop的commit。
         2. 在master進行merge分支之前，在分支上都需要把master先merge過來。
      2. 開發人員只需要開新branch就好，push之後，管理人員下git tag
         1. ci / cd就會根據tag自動合併和部署，如此可以避免新人合併進錯誤的branch。
         2. 根據tag讓開發人員繼續開發
         3. tag指令範例：
            1. `git tag v0.1.0 -a -m '附註訊息'`：直接貼在現在的commit上
            2. `git tag tag_name commit_sha1 -a -m '附註訊息'`：貼在想貼的commit上
            3. 背後原理：tags和branch很像，branch是一個會跟著commits走的head，tags則是不跟著commits前進，永遠停在某一個commits的sha值，指向那個commits。
      3. [實戰](https://blog.hellojcc.tw/a-better-git-flow/)
         1. 開源專案基本上還要加上git-flow，因為大版本的bug需要開一個branch繼續一直修bug下去。
         2. git tag有版本當作tag，所以可以方便記錄版本差異。
         3. develop branch是在testing的環境
         4. release branch是在staging的環境
         5. testing和staging的環境都是在驗證
4. 狀況題：
   1. 不小心在main上面修改了檔案，根據git-flow，這應該要放到feature的branch上面，要怎麼把這個修改的檔案放到feature的branch上面呢？
      1. 把這次修改先commit起來
      2. 開一個新的feature branch
      3. 到新的feature branch上面cherry-pick這次修改的commit
      4. 回到main上面reset hard回前一次的commit

## 檔案

1. mac版的git有可能不是case sensitive的，所以可能本地改大寫，github上檔名看到仍然小寫，會出現奇怪的錯誤。錯誤的解決方式是指令`git config --local core.ignorecase false`，會記得檔名大小寫改變，但是會發生奇怪的錯誤行為。例如：file.txt => File.txt，在`git status`之下會出現`+File.txt`，但不會出現`-file.txt`。
2. 空的資料夾可能也會被忽略沒push到github上。

## 編輯時常用markdown語法

| 用途：             | 命令                              |
|:------------------|:---------------------------------|
| 標題第一階：        | \#space                          |
| 標題第二階：        | \#\#space                        |
| 換行：             | 兩個space再enter                  |
| 開頭有標示有底色縮排：| \>                               |
| 一般的向右縮排：     | 四個space                         |
| 單行code：         | 頭尾各一個back tick                |
| 單行code：         | 頭尾各三個back tick，而且要加上語言名稱|
| 數字項目：          | 數字、句點、空白                    |
| 無數字項目：        | 減號或星號、句點、空白               |
| 分隔線：           | 三個減號                           |
| URL：             | 一個小於、一個大於                   |

## 個人網頁可以善用github pages

1. 像開新專案一樣，只是命名要命成  
    `使用者名稱.github.io`
2. setting裡選擇你要的theme
3. 本地端同樣要在資料夾中新建專案
    然後要把遠地端的「使用者名稱.github.io」資料夾抓下來  
    (把yellowful替換成你自己的使用者名稱)  
    `git clone https://github.com/yellowful/yellowful.github.io`
4. 和一般專案一樣上傳  
    例如，本地端放在資料夾中的檔：  
    `/yellowful/githubNote.md`  
    和一般專案一樣上傳後  
    網址會是：  
    `https://yellowful.github.io/githubNote`  
    如果你的資料夾裡面有index.html作為首頁，例如：  
    `yellowful/index.html`  
    那麼以下網址就會直接打開你的網站了：  
    `https://yellowful.github.io/`
5. 首頁得自己做  
    以前github有The Automatic Page Generator的按鈕可以用  
    現在看起來只給企業用戶使用  
    或是可以去jekyll用：  
    `https://jekyllrb.com`

## 其它資源

- 用動畫解釋git：<https://dev.to/lydiahallie/cs-visualized-useful-git-commands-37p1?fbclid=IwAR3Pzk7-nu1PLipx8tNGVUTRox3RibIJM5rVCxcDFzZFlwOD9LfuIsyl_u4>
- 為git commit加上emoji：
  - <https://gist.github.com/parmentf/035de27d6ed1dce0b36a>
  - <https://allcontributors.org/docs/en/emoji-key>
  - <https://www.webfx.com/tools/emoji-cheat-sheet/>
  - <https://hooj0.github.io/git-emoji-guide/>
