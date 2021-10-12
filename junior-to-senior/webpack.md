# webpack：116堂課

1. bundler：
   1. 用來把html、css、javascript這些檔案進行優化的工具。
   2. webpack：
      1. 穩定
      2. 彈性，可以很多設定
      3. 大project會用
   3. parcel：簡單，給他檔案，就直接給出bundle
   4. rollup.js：
      1. tree shake做的很好，可以避免掉很多不必要的code。
      2. 要發行npm package的話可以用。
2. 結構：
   1. entry：進入點，例如`index.js`，可以從進入點獲得其它所有的javascript檔案。
   2. output：輸出的`bundle.js`，`/dist`是distribution的意思。
   3. loader：轉換code，裡面有babel之類的，可以轉成比較舊的版本的js。
   4. plugin：用來改code。
   5. webpack dev server：用來在local靜態放htlm和js檔。
3. 實作：
   1. 資料夾：
      1. 先建一個主資料夾，並且'npm init -y'，以得到一個package.json
      2. 主資料夾裡建一個目的資料夾`dist`，裡面放一個index.html
   2. 安裝：`npm install --save-dev webpack webpack-dev-server webpack-cli`
   3. `package.json`：
      1. start改成`webpack server --config ./webpack.config.js --mode development`(更新的指令)
      2. 其中：`--mode development`是webpack 4才有的功能，讓很多東西可以default比較方便。
      3. `webpack.config.js`：
         1. 主要是用`module.exports`一個object
         2. `var path = require('path')`，用`path.join(__dirname,'dist')`比較不會弄錯。
         3. 設定entry，是一個字串的array，沒設的話，default就是`'./src/index.js'`
         4. output是一個object：
            1. path：out put目標位置，`__dirname+'/dist'`
            2. publicPath：相對於index的位置，我想應該就是指output相對網站網址root的path，`'/'`
            3. filename：`'bundle.js'`
         5. devServer也是一個object：
            1. local的server
            2. serve的資料夾，例如：

               ```js
               devServer: {
                 static: {
                       directory: path.join(__dirname, 'dist'),
                 },
                 compress: true,
                 port: 9000,
               },
               ```

            3. `contentBase:'./dist'`：已經deprecated，改成static了
      4. `index.html`：
         1. 如果是給react用，可以在body裡給一個`<div id="root"> </div>`
         2. `<script src="/bundle.js"> </script>`放在`</body>`之前，`bundle.js`對應out put的pathe和filename
4. babel：(119堂更新為準)
   1. 安裝：`npm install --save-dev @babel/core @babel/preset-env @babel/preset-react babel-loader`
      1. `@babel/core`是用來解讀新的語法
      2. `@babel-loader`是compile，把新語法轉成舊語法。
      3. `@babel/preset-env`是用來測試這是哪個瀏灠器，還不會用哪個語法，針對不支援的語法做轉換就好，可以大大提升效能。
   2. `package.json`裡面babel需要設定
      1. presets裡放一個字串的array：
         1. 然後設一個`"@babel/preset-react"`的目的是把jsx轉成js語法。
         2. "@babel/preset-env"
      2. 如果要測試javascript新功能：
         1. 可以安裝`babel-preset-stage-0`，但有的語法還不確定支援，官方已經不建議了。
         2. 想要什麼新的語法，就要在`package.json`裡面babel裡面的`plugins`增加那個語法，plugins是一個字串的array例如：`["@babel/plugin-proposal-class-properties":"7.0.0","...."],`
   3. babel的設定可以放在package.json裡面，也可以另外設一個.babelrc.json檔案，把設定放在裡面。
5. webpack.config.js：
   1. module是用來處理input和output之間要做的事。
   2. module裡面的rules是一個object的array，每一個item是一個object，每一個object可以有以下的東西：
      1. test是過濾的意思
         1. 用regex語法說「過濾出所有的js和jsx檔案」
         2. 例如：`/\.(js|jsx)$/`
      2. exclude是不需要的意思，裡面也是放regex語法，例如：`/node_modules/`
      3. regex直接加就好了，不是string，不需要加引號
      4. use是放一個string的array，是在說過濾後的檔案要用什麼來處理
         1. 例如：`['babel-loader']`
         2. 意思就是上面條件的檔案都要用`babel-loader`處理
   3. resolve：
      1. 作用是在create react app裡面，import都不用加.js和.jsx就是用這個功能做的。
      2. 例如：`extensions:['.js','.jsx']`
6. React：安裝`react react-dom`
7. eslint：
   1. 新作法用`eslint-webpack-plugin`，[參考documentation](https://www.npmjs.com/package/eslint-webpack-plugin)
      1. 先`npm install eslint eslint-webpack-plugin --save-dev`
      2. 在`webpack.config.js`裡：
         1. 最上面`const ESLintPlugin = require('eslint-webpack-plugin');`
         2. `module.exports`裡面放`plugins: [new ESLintPlugin(options)]`
         3. options可以放：`{extensions:['.js','.jsx']}`
         4. 舊作法用`eslint-loader`：已經deprecated
            1. `npm install --save-dev eslint-loader`讓webpack.config.js裡面可以設定讓eslint檢查所有的js檔案。
            2. 一樣在`webpack.config.js`裡的rules，要放進`'eslint-loader'`
   2. rc檔是`package.json`的一部份，比較大的設定，都可以獨立出去成為一個rc檔。
      1. `.eslintrc.json`檔讓eslint-loader可以在run的時候可以設定，是必要檔案。
         1. `.eslintrc`已經deprecated了，要用`.eslintrc.json`。
         2. 裡面只要先放一個rules的object就可以跑了，例如：
            1. "no-console":"warn"
            2. "no-console":"error"
         3. [其它rules](https://eslint.org/docs/rules/)
         4. 開始跑的時候，可以去network裡面看一下`bundle.js`裡面會有webpack改寫js的code，可以看看有沒有問題。
         5. 要檢查es6, es7的語法就要安裝babel-eslint
            1. `npm install --save-dev babel-eslint`
            2. `.eslintrc.json`裡面要設定`parser:"babel-eslint"`
      2. babel在`package.json`裡的設定，也可以獨立成一個rc檔。
   3. eslint airbnb：
      1. 根據documentation，osx應該下這個指令：

         ```shell
         export PKG=eslint-config-airbnb;
         npm info "$PKG@latest" peerDependencies --json | command sed 's/[\{\},]//g ; s/: /@/g' | xargs npm install --save-dev "$PKG@latest"
         ```

      2. 灌完了，其實是會自動灌以下幾種eslint：
         1. `eslint-config-airbnb`
         2. `eslint-plugin-import`：檢查import statements
         3. `eslint-jsx-a11y`：檢查是不是符合輔助視障或身障使用者
         4. `eslint-plugin-react`
         5. `eslint-plugin-react-hooks`
      3. airbnb有很多很嚴格的規則：
         1. expresion後面一定要加分號。
         2. 逗號後面要有空格
         3. 後面還可以加東西，沒加的話，仍要加逗號。
   4. `.eslintrc.json`裡面的extends代表接受前面的規則，然後增加這些規則。例如參考eslint-airbnb的github加上：`"extends":["airbnb-base"]`。代表的是接受前面的babel-eslint之外，再加上airbnb的規則。
8. 其它：
   1. 客制create react app：可以先執行eject，然後再用create react app，就可以去改webpack.config.js檔案了。
   2. parcel：和webpack一樣，但是不用設定，全部有內設值，超簡單。缺點是比較沒有彈性。
   3. 用這個app來設定webpack：<https://createapp.dev/>
9. tree shaking：141課
   1. webpack和babel在背後幫我們做的事情，如果我們import一個很大的library，只用到一點點，tree shaking就會幫我們把這些沒用的程式碼弄掉。
   2. <https://developers.google.com/web/fundamentals/performance/optimizing-javascript/tree-shaking/>
10. webpack的缺點：
    1. 設定很繁瑣
       1. 要用時才去查，不要狂讀documentation，因為這工具變化很快，很快就不一樣了。
       2. webpack背後機制還是需要了解。
    2. 很常發生有的地方副檔名要`.`有的地方不用`.`，要小心。
    3. 而且錯誤訊息不明確，所以要參考documentation怎麼設定的

## 自己練習的project

1. [repo](https://github.com/yellowful/webpack-project)
2. 還可以增加的：
   1. 增加一點功能，例如練習shouldComponentUpdate的button
   2. webpack要增加 production 的設定，可以deploy到網站
   3. parcel版
