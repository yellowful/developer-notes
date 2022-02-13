# html

1. [練習的project](https://github.com/yellowful/html-exercise)
2. 基本要記的tag有
   - paragraph
   - strong
   - emphasize
   - oder list
   - unoder list(又稱為bulleted list)
   - list
   - horozontal line
   - image source(alt屬性是讓圖片無法正常顯示時，可以顯示的文字)
   - ankor reference
3. form：
   1. sumit一定要搭配form才會有效
   2. react底下的form
      1. 不用submit的話：
         1. 原因是直接用submit搭配form，會引發網頁離開。
         2. 可以改用tag button。
         3. techyons裡的form template有一堆有的沒的tag最好都刪掉，或改成div，以免出現怪異行為。
         4. 例如for、value在Reac裡沒什麼用，可以刪掉。
      2. 如果要用submit的話：
         1. form tag的method要設定"post"
         2. form tag要設onSubmit和對應的handle submit
         3. handle submit要執行event.preventDefault()，避免reload
         4. button tag要設type="submit"
      3. tags:
         1. `<fieldset>`：分組。
         2. `<legend>`：分組的標題。
         3. `<label>`：用來做為input的標題。
4. inpute的type有很多種：
   1. text
   2. textarea
   3. search：會出現xx讓你清除輸入的字
   4. file：
      1. 注意，onChange如果兩次點相同的檔名，不會觸發event。
      2. 用javascript先把檔案指定給某const，然後把這個event清空，就會觸發這個event了。 
5. division會把某段html隔絕成一區，在css中很好用，span很類似的功能，但是只專注在設定某行
6. div和span和內容無關，html 5和內容有關的tag，稱為semantic\(語法\)，例如：header, nav, footer，html 5新的semantic"有可能"讓google搜尋排名提高。
   感覺排版則沒什麼差別。
7. 設定背景顏色是style="background-color:yellow;"，設定背景相片是用style="background-image:url()"
8. table要記的有table head包住的會置中粗體，table row包住每一列，table data是包住每一格的內容
9. 播自己的影片檔用video的tag，播youtube的影片用iframe的tag，自己的聲音檔，用audio的tag可以播放
10. 要讓一整塊地方可以填文字要用textarea，用rows和cols來設定區域大小。
11. 如果要某一個區域讓使用者可以編輯文字，就要用一個div，屬性contenteditable設成true。
12. 要讓輸入的文字，當一離開文字框點別的東西，就submit出去，就要加上onblur的「事件屬性」。
13. 可以用svg的tag直接將svg向量圖嵌入html
14. 最基本的輸入的tag是input設定的type是text，其他常用的type還有search、date、email、password、number、summit、button、checkbox
    1. checkbox是複選
    2. radio是單選
       1. 純JS底下的處理方式：
          1. html裡每一個radio input要設相同的name attribute，例如：gender。
          2. 在form sumit的時候，要取得radio的值得話，要用for loop
             1. 去select所有name是`'gender'`的input。
             2. 挑出attibute `checked`是true的value。
       2. jQuery底下，在form sumit時要取得radio的值的話：`$('input[name=radioName]:checked').val();`
       3. react底下要去操作radio先預載某個選項的話：
           1. 要單一選項的checked的property設成true，其他選項設成false。
           2. 不能用onClick，要用onChange。
           3. name是用來設定單選群組的，所以name不要使用。
           4. 外面包label用來顯示選項名稱。
15. 拉條的做法是input type設成range，生命值這種顯示條是設定meter的tag
16. 單選和多選的這種點選的選項，因為文字配合變化多端，所以要顯示的文字不需要被包在tag裡，而是value的屬性填入要顯示的文字就好，所以直接用input屬性設radio或checkbox就可以了。
17. 但下拉式選單，裡面每一個選項顯示方式固定，所以直接包在tag裡就好了，所以是用select配合option的tag處理。
18. html 5 semantic：
    <https://www.w3schools.com/html/html5_semantic_elements.asp>
    `<header>`可以很多，每個區塊都可以有header，甚至不一定在最上面
      `<header>`不能放在其它的header或footer裡面。
    `<section>`整個page裡面的一部分，例如：這一頁裡面有「關於我」、「關於網站」、「聯絡我」，這些都是section。
    `<article>`獨立的一篇內容，和這頁其他地方無關，例如：好幾篇部落個文章，每一篇都是一個article。
    link連外時 target="_blank"
    rel有三種選項：
        noopener：目標網站無法用javascript操控我們的網站，要用
        noreferrer：目標網站不知到從我們網站推薦過去的，建議一般情況不要用，但是在target_blank的情況下還是有編譯器通常還是會因安全性問題要求要用。
        nofollow：目標網站無法獲得我們SEO分數上的權重，像youtube和facebook就會讓他們外連的連結加上nofollow，可用可不用。

## accessibility

1. role：
   1. 用在非標準tag，例如用div做出來的button。
   2. 標準的tag不用加role，因為本身就可以被唸出來。
   3. [role的種類](https://github.com/jsx-eslint/eslint-plugin-jsx-a11y/blob/master/docs/rules/no-noninteractive-element-interactions.md)分為non-interactive的和no-noninteractive的。
2. aria：Accessible Rich Internet Application
   1. 通常用在描述具體的**狀態**或名稱，要加上對應的role或tag一起用，狀態可能要配合state動態更新。
   2. `aria-checked`在說明這個div做出來的checked box是不是被checked了。
   3. `aria-label`在說明這個沒有label的input，要用什麼label說明給disability的人聽。
   4. `aria-labelledby`裡面要放有label說明的那個element的id。
3. [cheat sheet](https://www.digitala11y.com/wai-aria-1-1-cheat-sheet/)
4. 我猜有互動性的element才有role的必要
5. [list of role](https://developer.mozilla.org/en-US/docs/Web/Accessibility/ARIA/ARIA_Techniques#roles)