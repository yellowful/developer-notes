# asynchronous

1. Http: hyper text transport protocol是browser和server之間的傳輸協定。
2. Request：是browser傳向server
   1. GET：像是跟twitter要一篇文章來看，透過URL傳資料給server。
   2. POST：像是貼一篇twitter的貼文一樣，透過body內文傳資料給server。
   3. PUT：像是修改推文一樣，修改已經存在的資料。
   4. DELETE：像是刪除twitter文章一樣。
3. response：是server傳資料給browser，會有一堆代號，如：200正常、403找不到網頁。
   1. 範例網頁：http://example.com/
   2. 可以用chrome裡的console的network來看到request是什麼，response是什麼。
4. Https：因為GET和POST都可以被監聽和攔截，所以容易被看到密碼，才會有https來加密，s代表secure。監看的人看到的內容沒有任何意義，只有browser和server之間看得懂。
   1. TLS：transport layer secure
   2. SSL：secure socket layer(後繼者)
5. JSON：
   1. 不同電腦用的後端語言不同，和JavaScript也不同，所以不懂JavaScript的資料，所以有JSON資料格式讓不同電腦間能溝通。
   2. 不同電腦間用純文字格式的JSON傳資料。
   3. JavaScript Object Notation
   4. 另一種資料格式是XML，較慢，將被淘汰。
   5. 把string或json轉object：(收到網路資料後)
      1. JSON.parse(資料)。
      2. 資料.json()
   6. 把object轉json或string：(送出網路資料前)
      1. JSON.stringify(資料)
      2. .json(資料)
   7. `.json()`：
      1. 為什麼網路上收到data後，要用`data.json()`？是因為收到的data是一個object，這個object有一個key叫作json，他的值是一個Promise，執行後就會回傳你要的資料了。
      2. 大概長這樣：

         ```js
         {
            json: () => Promise.resolve({
                total: 5,
                result: [1, 2, 3, 4, 5]
            })
         }
         ```

6. Promises：
   1. 因為JavaScript處理asynchronous的方式是，一些需要等的，就會丟去一個callback qeue等待執行，然後先執行其他行程式，都執行完了，等這些callback function有回應再執行他們。這樣會造成這些callback function讓我們很難很直覺的操控他們，很難讓他們依照我們要求的順序執行。如果要依照順序的話，需要一層callback包著一層callback，這就會造成所謂的the pyrimid of doom。Promises就是為了解決這個問題，他會等callback，並照then和catch順序執行，寫起來和讀起來會和synchronous的寫法很像，很好寫很好讀。
   2. 執行fetch()會得到一個Promises。
   3. Promises加句點，會把抓到的值下傳，抓到的值有兩種：
      1. resolved value：是promise第一個參數是一個function，這個function給他一個參數的話，這個參數就會是.then的value。
      2. reason：rejected
   4. promises有三個狀態：
      1. fulfilled: 會把response傳給.then()
      2. rejected: 會把error傳給.catch()
      3. pending
   5. 傳統作法JS需要把code寫清楚等資料要等幾秒...等等，但是Promises不重視這些，它重視的是未來要如何回應。
   6. Promises可以做兩個動作：
      1. 一個是對抓到的資料（Promises是resolved value，例如網路抓到正確的response）做指定的function，這時用.then()。
      2. 另一個是對抓到的資料(Promises是reason，例如沒抓到資料，或抓到錯誤資料，產生的error message)，做指定的function，通常是顯示錯誤訊息，這時用.catch()。
   7. .catch()：
      1. 只有對這一整串Promises的error執行一次，也就是整串Promises只要有一個錯，那就是錯，其他的對也沒意義。
      2. 只會抓.catch之前的error，不抓之後的。
      3. 如果想要有對不同的錯誤去抓錯，那就要分成多串，而不是放同一串。
      4. 不管.then()或.catch()裡面放的都會是一個call back function，可以用以下面方式求證：
         1. 有錯誤(err給值)才會印出來：.catch(err => {console.log('failed')})
         2. 沒錯誤(err沒給值)也會每次印出來：.catch(console.log('failed'))
         3. 有錯誤(err沒給值)才會印出來：.catch(() => {console.log('failed')})
   8. Promise.all()：
      1. 可以把一堆promise收集到一個陣列中，他會等所有資料都到了才會一次把資料傳到value中。
      2. 在設定const promise = Promise的時候，Promise就開始跑了，而不會等Promise.all才開始跑那些Promise。
      3. 這一整串陣列裡面，只要有一個被rejected，整串都會被rejected，沒加catch會出現錯誤。有加catch的話，.then()裡面不會跑，只會跑.catch()。
      4. ES2020：Promise.allSettled([promiseOne,PromiseTwo])
         1. 即使有一個promise被rejected，整串還是會跑完。
         2. 不管.then()還是.catch都會跑。
   9. Promise內部：
      像fetch這樣的Promise，他裡面在function最後會長這樣：

   ```js
      return new Promise((resove,reject)=>{
         resove(result);
         reject(error);
      })
   ```

7. ES8：
   1. 把asynchronous的寫法，像synchronous的寫法，但是仍然維持JavaScript的運作原理，synchronous的code全部跑完之後，才會跑asynchronous的程式碼。
   2. 一定要先包在一個async function內，所有要等的method前面通通要加上關鍵字await。或這樣理解更為精準，定義function前面放async，執行的function前面放await。
   3. 在async function和await method之間可以用try把這些await method包起來，用catch(err)來處理等不到callback時的錯誤訊息。
   4. 在async的function內部，非同部的fetch、setTimeOut前面都要加上await，而且所有步驟都像是sync一樣，會等一個執行完才會執行下一個。
8. ES9：
   1. spread operator：
      1. ES6時的spread operator...主要用在array，可以把array裡的items變成逗號隔開的數字的程式碼，方便coding。
      2. ES9的spread operator...擴大到object也可以用這個功能，...rest設定了rest代表的時object剩下的所有key用逗號分開的程式碼。rest不是保留字，是新變數，可以任意取名。
   2. .finally()，不管拿到的是value或是reason都會執行。
   3. await loop：可以跑for await of