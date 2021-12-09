# typescript

1. 檔案：
   1. [還沒改typescipt之前的檔案](https://github.com/aneagoie/robofriends-typescript)
   2. [改完的檔案](https://github.com/aneagoie/robofriends-typescript-completed)
2. dynamic vs static typing
   1. 語言的四個方向：
      1. 上：string
      2. 右：static，第1、4象限
      3. 下：weak
      4. 左：dynamic，第2、3象限
   2. 象限：
      1. 第1象限：c#, scala, haskell, java, f#
      2. 第2象限：Erlang, Groovy, Ruby, Python, Clojure
      3. 第3象限：Perl, VB, PHP, JS
      4. 第4象限：c, c++
   3. dynamic typing
      1. typing決定在run time，宣告時不需要決定typing。
      2. 比較彈性，開發速度較快。
   4. dynamic vs static typing
      1. 優點：
         1. self documentation，直接看code就知道function要怎麼用了。
         2. IDE可以自動偵測typing，所以可以自動填入，比較快。
         3. production會比較少bugs，因為是事先compile的，所以還沒到production，compile time就知道哪個type會出錯了。
      2. 缺點：
         1. 程式碼較複雜較難讀。
         2. 需要花時間學習。
         3. type checking在testing就可以儘量做得到避免bug，而static typing並無法完全避免bug。
         4. 降低開發速度。
   5. strong vs weak typing
      1. weak typing：
         1. typing可以變，例如js裡，數字和字串相加時，數字可以自動轉字串，稱為(implicit) coercion (西語是constrain的意思)。
      2. strong typing：
         1. 不能隨便轉換，例如python要把字串和數字相加，會出錯。
3. js的static type的工具：
   1. flow：
      1. 透過babel轉成js
      2. facebook開發的
      3. 很常和react配合做type checking
      4. 在create react app之後，加上兩行命令就可以用了
      5. 在檔案的開頭加上 // @flow，這時候flow就會幫你檢查這個檔案的所有type了。
   2. typescript
      1. 透過ts轉成js
      2. 是個javascript的super set，意思是在javascript上加上功能，也就是ts就是js，只是可以在js上加上ts的功能。
      3. ts有自己的compile，要把ts轉換成js。
   3. [reasonml](https://reasonml.github.io/)：
      1. 和ts不同的是，reasonml是一種獨立的程式語言，並不是js，只是跟js很像，讓js的人可以很容易學起來。
      2. facebook開發的
   4. elm
      1. 和reasonml原理一樣，是一種獨立的程式語言。
      2. 透過elm轉成js
   5. 學ts的原因：
      1. elm和reasonml很少人用，flow和ts比較多人用，而ts又大大成長很多人用。
      2. angular直接是用ts開發，預設ts。
      3. 為什麼angular要採用ts，因為angular像是廚房，什麼都有，很多人參與開發，所以最好是什麼都定義好了，讓大家直接follow規則，比較容易一起工作。
      4. 適合使用ts的情境：team成長愈來愈快，程式碼愈來愈需要document來避免這麼多人加入產生bug，而且公司可以忍受開發速度變慢。
4. ts compiler：
   1. 先上官網看doc和安裝
   2. 需要node.js環境，而且這個compiler是用ts寫的然後compile成js。
   3. 安裝完打`tsc`
      1. 可以看到版本
      2. 可以看到怎麼用comiler
   4. 如果[tsc不能用](https://stackoverflow.com/questions/39404922/tsc-command-not-found-in-compiling-typescript)
   5. 開一個資料夾，裡面的`.ts`檔就可以被編譯了。
5. 開始使用：
   1. 程式檔名`.ts`。
   2. compile：
      1. `tsc xxx.ts`之後會跑出一個js檔。
      2. 會發現轉成的js會是es5的語法，具有babel的功能。
      3. `tst --init`會產生一個`tsconfig.json`，可以對ts做設定
      4. `tsconfig.json`的`target`的地方可以改成es5或其它es版本，這和babel一樣的功能。
      5. `tsc xxx.ts --watch`，就不用一直下指令。
   3. function的parameter裡面可以設type：
      1. compile之後的js檔裡不會有type
      2. 在用function的時候，滑鼠會出現type提示
   4. 語法：
      1. `let isCool: boolean = true`只能在`.ts`裡面使用，在`.js`裡使用會錯誤。
      2. 型別：
         1. `boolean`
         2. `number`
         3. `string`：可以是template string
         4. `string[]`和`Array<string>`相同
         5. `object`：參考interface
         6. `null`
         7. `undefined`
      3. Tuple：`let basket: [string, number];`
      4. Enum：
         1. 限制性選擇，原本js沒有這個功能，現在可以用了
         2. 不是這樣使用`const Size: enum = {Large = 1, Medium = 2, Small = 3};`，而是以下兩種常用用法

            ```ts
            enum Size {Small = 10, Medium = 20, Large = 30}
            let sizeName: string = Size[20];
            alert(sizeName); // 顯示Medium
            let sizeNumber: number = Size.Small;
            alert(sizeNumber); // 顯示10
            ```

         3. 其實Size等於以下的object：

            ```js
            Size {
            '10': 'Large',
            '20': 'Medium',
            '30': 'Small',
            Large: 10,
            Medium: 20,
            Small: 30
            }
            ```

      5. Any：要非常小心使用，只用在非常複雜的function，不得已必需用的地方用，否則常用的話，將無法獲得TS的好處，反而遭遇TS需要事先compile的缺點。
      6. `void`：function沒有return時，這個function就是void。
      7. `never`：很難理解，表示function沒有執行到最後，如果function有執行到最後，沒有丟出error的話，那麼就會出錯。

         ```ts
         // 因為function沒有執行到最後，所以符合
         let error = (): never => {
         throw Error('blah!');
         return 'lalala'
         }
         ```

      8. `interface`：
         1. 變數名稱應該要大寫開頭
         2. 用超多
         3. 等於是自定的type
         4. 範例：

            ```ts
            interface RobotArmy {
            count: number,
            type: string,
            magic?: string
            }

            let fightRobotArmy = (robots: RobotArmy) =>{
            console.log('FIGHT!');
            }
            // 這個寫法等同於上面，但是就沒有一個自定型別的名稱可以在其它地方使用
            let fightRobotArmy2 = (robots: {count: number, type: string, magic?: string}) =>{
            console.log('FIGHT!');
            }
            ```

         5. 另一種寫法是`type`取代`interface`再加上`=`：

            ```ts
            type RobotArmy = {
            count: number,
            type: string,
            magic?: string
            }
            ```

      9. `interface`和`type`的不同：
         1. `interface`有建立一個新的名稱，可以用在各個地方，比較好用，也比較clean。
         2. `type`稱為Type alias，不會產生新的名稱。
         3. [Interface vs Type alias in TypeScript 2.7](https://medium.com/@martin_hotell/interface-vs-type-alias-in-typescript-2-7-2a8f1777af4c)
         4. [Interfaces vs Types in TypeScript](https://stackoverflow.com/questions/37233735/interfaces-vs-types-in-typescript)