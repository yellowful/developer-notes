# bootstrap

1. [repo](https://github.com/yellowful/bootstrap-exercise)
2. bootstrap已設寫好很多class，所以可以利用這些class可以快速設定自己要排版、位置、顏色、大小。
3. 要控制東西水平靠中間可以用的技巧：
   - 用css的property：text-align:center、display:flex加上justify-content:center、margin:auto
   - 用bootstrap的class：text-center、row配合col-12、d-flex配合justify-content-center
4. flex：
   1. flex是伸縮的意思，就是那個div會隨著螢幕變大變小。
   2. display的flex和inline-flex有一點點不一樣，inline-flex可以用在內部的div想取消flex的時候用。
   3. flex-direction: column;時，可以讓這些div直直的排下來，要注意的是，這些div預設會張開到最寬。
5. 要控制東西垂直靠中間可以用的技巧：
   - html：注意div或是html的height是不是100%、margin
   - 用css的property:display:flex加上align-items:center
   - 用個空的div，設定height
   - 用bootstrap的class：d-flex配合align-items-center
6. 必走程序：
   - 拷貝css的link到head中
   - 拷貝js的link到body最下面，如此可以不用一開始就載入
   - 若有要用google字型，link也拷貝到head中
   - 拷貝兩個meta，一個是utf-8，另一個是viewport，主要是讓網頁可以成為rwd
   - 設定css檔，讓html和body的寬度和高度都是100%
   - 把自己的css檔的link加到head中，而且要放在bootstrap的link之後，因為如此自己的property才會蓋過bootstrap的property。
7. bootstrap實作技巧心得：
   - 不用自己在另一個css檔，寫css的property，直接呼叫class名，就可以產生那個特性。
   - 如果想改變bootstrap的property，可以直接另一個css檔裡，同一個class增加自己想要的property，而且會一致性的修改，所有呼叫到那個class的元件都會被修改，很有一致性。
   - 如果只是要改某個或某些特定元件，並不想改動到其它呼叫到bootstrap的同個class的其它元件，可以增加一個自定的class，給該元件用。
8. grid
   - html nav來作manu
   - 用ul和li來弄nav裡的一個一個的選項，不一定用margin來弄開左右的間距，可以用padding
   - 用margin-left:auto來取代div和flex的around-between，auto表示會自動填滿左邊
   - 用@media來設定max值，當小於這個值時，就用另一套css，可查css tricks的media queries，而且我發現，放在css檔的最上面是沒有用的，一定要放在所有有用節的屬性的下面，才會起作用。
   - 通常螢幕小於一個值時，設計時會把nav的值變小，比較好看
   - 設定高度的大小，要用vh(viewport height)來取代height，會以營幕的總高度當單位，會比較responsive，50vh代表螢幕高度的50%。好處是也不會像用grid-template-rows的設定一樣，被限制住，如果去個別div改尺寸，會跑出margin。
   - 要消除div旁邊的白邊，就要去設定body的margin是0
   - img的大小用%來設定，就會依螢幕大小變化
   - 要會固定nav的功能，用position:fixed
9. 美化網頁的免費資源：https://zerotomastery.io/resources/
   - 繪圖：https://undraw.co
   - 照片：https://unsplash.com
   - 照片：https://www.pexels.com
10. mailchimp：
    1. 可以在mailchimp上設定landing page，供自己的網站或web app轉過去。
    2. 可以設定不同的landing page，以建立不同的會員群組。
    3. 有api可以讓後端連結。
