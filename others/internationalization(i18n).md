# react-intl

1. 安裝：`install npm i -S react react-intl`
2. 根據要翻譯的語言，弄成不同的json檔案，放在`./src/lang`裡面，格式注意key、value都要用雙引號。
3. 過程大致上是：
   1. 先把翻譯的JSON檔案匯入App內
   2. 通過一個變數確認瀏覽器預設或使用者的locale
   3. 根據變數把locale和對應locale的翻譯資料的變數放入IntlProvider裡
   4. 把要翻譯的字用FormattedMessage來替代，要把對應JSON的key的id當成FormattedMessage的property。
4. App.js
   1. 先匯入：`import {IntlProvider} from 'react-intl';`
   2. 在index的App外，或者App檔的最外層包上

        ```jsx
            <IntlProvider locale={規定的locale碼} messages={對應locale的翻譯JSON檔}>
                <App />
            </IntlProvider>
        ```

5. 要翻譯的地方
    1. 一般用法：
       1. 那一個component裡要先`import {FormattedMessage} from 'react-intl';`
       2. 要翻譯的字換成`<FormattedMessage id='send' defaultMessage="Send URL" />`
       3. 對應語言的json檔案裡`{"send":"Enviar URL"}`
    2. 如果有被DOM包圍的話，用以下的語法

        ```jsx
            <FormattedMessage
                id='credit'
                defaultMessage="App build by <code>Richard Huang</code>"
                values={{
                    code:(text)=>
                    <a  href="https://github.com/yellowful/smartbrain" title="Richard Huang">{text}</a>
                }}
            />
        ```

        - 上面`defaultMessage`裡面的`<code>`會被`<a href...>`給取代，text代表的就是code tage之間的Richard Huang。
        - JSON檔裡的`<code>`也一樣會被`<a href...>`取代掉
    3. 如果有變數要傳進去的話，可以把變數取名，用object傳進去。

        ```jsx
            <FormattedMessage 
                id="entry"
                defaultMessage="Hi {name}, your current entry count is {entries}"
                values={{name:currentUsers.name,entries:currentUsers.entries}}
            />
        ```

    4. 想換行的話，要用一個變數，把`<br />`傳進去

        ```jsx
            <FormattedMessage
                id='instuction'
                defaultMessage="Please input an image URL or upload an image, SmartBrain will tell you who the celebrity in a picture is. {linebreak} Give it a try!"
                values={{linebreak:<br />}}
            />
        ```

6. 瀏覽器預設的第一語言是`navigator.language`，所有預設語言依順序列出是`navigators.language`

7. 如果app很大，結構已經很複雜的話，還可以用一些技巧和工具：
    1. id的取名可以加上小數點
    2. 可以配合`<FormattedTime \>`、`<FormattedDate \>`、`<FormattedNumber \>`、`<FormattedPlural \>`等來設定時間、日期、金錢、單複數等的格式自動設定。IntlProvider裡的locale要設定正確，這些內建自動功能才會正確使用。
8. 多國語言資源：react-intl, react-i18n-redux, react-intl-universal, LinguiJS, i18next
        <https://phrase.com/blog/posts/react-i18n-best-libraries/>
        <https://formatjs.io/docs/getting-started/installation>
9. i18next：
   1. 透過後端來載入翻譯檔，也就是透過http/https來載入翻譯檔。
   2. 是i18next內設會自動偵測http:/domain/locales/en/translation.json，
   3. 如果React app的homepage是有slug的，例如：http:/domain/project，那麼i18next會找不到translation.json，這時候會需要把backend設定path，i18next才會找得到translation.json。
   4. react app因為不能處理router，所以package.json需要設定homepage,如果homepage的設定有slug，react app開啟的home就會有slug。換句話說homepage之所以會有slug，是因為package.json的homepage設定有slug，如果改成沒有slug的網址，react app的homepage就不會有slug的了。
   5. 如果react app的homepage，也就是http:/domain/找不到任何檔案，只有app本身，代表public這個資料架serve錯誤，有可能webpack壞了。解決方式是，把module砍掉，然後npm install.