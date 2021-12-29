# typescript

1. 檔案：
   1. [還沒改typescipt之前的檔案](https://github.com/aneagoie/robofriends-typescript)
   2. [改完的檔案](https://github.com/aneagoie/robofriends-typescript-completed)
2. dynamic vs static typing
   1. 語言的四個方向：
      1. 上：strong
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
      1. static typing優點：
         1. self documentation，直接看code就知道function要怎麼用了。
         2. IDE可以自動偵測typing，所以可以自動填入，比較快。
         3. production會比較少bugs，因為是事先compile的，所以還沒到production，compile time就知道哪個type會出錯了。
      2. static typing缺點：
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
5. 基本用法：
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
         4. `string[]`和`Array<string>`相同，`Array<string>`這種語法稱為泛型。
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

      5. Any：
         1. 要非常小心使用，只用在非常複雜的function，不得已必需用的地方用，否則常用的話，將無法獲得TS的好處，反而遭遇TS需要事先compile的缺點。
         2. 另一個使用的場合是，可能無法確定`JSON.parse()`回傳的type是什麼，只好用`any`。
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
         4. 主要用途是讓固定格式的object可以有一個type的名稱，以後要用這種資料結構的object就可以直接用這個名稱來設定type
         5. 範例：

            ```ts
            interface RobotArmy {
            count: number,
            type: string,
            // 其中的?表示可能有magic屬性，也可能沒有
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

         6. 另一種寫法是`type`取代`interface`再加上`=`，`=`容易忘掉：

            ```ts
            type RobotArmy = {
            count: number,
            type: string,
            magic?: string
            }
            ```

      9.  `interface`和`type`的不同：
         7. `interface`有建立一個新的名稱，可以用在各個地方，比較好用，也比較clean。
         8. `type`稱為Type alias，不會產生新的名稱。
         9. [Interface vs Type alias in TypeScript 2.7](https://medium.com/@martin_hotell/interface-vs-type-alias-in-typescript-2-7-2a8f1777af4c)
         10. [Interfaces vs Types in TypeScript](https://stackoverflow.com/questions/37233735/interfaces-vs-types-in-typescript)
6. type assertion：
   1. 可以用各種方式改變typing
   2. `as`：

      ```ts
      interface CatArmy{
      count: number,
      type: string,
      magic: string
      }

      // let dog = {}
      // dog.count // 錯誤，因為dog和dog.count都沒有type
      let dog = {} as CatArmy;
      dog.count // 正確，因為dog有type，而且裡面有count
      ```

   3. 但是這要小心使用，因為`dog`目前還是empty object，還沒有`.count`的屬性，我們只是先告訴compiler：「先不要報錯，我確定dog雖然目前是empty object，但是那只是暫時的，他的type會是CatArmy。」
   4. [assertion水很深](https://basarat.gitbook.io/typescript/type-system/type-assertion)
7. class有private的功能：

   ```ts
   class SeaOtter {
      // scream:string = 'eee~~~~';
      // public scream:string = 'eee~~~~';
      private scream:string = 'eee~~~~';
      constructor(sound:string){
         this.scream = sound;
      }
      laugh(): string {
         return this.scream;
      }
   }

   const joey:SeaOtter = new SeaOtter('haha');
   console.log('laugh',joey.laugh());
   console.log('scream',joey.scream); //　會報錯，因為是private，所以無法取用
   ```

8. union type：限制成兩種以上的type選一種

   ```ts
   let size: string | number;
   size = 'large';
   console.log('string',size);
   size = 100;
   console.log('number',size);
   ```

9. 自動給type：如果沒指定type的話type script會自動給定初始值的type，之後如果變數給別的type的值就會報錯。

   ```ts
   let x = 100;
   x = '100'; //　會報錯，因為x已經自動被給定成number
   ```

10. [definitely typed](https://www.typescriptlang.org/dt/search?search=)：這是community寫的type，因為很多library官方沒有出ts版，所以community有人寫了就貢獻出來了，可以直接搜尋到指令或下載處。
11. [create-react-app的用法](https://create-react-app.dev/docs/adding-typescript/)
    1. 原本的CRA專案的話，可以用上面官方作法開一個新的ts的cra app，然後把source code copy過去。
    2. `package.json`可以在`devDpendencies`裡面看到`@types/jest`、`@types/react`、`@types/react-dom`等等，這些都是幫這些library裡面的type事先定義好了。
    3. 可以到module裡面的`@types/react`裡，可以看到已經幫我們定義了很多：

       ```ts
       type NativeFocusEvent = FocusEvent;
       type NativeMouseEvent = MouseEvent;
       type NativeKeyboardEvent = KeyboardEvent;

       interface ReactElement<P> {
         type: string | ComponentClass<P> | SEC<P>;
         props: P;
         key: Key;
       }
       type ReactChild = ReactElement<any> | ReactText;
       type ReactNode = ReactChild | ReactFragment | ReactPortal | string | boolean | null | undefined;

       ```

12. React裡：
    1. interface的命名習慣通常用`I`作為開頭，例如'`IAppProps`、`IAppState`、`IRobot`…等等。
    2. React裡的特殊用法，例如component的用法：`class App extends React.Component<IAppProps, IAppState>{}`
    3. 副檔名是`tsx`
    4. interface在其它component要用，就要export。
    5. 參考[PJ的筆記](https://pjchender.blogspot.com/2020/07/typescript-react-using-typescript-in.html)
       - [cheat sheet](https://github.com/typescript-cheatsheets/react)
    6. robot的解答：
       1. root要用assertion設成HTMLElement的interface。
       2. destructure的用法
       3. stateless component的用法：`React.SFC<CardStatelessProps>`，去`@types/react`裡可以查得到，另外`CardStatelessProps`要自定props長什麼樣子。
       4. children的type是`JSX.Element`。
       5. event handler當成props的用法，和定義event handler時候的用法是一樣的
       6. event的用法：`event:React.SyntheticEvent<HTMLInputElement>`
       7. life cycle的用法
          1. render看情況是`JSX.Element`或是`React.ReactElement`
       8. 做完的好處是開發時某些type錯誤，會在compile時馬上發現。
13. 結論：
    1. JAVA是非常static的語言，要做一件事要寫很多code很詳細才能完成，這也是很多人愛JAVA的原因，但是python是非常dynamic的語言，可以非常簡短達成要的功能，也有他自己的優點。
    2. TS的確能減少bug，但是並不一定什麼project都要導入，要考慮：
       1. 環境。
       2. team。
       3. 目標。
       4. 時間，特別是需要快速建出新feature的時候，並不適合。
       5. unit test是不是可以取代typescript？
