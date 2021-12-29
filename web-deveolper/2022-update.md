# Complete Web Developer in 2022: Zero to Mastery: 💻 

## 2022 update

### lecture 22: Build Your First Website

- 試著拉index.html檔案去chrome，可以打開網頁，這可以模擬server把檔案丟給browser

### lecture 52:[Optional: PX, EM, REM, %, VW, and VH](https://elementor.com/help/whats-the-difference-between-px-em-rem-vw-and-vh/)

1. em：相對於爸爸
2. rem：相對於root element
3. %：相對於爸爸
4. vw：相對於viewport的寬度
5. vh：相對於view port的高度

### lecture 66：Evolving Technology

   1. 開發者似乎總是需要跟上新的科技，例如：
      - text editors
      - libraries
      - best practices
      - programming languages
      - dependencies
      - apis
   2. 但是這個課程並不是像其它課程只教當時最新的best practice的步驟，這個課程還有教如何跟上新的技術，這些知識以後都一直用得到。只要能跟著科技不斷進化，一定會是個很棒的開發者。

### lecture 69: Bootstrap 5 Update

   1. project要更新到bootstrap 5只需要把bootstrap的連結改成新版本的連結就好了
   2. project裡mailchimp看不到登入表格的話是被chrome的adblock擋掉了，只需要用無痕模式就看得到了。

### lecture 107: 2022 Updated Statistics

- 只是更新數據

### lecture 145: jQuery

   1. 以前瀏覽器並不支援所有javascript的語法，所以要上caniuse.com查，而jQuery解決了這個問題。
      jQuery範例：
         如果html裡要把js放在這裡面：

         ```js
         $(document).ready(function(){
            這裡可以放上所有要寫的程式碼
            })
         ```

         如果要選擇`<p>`的話，下面兩個語法相同：

         ```js
         $(p).click(function({
            // 讓<p>隱藏
            $(this).hide();
         });
         ```

         ```js
         p.addEventListener('click',function(){
            this.hide();
         })
         ```

   2. [教你如何用原生js取代jQuery](https://youmightnotneedjquery.com/)

### lecture 175: ES2021

1. `str.replaceAll('best','worst')`可以把字串裡所有的'best'改成'worst'
2. 原本的js就有`str.replace('best','worst')`的語法，但是只能替換掉第一個'best'，新語法可以替換掉每一個。

### lecture 209: Update: Latest Node.js and NPM

- 只有node版本更新

### lecture 212: Why Update Packages?

1. github上面會有安全警告，可以在上面update
2. [可以查npm install時，會更新到哪個版本](https://semver.npmjs.com/)

### lecture 216: Create React App

- npx能用CRA，而不需要globally更新react

### lecture 217: React App Folder Structure

1. `package.json`裡面的`npm start`，就是`react-scripts`這個module幫我們寫好的一個start.js在做的事情。
2. `package-lock.json`保證你把檔案搬去別的地方也可以run，因為這是package.json所產生，裡面記錄你確實安裝的所有dependency和它們的版本。
3. `./public`：
   - 裡面主要放favicon、真的要被React render的html、用來產生pwa icon的manifest.json。
   - 最重要的是html裡面有一個`<div id="root"></div>`
4. `./src`：
   - 所有邏輯都放這裡面
   - `index.js`裡面的`document.getElementById('root')`就是用來把所有邏輯串上`./public`裡面的`<div id="root"></div>`。

### lecture 218: React Fundamentals

1. `index.js`是所有file的進入點(entry)，app第一個會執行的檔。
2. `react-dom`這個library是用來連接react和DOM。
3. `ReactDOM.render(第一個參數，第二個參數)`，其中第一個參數要放我們要顯示的jsx寫的component，第二個參數放我們要用哪一個dom來render。

### lecture 219: React Fundamentals 2

1. 身為開發者，永遠要去測試，測試老師講的或是我自己想的是不是正確。
2. 例如把`<App />`改成`<h1>hello</h1>`看看是不是能跑，結果真得能跑。
3. jsx就是custom html tag，例如`<App />`

### lecture 220: Class vs Functional App.js

- React.Component給我們react package裡面的能力，這能力會偵測render()這個method，並且會自動把render()裡面return的jsx給render出來。

### lecture 221: Hooks vs Classes

1. Hooks的問題是elephant in the room
2. class component是很重要的概念，因為其它語言也用得到class。

### lecture 226: Exercise: Learn to Read the Docs

1. lecture 220 Andrei說`<React.StrictMode />`只是給我們額外的保護。
2. [官方文件](https://reactjs.org/docs/strict-mode.html)：
   1. 只有在development mode有作用
   2. 可以在任何jsx裡局部使用
   3. 會對一些不安全、過時、直接操作DOM、不預期的side effects的寫法或life cycle提出警告

### lecture 235: Keeping Your Projects Up to Date

1. 更新robofriends
2. 強制更新`npm audit fix --force`，小心break dependency。
3. `npm update`更新了version，但是`^`限制了最高版本。如果不限制最高版本，例如`react-script`前面沒加`^`，update之後就從`1.x`更新到`3.x`

### lecture 254: ES2021: any()

- `Promise.any()`沒什麼用處，例如：

   ```js
   (async function () {
   const result = await Promise.any([p1, p2, p3]);
   console.log(result); // Prints "A", "B" or "C"
   })();
   ```

- 只要其中有一個promise最先resolved了，就會用那個當成result，如果沒人resolved了，就丟error。

### lecture 263:

- react-tilt沒在維護了，改用[react-parallax-tilt](https://www.npmjs.com/package/react-parallax-tilt)

### lecture 265: Before We Start The Big Part

1. final project的github code是正確可以跑的，每個星期都有自動測試。
2. clarify的api已經改用gRPC，課程最後會討論，但課程仍然先用舊的API，因為這種用法在業界仍比較普遍
3. 把Clarify module import之後，`console.log(Clarify)`出來，可以得到所有的`model id`

### lecure 267: Clarifai API Updates, Models and Troubleshooting

1. 可以上clarifai官網測試clarifai的API是否正常運作
2. 可以換自己需要用的model
3. `Clarifai.GENERAL_MODEL`很酷，看得懂相片，可以玩一玩

### lecture 272: Introduction To Node.js

- object：
  - 沒有`window`、`document`、`window.fetch`、`global.fetch`
  - 但是有`global`、`process`

### lecture 327: Optional: Using gRPC API

1. gRPC目前還不多見
2. [gRPC vs REST](https://www.imaginarycloud.com/blog/grpc-vs-rest/)
   - gRPC速度比較快
   - google Remote Procedure Call：也就是google定的
   - REST用JSON，gRPC用Buffers
   - gRPC比較少browser支援，不像REST支援所有環境
3. smart brain改成gRPC的做法
   1. 開啟clarifai nodejs的[github repo](https://github.com/Clarifai/clarifai-nodejs-grpc)跟著做
   2. 到後端`npm install clarifai-nodejs-grpc`
   3. 貼上，並改上你的key

      ```js
      const {ClarifaiStub, grpc} = require("clarifai-nodejs-grpc");

      const stub = ClarifaiStub.grpc();

      const metadata = new grpc.Metadata();
      metadata.set("authorization", "Key YOUR_CLARIFAI_API_KEY");
      ```

   4. 原本的require和api key的程式碼可以comment掉。
   5. 再把這段奇怪的程式碼copy來貼上，我們開發者有時候就是要這樣，把奇怪的東西弄懂，把問題解決，這門課的目的就是幫你建立解決問題的腦中肌肉。

      ```js
      stub.PostModelOutputs(
         {
            // This is the model ID of a publicly available General model. You may use any other public or custom model ID.
            model_id: "aaa03c23b3724a16a56b629203edc62c",
            inputs: [{data: {image: {url: "https://samples.clarifai.com/dog2.jpeg"}}}]
         },
         metadata,
         (err, response) => {
            if (err) {
                  console.log("Error: " + err);
                  return;
            }

            if (response.status.code !== 10000) {
                  console.log("Received failed status: " + response.status.description + "\n" + response.status.details);
                  return;
            }

            console.log("Predicted concepts, with confidence values:")
            for (const c of response.outputs[0].data.concepts) {
                  console.log(c.name + ": " + c.value);
            }
         }
      );
      ```

   6. `model_id`改成你要的model的id，原來smart brain後端的`handelApiCall`裡面放上面這段奇怪的code，這樣就可以把裡面的url可以改成原本的url的變數`req.body.input`，並且把收到的結果在後端`console.log`出來。
   7. 可以看得出這個程式碼裡面有一個處理error的地方，有一個判斷status.code不是10000的地方。後者就是放資料正常的時候要做的事，在這個地方把response回傳給前端。
   8. 我們只是用我們這門課已建立的解決問題的頭腦想出來怎麼做，寫程式就單純是一個語言，假如我們了解一些基本的原則，我們就可以看得懂這些程式碼是在幹麼。
