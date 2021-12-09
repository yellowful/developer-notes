# NPM, Browsify

## NPM

1. node是讓JavaScript可以在瀏覽器以外運行的環境。
2. node和chrome一樣是v8引擎。
3. NPM是管理JavaScript Library的社群，現代寫程式，很少從頭開始寫了，都組合別人的Library。  
    要用的話要先去下載node.js
4. 版本更新：
   1. NPM：`sudo npm install -g npm@latest`
   2. Node：直接官網下載安裝，內含新的npm
   3. 看webpack版本：npm ls webpack
5. react就是facebook開發的一個Library：
   `sudo npm install react`
6. .json：
   1. 在專案資料夾下，下指令就會自動產生：
   `npm init`
   2. 在超大的project下，在一個.json檔就把所有dependencies的函式庫整理很清楚，甚至連版本都標清楚了，不用「手動把檔案下載下來後，還一個一個js檔關聯到html裡」，所以很好用。
   3. 別人要把dependencies裡的函式庫都下載，只需要要在有.json檔案的地方執行：
   `npm install`
   4. .json檔的dependencies還可以控制，正式上線前，把需要的module弄進來，其他不要放進來，以減少檔案大小。
7. 不錯的package：
   1. live-server：會偵測檔案是否更動，如果有更動，會push給browser，browser就會自動refrash。
   2. lodash：更多function可以用
   `npm install lodash`
   3. -g：
      1. -g代表整台電腦都能執行，json檔不會被更動
      2. 不用-g就是只有這個資料夾，json檔會被更動，dependencies顯示關聯到哪個函式庫，而且會多一個loadash的資料夾。資料夾不需一起走去別的電腦或github上，因為別處只需要自己install一次lodash就好了。
   4. --save：代表package.json會記載dependency
   5. --save-dev：代表package.json記載的dependency只供開發時用，其他人安裝你的project的時候或depoly到heroku時不會被安裝。
8. npm讓使用者可以從.json檔裡下command指令：
   1. 在.json檔裡：
   2. `"scripts":{"browserify script.js > bundle.js && live-server"}`
   3. 在command line裡：
   4. `npm run build`
9. [nvm](https://www.sitepoint.com/quick-tip-multiple-versions-node-nvm/)：
   - 可以讓不同的project用不同的node版本。

## semver

1. Semantic Versioning版本命名
2. 功能大改.功能小改.bug修復

Lecture 204: Why Update Packages?
   github security alerts會顯示需要更新的版本
   <https://semver.npmjs.com>
      可以看版本穩定度
      ^: 某一大版
      >=: 大於等於某一版
Lecture 259: Introduction To Node.js
   有：
      global
      process
      process.exit()
   沒有：
      window
      global.fetch
      document
Lecture 266: ES6 Modules In Node
   可以用import了
      方法1：在package.json裡面加上`"type": "module"`
      方法2:副檔名改成mjs