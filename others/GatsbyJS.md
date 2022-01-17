# GatsbyJS

## 概念

目前看過gatsby最完整的文章
<https://ithelp.ithome.com.tw/articles/10237897>

gatsby的生命週期
<https://hypergrowths.com/software-engineering/front-end-dev/13442/topic-99365668/>

1. 不需建立後端和資料庫。
2. 用headless CMS(content manage system)，速度可以飛快，不像wordpress，要顯示標頭需載入所有內容，速度很慢。
3. 用react建立的，也是速度飛快的另一個原因。
4. 一種自動產生靜態網站的library，內部用上react、react route、graphql。
5. 靜態網站的意思是slug早已經確定，每個網址對應相同的內容，就算react app內容看起來好像很動態，但是還是早就用javascript寫好固定不變，會動只是javascript在瀏覽器裡面render，並沒有向伺服器request，伺服器也不需產生網頁，所以這叫做靜態網站。最古老的html是靜態網站，後來流行serverside render的動態網站，現在因為client side render又流行起來，所以靜態網站又開始流行了，具有速度快、便宜、安全性高的優點。通常現代網站會善用靜態和動態網站的優點混合而成。<https://ithelp.ithome.com.tw/articles/10253329>
6. 動態網站相同的slug可能出現不同內容，特別是不同時間可能看到不同的內容，例如相同搜尋頁面網址的內容，明天可能排序不同內容就不一樣，內容是網址request之後，server才render給browser。
7. gatsby的ssr是在server就把javascript render好丟給browser，browser就不用再render了，這樣速度較快。
8. 其他類似的library：
   1. Jekyll：是用ruby寫成的。
   2. hugo：是用Go寫成的。
9. 作法：
   1. 設定一個樣板，裡面要設定graphql，把markdown的matadata轉成props，以用在樣板裡面
   2. 在gatsby-node.js用graphql把markdown找出來，並一一轉成html，傳給gatsby
   3. 在index的地方，一樣設定graphql的搜尋條件，讓它變成props，以用在react版面設置用

## 設定

第一次用gatsby：
sudo npm install -g gatsby-cli

新建project
gatsby new gatsby-blog

啟動
gatsby develop

vscode material theme

要用一種取代css的style套件的話：
    npm install --save gatsby-plugin-sass

    config裡加上gatsby-plugin-sass
    取消css的import

    import scss

## markdown用的

參考：
<https://www.youtube.com/watch?v=b2H7fWhQcdE&list=PLLnpHn493BHHfoINKLELxDch3uJlSapxg>

### plugin

npm install --save gatsby-transformer-remark

先確定有兩個plugin，關於filesystem和mardown轉html的
弄樣板的post.js檔，匯入graphql從gatsby-node.js找到要呈現的html，然後把html依照自己的格式畫出來。
弄連接的gatsby-node.js，用graphql和allMarkdownRemark找到所有的markdown檔，然後用createPages把markdown轉成html，再灌入post.js檔(template)裡面

    filesystem
        如果dependencies裡面沒有gatsby-source-filesystem
        npm install --save gatsby-source-filesystem

        config pugins
        {
            resolve:'gatsby-source-filesystem',
            options:{
                path:`{__dirname}/src/pages`,
                name:'pages'
            }
        }

    把markdown轉成graphql
        gatsby-transformer-remark
        放進pugins

### markdown材料

    開一個資料夾在pages裡，裡面可以放markdown檔
        markdown檔裡面要放：
        ---
        slug: '/first-post'
        title: 'first-blog post'
        ---

        **非常重要的一點：其中冒號後面要有空格，否則graphql會找不到*
        slug原意是蛞蝓，這邊是網址列要呈現的路徑的意思

### template

    開個資料夾templates在src，取名post.js
        import react和react-helmet
        post.js會export Template(會是一段jsx)和postQuery(會是graphql抓回來的markdown資料)
        export default function Template({data}){return(jsx)}
        //裡面的data是一個object，要用來注入graphql的查詢結果。
            const {markdownRemark:post}=data;
            //const post=data.markdownRemark;
            //以上兩行程式是一樣的，其實就是destructuring

        data是redux的store傳進來的props.data

        以下就是jsx的語法，可以弄成自己喜愛的樣式，裡面分別是title和markdown內容
        return(
            <div>
                {post.frontmatter.title}
                <div dangerouslySetInnerHTML={{__html:post.html}} />
            </div>
        )

        下面是graphql resources的語法，用來把抓到的markdown設定slug和title

        export const postQuery = graphql `
            query ($slug:String!){
                markdownRemark(frontmatter:{slug:{eq: $slug}}){
                        html
                        frontmatter{
                            slug
                            title
                        }
                }
            }    
        `

        graphql的語法都是用兩個back tick包住
        query是一個function接受$slug:String!，這個參數是slug。
        query後的function名稱要不要都可以
        markdownRemark用來找markdown檔
        條件是frontmatter:{}
        $slug代表variable
        Strying是type，驚嘆號代表required
        查詢條件就是：slug:等於傳進來的$slug
        graphql的語法html不能有逗號

### markdown轉html

開檔案gatsby-node.js
    呼叫path函式庫，require path
    自動產生pages，注意這裡是複數createPages
        exports.createPages = (async function)
    這個async function就算不是async function也可以跑
    裡面是
        ({actions,graphql,reporter})=>{
            叫出actions裡面的createPage，注意這裡是單數createPage
            載入template
            graphql查到所有的mardown
            把所有的markdown轉成html資料
        }

    因為gatsby有用redux來管理state，actions讓我們可以用gatsby action
    reporter是可以用來報錯的，沒放也沒關係

    const {createPage} = actions;
    把建立新page的method呼叫出來

    載入template是用path.resolve把template帶給變數postTemplate

    先用graphql和allMarkdownRemark找到markdown，再用createPage產生html
    const result = await graphql(`
        {
            allMarkdownRemark{
                edges{
                    node{
                        html
                        id
                        frontmatter {
                            slug
                            title
                        }
                    }
                }
            }
        }
    `)
    
    if(res.errors){
        reporter.panicOnBuild(`Error while running GraphQL query.`)
        return
    }

    result.data.allMarkdownRemark.edges.forEach(({node})=>{
        createPage({
            path:node.frontmatter.slug,
            component:postTemplate,
            context:{
                slug: node.frontmatter.slug,
            },
        })
    })
    

    allMarkdownRemark()是gatsby plugin的內建函數，小括號裡面可以有其他參數設定，例如排列等等，這裡只是要把markdown全部叫出來，所以就沒放參數，也不用放小括號。
    edges是一個array，代表每一個markdown檔
        這邊的edges很容易弄錯，每一個edges是一個element，不是node，每一個edges裡面都有一個node
    forEach才開始真得建立每一個page
    slug是得到的node.frontmatter.slug
    component:是這一頁定義的postTemplate
    要有context，不然也會出錯，這是markdown裡記錄的mata data

### 作自動的markdown的index

把markdown的index放在index.js裡
    去graphql看看query的條件是什麼，複製到index.js裡，並且取名和template做個區別
        export const pageQuery = graphql `
            query IndexQuery {
                allMarkdownRemark (limit:10){
                edges {
                    node {
                    frontmatter {
                        slug
                        title
                    }
                    id
                    }
                }
                }
            }
        `
    加上了限制查詢10筆
    **這個查詢結果會變成props**

用chrome的react工具找出index的props有什麼
會看到這次查詢的結果
用react把查到的slug變成連結的component
用map把每一個render出來
    {data.allMarkdownRemark.edges.map((element) => {
      return(
        <div>
          <a href={element.node.frontmatter.slug}>{element.node.frontmatter.slug}</a>
        </div>
      )   
    })}

### 讓index自動排列

在markdown檔裡加上`date: "2020-10-23"`
在gatsby-node.js裡的graphql加上date
在template裡的graphql加上`date(formatString: "MMMM DD, YYYY")`
在index.js裡的graphql加上搜尋條件：`sort: {order:ASC,fields:[frontmatter___date]}`
如果要反向排列，把order改成DESC

### 讓草稿不要顯示

在markdown裡加上`published: true`或`published: false`
true和false不是字串，因為之後要給graphql判斷篩選用
在gatsby-node.js裡的graphql加上published
在template裡的graphql加上published
在index.js裡的graphql加上filter:{frontmatter:{published:{eq:true}}}
符合的才會被查到，不符合的就不會在結果中，條件是frontmatter裡的published如果是true，才會被查到

## 其他

react helmet：
    用來動態產生網頁的meta data，用來優化SEO，否則整個app只會有一個meta data。
    Helmet不要包住Helmet，很容易出錯。可以像兄弟一樣，平行的放很多個，就不容易出錯了。

SEO
<https://www.gatsbyjs.com/docs/add-seo-component/>

SEO排名主要和第一次load速度有關
    這可以加速一點點<https://www.gatsbyjs.com/plugins/gatsby-plugin-preload-fonts/>

google searchable console
<https://search.google.com/search-console/welcome>


大約有三種：
meta、og、JSON-LD
<https://yakimhsu.com/project/project_w6_HTML_SEO.html>
<https://ysnweb.net/2020/01/seo-guide/html-meta-tag-seo/>
<https://www.seoseo.com.tw/article_detail_597.html>

general meta:
<https://www.w3schools.com/tags/tag_meta.asp>

og:
    讓fb等社群產生預覽的meta data
    <https://ogp.me/>
    預覽看看：<https://developers.facebook.com/tools/debug/>
    image的網址用graphql拿到
    `fluid(maxWidth: 1380) {
        src
    }`
    或
    `fixed(width:1024){
        src
    }`
    og:url
        要給完整的網址，而不是只有slug

JSON-LD
    google要的meta data
    測試：<https://search.google.com/test/rich-results?utm_campaign=sdtt&utm_medium=message>
    文章類：
    <https://developers.google.com/search/docs/data-types/article>
    logo：
    <https://developers.google.com/search/docs/data-types/logo>

    其他還有：書、課程、how to、當地商家、Movie、評論
    web app類：
    <https://www.darrenlester.com/blog/json-ld-structured-data-for-web-applications>
    <https://schema.org/WebApplication>

`<meta property="al:ios:url" content="partnerapp://product/123" />`
是讓這網頁可以讓使用者打開app用的

example：
    其他meta data可以參考source
    imdb的meta data
        <https://www.imdb.com/title/tt0117500/>
    medium的tag頁
        <https://medium.com/tag/books>



graphql：
<https://ithelp.ithome.com.tw/articles/10200678>
    - 用在gatsby-node.js或是template的時候：
      - 用單純的graphql
      - 寫在component外
      - 我猜這是因為這是給node.js用的，語法較舊
    - 用在none-page component的時候，例如layout component：
      - 用useStaticQuery(graphql`xxxx`)
      - 寫在component裡面，return外面
建立graphql新的資料：
    可以參考看看<https://ithelp.ithome.com.tw/articles/10246499>

gatsby-remark-contentful-images
contentful和netlify：
    <https://www.youtube.com/watch?v=PzGT1mZN7FE>
hambergur menu：
    <https://www.youtube.com/watch?v=KA_tW7zK04w&list=PL1FIFZraE9UF9D_-MPgd-FykyjV3WKrrn&index=1>
route：
    router的endpoint和export的名稱無關，和.js的檔名有關
用netlify後端的註冊功能：
    <https://docs.netlify.com/forms/setup/>

netlify：
    建立網址：
        gatsby-node.js在建立網址的時候
            - 最好為/結尾，因為netlify放靜態網站時，他會把所有靜態網站的網址導向/，才又被gatsby導向原網址，一開始就用/結尾的網址，就不會被重新導向，速度快很多。
            - 也最好.toLowerCase()，因為netlify放靜態網站時，他會把所有靜態網站的網址導向小寫網址，才又被gatsby導向原來的網址。
              - 可能可以修改netlify的設定，用gatsby-node.js的onPostBuild去更動Netlify_redirects <https://hypergrowths.com/software-engineering/front-end-dev/13442/topic-99365668/>
            - context:的slug不要toLowerCase，因為會造成graphQL query不到大寫的slug。
            - netlify上面公開的檔資料夾是`public/`，我們本地則是`public`，netlify原則上會自己改，但有時仍會出錯，所以遠端改成和本地一樣比較保險。
            - 檔名：其實是和git hub關係比較大
                1. 如果有改大小寫要非常小心，因為mac os是case insensitive，所以git不會記錄檔名大、小寫的轉換，但是卻會記錄code裡面的檔名有大、小寫轉換檔名，所以會出現本地develop不會出錯，但是遠端的git hub和netlify會出錯的狀況。
                2. 有一種解決方式**不要用**：是在本地的打上git指令：`git config --local core.ignorecase false`，就會記錄本地的檔名大、小寫改變了。這種設定不要用在case-insensitive的file system上，因為當你把一個file.txt改成File.txt的時候，`git status`只會看到File.txt增加，不會看到file.txt消失。
                3. 真正的解法針對特定檔案：
                   1. `git mv -f yOuRfIlEnAmE yourfilename`。
                   2. 另一個解法：
                      1. 把File.txt改名成temp.txt
                      2. `git add .`
                      3. 把temp.txt改名成file.txt
                      4. `git add .`
                   3. 可以參考：<https://stackoverflow.com/questions/17683458/how-do-i-commit-case-sensitive-only-filename-changes-in-git>
    Domain：
        官方推薦作法：
            external dns：
                把a record設定apex domain導向netlify的published的static ip
                把www的cname導向netlify的primary domain
            優點：
                netlify的primary domain收到 www開頭的domain，可以利用netlify的cdn加快速度，而且避免DDos攻擊。
                而apex domain則在第一次無法享受netlify的cdn，但是之後就會專成www的網址，而有CDN的優點。
            用語：
                <https://docs.netlify.com/domains-https/custom-domains/#definitions>
                custom domain我們買的網址
                primary domain指的是我們主要的domain導向netlify
                domain alias是指我們其他指向netlify的網址
                netlify subdomain指的是netlify的app的domain
        gandi的作法netlify是同意的：
            CNAME把www的subdomain丟給netlify
            用alias把apex doman丟給netlify
            最重要的事情是，網址設定最後要加句點
        custom domain不管gandi端或netlify端都要設定，設定完其實幾分鐘就生效了
            設定成功與否可以到<https://dnspropagation.net/>查
            設定完之後要到netlify驗證，驗證完才會產生SSL和TLS憑證
    netlify不能跑：
        - 如果仍然deploy錯誤，去改一下environment variable，改一下node版本啦，ruby版本啦，改成和本地端一樣，可能就可以跑了。
        - 本地端import的順序改一下，也有可能就可以跑了。
        - 也可能是`gatsby-plugin-offline`的問題，可以移除它，然後安裝`gatsby-remove-serviceworker`，以完全移除offline的功能。<https://bleepcoder.com/gatsby/665976872/error-the-result-of-this-staticquery-could-not-be-fetched_26037>
        - 檢查import裡面 from 'xxx.js' 把.js刪掉

## 順序

1. 弄navbar
   1. 自幹navbar
      1. 弄一個desktop.js
      2. 把desktop.js放進header.js
      3. media query讓小螢幕時desktop消失(display:none)
      4. 弄一個mobile.js
      5. 弄出hamburger
      6. 點hamburger時，hambergur會變成x，nav會出現，再點一下，nav會消失，hambergur會出現
      7. 把mobile放進header.js，讓小螢幕時mobile出現，大螢幕時mobile消失
   2. bulma：
      1. 抓navbar過來貼上。
      2. 把anchor換成Link的tag。
      3. 用tackyons讓navbar在desktop上面會縮小。
      4. 用sass設定bulma變數，讓navbar顏色改變。
      5. 設完之後，用`npm run css-build`，就可以產生一個mystyles.css，讓人複製到gatsby裡面。
2. 配色灰階+藍+橘
3. 弄contentful
4. 弄template
5. 弄netlify
   1. 註冊netlify
   2. 選從github deploy
   3. 設定env
   4. 改domain name
6. 弄文章頁和關於頁
7. 弄markdown youtube plugin
8. 弄coment/like/share plugin
9. 弄algolia搜尋
10. 弄mailchimp訂閱
   <https://hollypryce.com/mailchimp-rss/> 
11. rss plugin：連接mailchimp
12. 弄seo、helmet
13. 弄google analytic
14. 要做site map
15. blog nav設計：
    1. 分類-tags：infinity scroll、按數量排列
    2. 日期
    3. 搜尋
        1. 站內
        2. google



### 不規則切版

將版面不規則切版
    css clip path maker
    <https://bennettfeely.com/clippy/>

    重點有二：
        裁切的部份是要的部份，會顯示的部份。
        其他部份是空的，一般看起來會是白色的，其實是透明的。如果要改變顏色，就是外面再包一個div，那個div設定background的顏色。

    更複雜的，例如曲線切版，只能套入svg的path來切：
    <https://www.sarasoueidan.com/blog/css-svg-clipping/>
    <http://meyerweb.com/eric/thoughts/2017/02/24/scaling-svg-clipping-paths-for-css-use/>
    <https://stackoverflow.com/questions/28311741/responsive-clip-path-with-inline-svg>
        要記住這種方法有一個現象，假如高比寬會變動很大，svg的曲度也會跟著變動很大。
            如果內容寬高比例變化不大，可以直接內容去clip出要的部份。
            因為mobile和desktop的寬高比例很大，所以要設定@media query，讓兩種clip出不同的path。
            如果內容牽涉互動，例如會展開，展開的曲度會變很大，這時候就不能直接clip內容，而是要clip出一個固定高度的膠帶，然後貼在內容的邊緣，這樣內容展開變高時，邊緣曲度就不會變了。
        首先去figma用鋼筆弄一個svg路徑迴路出來
            圖按enter可以調整節點位置
            點任何一個點，再點左上角一個點弧線點的icon，可以調每一個點點bez曲線，然後讓轉角圓滑
            輸出的時候選擇svg
        用vscode看svg裡面的string，拷貝出來，貼到jsx裡面
        把svg的width和height設成0，讓svg圖本身不佔空間。
        把svg裡面的path用一個clipPath包住，其中第2點和第3點是responsive的關鍵
            1. 設一個id給這個clipPath
            2. clipPathUnits的屬性設成objectBoundingBox
            3. 最重要的重點是，transform的屬性設成scale(比例1,比例2)，其中比例1是（1/svg的寬），比例2是(1/svg的高)。翻譯成白話文就是說，如果視窗寬度尺寸是1個pixel的話，那path就是圖形數據x(1/svg的寬)個pixel，高度亦然。
            4. 原來jsx裡的svg會佔一個顯示空間，把尺寸設成0，不會影響clip的效果。
        css裡面去設定一個class，屬性是clip-path:url(#那個clipPath的id)
        到要clip的div裡面去設定這個class，就可以了
        裁掉的底色不要是白色的話，有幾個作法：
            在外面用一個div包起來設其他底色，不過會有個問題，和下面的div之間，在某些比例下會出現一條白色細線。
            改良的作法是本身的z-index設成1，下面的div設transform: translate(0, -4em);，下面的div的上面就會疊在下面。

    邊界會看到白線，可以用以下兩種方式解決：
        把margin設成負的。
        或是把translate設成負的。
    
    如果把瀏覽器拉寬，原來的clip path兩邊邊緣的1px線會被拉大而顯現出白邊，解決方式是：
        用css把這一個膠帶的寬度設成calc(100%+4px)，margin-left設成-2px，這樣超寬螢幕的白邊露餡就會不見了。

讓文繞圖的工具：
    float沒有center可以用，float一定要用，shape-outside才能起作用。
    <https://olivierforget.net/css-shape-outside/>
    程式碼範例：

    ```css
        .image-hero-about{
        content: " ";
        float: right;
        background-size: 100% 100%;
        width: 150px;
        height: 150px;
        margin: 5em -1.5em 0 0;
        shape-outside: content-box circle( 50% at 50% 50% );
        background:url(xxx);
        }
    ```

    結構是
        一個爸爸：把大家裝起來
        一個div：裡面可以背景放相片，裝上這個css
        div後面其他的內容：會繞過前面div
    上面提到的工具抓到的css碼有::before，主要是用來給後面其他內容的，自己要用在前面的div上的話，要把::before刪掉。
    margin是用來控制相片的位置。
    width和height是相片的大小。

### tags系統

contentful的tag製作部份，請參考contentful章節

#### node.js

graphql可以一次query很多東西，所以可以在後面加上tag的query
query出slug和tagName就夠了
每一個tag去createPage新的tag page
slug換小寫的原因是讓netlify不用再轉址，速度較快
component要放template的檔案位置
context是要把slug傳到tag page去

#### 標籤template

依文章多的tag排列到少
    因為文章的多寡太深層了，無法用graphql依數量排列，正確的作法其實是要在gatsby-node.js裡面去建立新的tag資料，如此才能依文章多寡進行排列，但還沒時間研究，所以就先用javascript的sort處理，但是要小心，如果文章很多的話，會影響網頁效能。
    JS sort的原理是兩個相減比較，return為true的話，兩個element就會交換。
所有標籤依照數量排列
    先依照數量sort之後，再map把element回傳回來
    要讓tags可以橫向scroll，要加上nowrap和overflow-x-auto，才不會讓內容凸出去，超過100%。

冰火指數
    gatsby-node.js
        因為已經確定要做出0~9共10篇page，不需要query，直接產生10篇網址就好。
        透過context，把每一個page裡面需要query的資料，也就是該頁和前後1頁的數字也傳過去給page.
    template
        query冰火指數和前後各一個冰火指數的文章資料回來
        依日期排列，把文章排列出來
    slider
        用html的input的type選range
        css：
            -webkit-appearance設成none
                如果要在inline style裡設要用：`WebkitAppearance:none`，其中none要用back tick包住
                上面這種寫法適用於大部分的inline style裡面
            寬高和一般div設法都一樣
            按鈕的css是在class後面加上`::-webkit-slider-thumb`
            按鈕被按的時候，按鈕的css是class後面加上`:active::-webkit-slider-thumb`
        state：
            用onChange去改fireNumberState，改完的值不要直接傳給navigate，因為會造成一次只能前進一格，改完的值只能回傳input的value，改變input按鈕顯示的位置。
            onChange裡面的e.target.value用console.log是看不到的，因為react 17以前這種持續一段拉動的state會放在event pool裡面，去console.log裡看只會看到null。如果要看到的話，加上e.persist()就看得到了。
            navigate放在onMouseup和onTouchEnd就可以解決太早變換頁面的問題了。
    每篇文章都顏色表示
        把文章的冰火指數傳到要顯示顏色的component裡面
        依照比率算出顏色，放到inline style裡
        依冰火指數決定icon種類
    放dropdown menu裡面

## 字型

常用好看的字型：
    montserrat
    Nunito
    MS PGothic
    Mukta
    sans-serif
    Noto Sans 思源黑體 (正式版)

中文字型：
    放到.css檔中
        `@import url("https://fonts.googleapis.com/css?family=Noto+Sans+TC&display=swap")`
        font-family: 'Noto Sans TC',sans-serif;
    Noto Sans 思源宋體 (正式版)
        方法1:
            @import url("https://fonts.googleapis.com/css?family=Noto+Serif+TC&amp;display=swap");
            font-family: 'Noto Serif TC', serif;
            字型粗度只有fw1 fw3 fw4 fw5 fw7 fw9可以用
        方法2:用plug-in，直接灌在gatsby裡面，速度才快
            <https://www.npmjs.com/package/@fontsource/noto-sans-tc>
            <https://www.gatsbyjs.com/docs/how-to/styling/using-web-fonts/>

font awesome：
    icon很多，不用attribute
    <https://fontawesome.com/how-to-use/on-the-web/using-with/react>
    <https://www.youtube.com/watch?v=IiPQYQT2-wg>
    <https://www.gatsbyjs.com/plugins/gatsby-plugin-fontawesome-css/>

品牌icons
    <https://simpleicons.org>
    `import { faGithub } from '@fortawesome/free-brands-svg-icons'`

markdown裡要塞youtube
    <https://docs.embed.ly/docs/cards>

graphql：
page query和staticQuery的不同
    page query參考post.js：
        可以有變數進去query，query要export出來。
        資料在function的props的data裡面。
    staticQuery參考image.js
        不能放變數進去query。
        可以把query到的東西直接放到一個變數裡，直接用。

### mailchimp

專有名詞：
    audience：一組名單，例如架了不同網站，就設不同的audience
    contacts：一個audience裡的所有聯絡人
    campaign：email的意思
操作介面較傳統，視窗要拉寬一點點，否則有些功能不會顯示，例如campaingn的選取方框就不會顯示。
找到endpoint
    audience=>signup forms=>embedded forms=>程式碼的form action裡到網址
到gatsby-config裡面去設定
    把`GATSBY_MAILCHIMP_ENDPOINT=網誌`填入.env.development

    ```javascript
        {
            resolve: 'gatsby-plugin-mailchimp',
            options: {
            endpoint: process.env.GATSBY_MAILCHIMP_ENDPOINT,
            timeout: 3500,
            },
        },
    ```
做出表格外觀
用react去設定出state
`import addToMailchimp from 'gatsby-plugin-mailchimp';`
用addToMailchimp(email,物件)把資料送過去，把結果抓回來
物件裡是其他欄位，欄位名稱要去以下方設定
    audience=>signup forms=>settings=>audience fields and merge tags
    欄位名稱就會是物件的key
抓回來的結果會是一個物件，弄一個邏輯：
    空物件的時候，什麼都不顯示(null)，物件顯示成功的時候，就render成功訊息，物件顯示失敗的時候，就render失敗訊息。
改mailchimp的確認信
    audience=>signup forms=>form builder=>opt-in confirmation builder...等等。
連接RSS
    <https://mailchimp.com/help/share-your-blog-posts-with-mailchimp/>
    create => email => automated => share blog update => 輸入campaign名字
    <https://validator.w3.org/feed/>
    測試結果正不正確：
    <https://validator.w3.org/feed/>
    因為有中文網址，所以需要先對網址encoded
    <https://www.w3schools.com/jsref/jsref_encodeuri.asp>
    圖片相對網址轉絕對網址：
    <https://markshust.com/2020/06/25/fixing-images-in-gatsby-rss-feeds/>

## markdown

markdown裡面options的plugin順序是有差的：
    1. 例如"gatsby-remark-embed-video"必須在gatsby-remark-responsive-iframe和gatsby-remark-images-contentful之前，才能正確的responsive。

本機和contentful的不同：
    1. gatsby-config.js裡面source不同：
       1. 本機：gatsby-source-filesystem
       2. contentful：gatsby-source-contentful
    2. image處理不同：
       1. 本機：
          1. gatsby-remark-images
          2. 和gatsby的一般image處理方式沒什麼不同。
          3. 相片位置自己放資料夾。
       2. contentful：
          1. gatsby-remark-images-contentful
          2. 不用做各種size的處理了，而是直接抓contentful上適合的size回來處理
          3. 相片是統一由contentful media處理，放在同一個地方。
    3. 欄位不同：
       1. 本機：用markdown的frontmatter欄位來設定一些meta data，就是寫在markdown的最前頭。
       2. contentful：用content modal來設定一些meta data。
其他本機和contentful沒什麼不同，其他remark轉換相關的plugin設定都放在gatsby-transformer-remark的plugins裡面。

### gatsby-remark-embed-video

要嵌入youtube的話用這個plugin，雖然可以設定大小和iframe外面div的class，但是完全不responsive,要另外灌"gatsby-remark-responsive-iframe"，就可以responsive了。

### gatsby-remark-images

抓回來的圖轉成html後的格式很複雜，要一層一層打開，才看得到img

    ```html
        <p>
            <a href="圖片網址">
                <span>
                    <img />
                </span>
            </a>
        <p>
    ```

如果沒有看到圖，有可能p的class設成flex，種種原因讓裡面的盒子size不見了，把size撐開，就看得到了。

### gatsby-remark-classes

可以用來把class傳進markdown裡面
    從gatsby-config.js裡面傳給markdown轉換器
    其中css選擇的語法和名稱參考：
        <https://github.com/syntax-tree/unist-util-select#support>
    選項參考：
        <https://github.com/syntax-tree/mdast#contents>
    注意如果有重複設定的部份會和一般css的規則一樣，是看權重來決定是否顯示。例如p的class有mv4，在li裡面的p加上mv2，這時候等於li裡面的p同時有mv4和mv2，可能只會顯示mv4。解決方法是自己的css表弄個新的class把mv2的屬性加上去，把這個class放進li裡面的p，就能顯示自訂的class了，因為自訂的class權重比tachyons的來的大。

### gatsby-remark-prismjs

install
    `npm install gatsby-transformer-remark gatsby-remark-prismjs prismjs`
    設定copy進gatsby-config.js：
    <https://www.gatsbyjs.com/plugins/gatsby-remark-prismjs/>
在gatsby-browser.js裡貼上，之所以在gatsby-browser裡貼上，應該是因為，這是在react app執行的時候才匯入
    `require("prismjs/themes/prism-solarizedlight.css")`
可以去library中，看有什麼theme，都可以換掉
由於prismjs會超出div，為了控制overflow和做一些customize style，**可以在自己的css檔案裡面**做以下的設定：
    程式碼超出div的部份可以出現scroll

    ```css
        .gatsby-highlight {
        border-radius: 1em;
        overflow: auto;
        }
    ```
    程式碼裡面的背景顏色可以改

    ```css
        .gatsby-highlight pre[class*="language-"] {
            background: #333333;
            margin-right:0;
            padding: 0;
            overflow: initial;
            float: left; /* 1 */
            min-width: 100%; /* 2 */
        }
    ```

    單行程式碼裡面的背景和強制斷行設定

    ```css
        .language-text{
            background: #999999 !important;
            overflow-wrap: break-word !important;
        }
    ```

markdown的link也會overflow，也可以設css把link打斷

    ```css
        .anchor-word-breaker{
            overflow-wrap: break-word;
        }
    ```

## headless cms

傳統的cms叫做：
    monolithic CMS
    regular CMS
    coupled CMS
傳統CMS
    傳過來的是head+body
    head指的是和view相關的資料，body指的是純內容

headless
    指的是後端傳過來的資料是純資料，沒有view相關的資料
    好處是
        前、後端高度分離，前端可以用自己擅長的框架顯示view
        ios、android都可以用
        內容有一致、完整的輸入介面、不需要管view
        前端不用管後端的技術和穩定性
        wordpress會混入一堆用不到的程式碼需要載入，速度會較慢，headless的方式速度快
wordpress也在考慮加入headless的功能

headless cms有非常多：
<https://jamstack.org/>
直接有api可以接的：
    contentful
可以自己架headless cms：
    很多人推：Strapi
    netlify-cms

### contentful

是一種headless cms

locale：
    注意locale先設為單一locale，以後有需要再擴充
    若要多locale要傳locale資訊給contentful，才收得到response

用contentful做tags系統
    要用reference field來做
        先設一種新的content type
        設一個slug和一個references field
        設新tag的時候，分類名放slug，這個分類的文章都reference到這個references field
    這種系統的好處是寫文章時，不用設一對tags，而tags可以任意選進任何文章，再丟給graphql處理
    補充：
        原來每一個post也要有一個tags的欄位，然後reference到那個tags
        每一個tags或content最好都要有以下欄位，這樣以後修改external name的時候才不會連結不見了，保留一定的彈性
            external name
            internal name
            id
    GraphQL無法深層的排列(sort)，所以在tag page裡面，目前自己去sort，未來如果文章很多，可能會降低效率，這時候要去把tags裡面的文章列表新建立成GaphQL資料，才可以去sort。
export contentful 的資料
    安裝contentful-cli：`sudo npm install -g contentful-cli`
    認證：
        `contentful login`
        從瀏覽器登入取得token
        在terminal貼上token登入
    瀏覽器複製spaceid
    export：
        輸出json檔案：`contentful space export --space-id xxxxxxxx`
        只要輸出內容和圖片可以再加上：`--content-only --download-assets`
    登出：`contentful logout`
import content type 的資料
    <https://www.contentful.com/developers/docs/tutorials/cli/import-and-export/>
    contentful space import --content-model-only content-type.json

### figma

component，可以一次選幾個相同的東西，更動一個屬性時，其他會一起更動
按住shift在object1和object2時，可以讓object2相對object1對齊
autolayout，可以自動調整container的大小
prototype可以讓按鈕連接不同的頁面

vector要進去編輯節點：
    點選vector
    按enter
    shift+click節點是選取節點
    shift+back space是刪除節點，並補回大致的形狀
    back space會導致closed path變open path

開放的path或線轉成close的path的技巧
    可以設定粗度
    然後點...的圖示，可以設定圓形的頭
    然後可以設定outline stroke描邊
    如果要刪除裡面的stroke，用boolean selector的聯集把圖案合併成一體，內部的stroke就會消失。

https://www.youtube.com/watch?v=2jYmxWC11ac&list=PL2adUX6Utt_3OANMSwJ6YBajQlTfAXiPH



### facebook

先去npm install
<https://www.npmjs.com/package/react-facebook>

    ```jsx
        <FacebookProvider appId="XXXXX">
        //包住整個app，用一次就好了
        </FacebookProvider>
    ```
    裡面的appId是去facebook for developers裡面，弄一個app，就有appId可以用了。

    ```jsx
        <Like href={`https://bugdetective.netlify.app/blog/${slug}`} colorScheme="light" showFaces lazy="true" size="large" width="90" button_count />
    ```

    裡面的component的屬性參考：
    https://developers.facebook.com/docs/plugins/like-button/

    facebook會記得這個appId底下這個網址的統計資料

    如果格式大跑掉，有可能是browser連fb太頻繁了，造成fb把browser擋住了，然後會出現一個超大你被擋住的警示。解決辦法是想辦法用一個div圍住他，然後用overflow-hidden把它藏起來。

    要用`<Helmet \>`把`fb:app_id`加入meta data中，如此，才會出現可以管理留言的功能。

### formspree

    主要是要讓別人寄email給你，也可以用express和nodemailer來用
    先去註冊：<https://formspree.io/>，讓他們可以轉寄到你的信箱。
    註冊完去copy react的程式碼回來貼上
        react版本無法用captcha防止機器人亂寄信，如果要用captcha就要用iframe的方式。
    程式碼改成fetch的方式是可行的，主要傳過去的資料是name、email、message的object就好了。
    把監聽enter和onfocus設定好，注意message的地方不要放監聽enter，否則使用者寫信的時候會無法換行。

## 搜尋

有兩個可以用：
    google CSE
    Algolia

### Algolia

    直播gatsby教學：<https://www.youtube.com/watch?v=VSkXyuXzwlc&feature=youtu.be>
    官方react教學<https://www.youtube.com/watch?v=Uc9JAQNP1PE&t=9s>
    queries裡的settings的參數：<https://www.algolia.com/doc/api-reference/api-parameters/>
    分兩大部份：上傳indices和下載搜尋結果
    上傳：
        利用gatsby-config.js裡面的queries上傳，格式是[object,object...]
        每一個object是一種indices，每一個indices裡面的資料，可以讓algolia利用你提供的transformer變出一堆子object，每一個子object都是blog的一個頁面
        不同的資料結構可以創建不同的indices，algolia沒有限制indices的數目
        每個indice會佔用一個搜尋次數，所以越少越節省次數
        可以寫一個algolia-queries.js來export這些queries
            每一個object的結構是
                query：放graphQL抓到的資料
                indexName：放algolia網站上的indices名稱
                transformer：
                    最關鍵。
                    放一個要給algolia的function，這個function在algolia那邊，會把前面query到的東西放進parameter裡面，然後要得到一面網頁對應一個子object。
                    這個object一定要有objectID，可以用contentful的id當成objectID，這樣可以讓algolia認ID認出是不是同一個網頁，減少重複的搜尋比數。
                    其他欄位就是自己想傳什麼給algolia搜尋，就傳什麼上去，只要確定是object的格式就好了。
                    注意不要傳客戶或機密資料上去。
                    slug格式要改變，因為網站的slug和graphQL的slug不同，所以要改成網站的slug再上傳，之後的搜尋結果，才有辦法產生正確的連結。
                settings：
                    attributesToSnippet：搜尋結果要顯示幾個字
                    searchableAttributes：可以搜尋的欄位
    下載搜尋到的資料，用react把它render出來：
    React InstantSearch
        大原則：
            API內建的component，就是大括號引入，function的argument也是大括號來destructure，如此可以讓Algolia傳資料給我們用。
            custom的component，就用API提供的connectXxx()，然後把你設計的jsx用成一個component放進去，Algolia就會把資料傳進去你的jsx給你用，然後再把connectXxx回傳的component匯出來用。
        主要的component有幾個個部份：
            InstantSearch：API內建的，要包住所有component，用來限制特定搜尋
            CustomSearchBox：要自己做，裡面要有API內建的connectSearchBox
            SearchResult：要自己做，裡面要有API內建的Index、connectStateResults、Hits、Highlight、Snippet
            useClickOutside：要自己做，用來偵測是不是點到外面了，點到外面就不顯示search
        InstantSearch裡面的index是隨便取一個名字，因為可能整個網站有不同地方的有不同的搜尋，取名不同的index就可以區分，如果整個網站只有一個搜尋，就隨便取一個名字就好了。
        CustomSearchBox：
            connectSearchBox
            用來把自己設計的component和algolia連在一起
            parameter是一個component，這個component裡面要有搜尋的input element
            這個component的arguments：
                refine是一個function，把聽到的event丟給algolia
                currentRefinement是algolia再把event傳回來，可以傳給value，以利顯示
            focus
                API的SearchBox的autoFocus的property沒有用，可能只適合API的，不適合custom的
                直接在custom的component裡面，input element設定autoFocus的property就可以了
        SearchResult：
            Index：
                Index的indexName讓你可以限定搜尋的indice
                多個indices的用法是，用Index把connectStateResults和Hits包起來，所以可以用一個map，把要搜尋的indices都傳進去給Index，這樣所有的indices都會被搜尋到了。
            connectStateResults：
                是用來顯示統計的，例如找到幾筆，花多少時間
                argument：searchResults用來把搜尋出來的筆數傳進來
            Hits：
                API內建
                用來印出結果的component
                hitComponent這個property是用來把自己做的component傳進去
                這個自己做的component
                    會得到一個props，props.hit就是搜尋到的資料，props.hit.title、props.hit.exerpt就是當初做indices做的。
                    props.hit.slug讓你可以做一個連結，Link to你要去的網頁。
                    裡面主要還要放API內建的Highlight和Snippet
                    上面提到的props可以傳進去Highlight或Snippet裡面，讓他變不同的style。
                    Highlight和Snippet
                        不同的地方在於Highlight是顯示完整的attribute，Snippet只顯示附近的摘要。
                        兩個都必須指定attribute，目前還搞不定多個attribute的格式，所以要能顯示多個被搜尋的attribute，就只好放多個component上去。
    ClearRefinements是一個按鈕，可以用來清除過濾條件。
    Configure裡面的hitsPerPage是用來控制一頁顯示多少結果。
    Pagination用來顯示頁數

## 其他不重要的

gatsby-plugin-excerpts
    可以引用時head縮小一個size
    不太需要

spotify currently playing track

建立日期的頁面archive
<https://stackoverflow.com/questions/54473828/how-to-query-date-range-in-gatsby-graphql>

佈景發表
    <https://ithelp.ithome.com.tw/articles/10250581>

登入、會員相關
    1. gatsby-plugin-create-client-paths
    2. <https://ithelp.ithome.com.tw/articles/10251554>
       1. 從localstorage去撈資料，來判斷是不是已經登入，未登入就導去登入頁面，已登入就導去登入的path
       2. 利用`<Router> <C1 path="p1"/> <C2 path="p2" /> </>`來動態控制導去哪個頁面。
       3. 用一個`<PrivateRoute>`的HOC，判斷是不是已登入。這個HOC要傳profile的component進去，或是可以傳任何已登入才能看的component進去。HOC裡面如果判斷已登入已登入的話，就可以把這個component return出來，也就會被render出來了。如果未登入，就return null，什麼都不顯示。
    3. <https://www.gatsbyjs.com/docs/how-to/routing/client-only-routes-and-user-authentication/>

gatsby-transformer-sqip

## 其他還沒弄的

網購
    snipcart
    shopify button
    stripe
        <https://ithelp.ithome.com.tw/articles/10252625>
    綠界、藍新、紅陽
    一個小時建立shopify購物網站
        <https://www.twitch.tv/videos/1012097620>

系列文章

修rss.xml
    Email還沒寄出去過
    重複兩張圖

algolia
    pagination
    agolia搜尋量超大，要減少搜尋量
    <https://www.algolia.com/doc/api-reference/api-methods/multiple-queries/>
    減少indices
    減少觸發時機
    用stopIfEnoughMatches，讓搜尋滿一頁就停止。

Error boundry
    包住restoftags
    timetoread

push notifications
會用到netlify的function功能<https://medium.com/@alfrekjv/implementing-a-web-push-notification-system-with-subscribers-gatsby-js-and-netlify-680f49639fe2>
<https://spectrum.chat/gatsby-js/general/web-push-notifications-with-gatsby~5e6abe4f-e015-4966-8e6b-0f5a6b53fa3c>
<https://blog.agney.dev/one-signal-on-gatsby/#4-site-setup>

gatsby-plugin-offline

檢查
    codepen plugin
    gist plugin
        可以截取github上完整專案的一小段程式碼做分享
        <https://www.gatsbyjs.com/plugins/gatsby-remark-embed-gist/>

 新排序建立新的graphql source資料
 讓tag能用graphql排序，增加效率

web push notification
<https://medium.com/nick-%E5%B7%A5%E7%A8%8B%E5%B8%AB%E5%AD%B8%E7%BF%92%E8%A8%98/%E4%BD%BF%E7%94%A8-firebase-%E5%BF%AB%E9%80%9F%E5%BB%BA%E7%AB%8B%E7%B6%B2%E9%A0%81%E6%8E%A8%E6%92%AD%E6%9C%8D%E5%8B%99-web-push-notifications-service-3e7b0d0c5ac6>

弄google analytic
要做site map

netlify改為bdr
    gandi 的domain改指向bdr
    meta data, canonical url, 所有bugdetective都要修改
    fb share預覽
    contentful
    mailchimp
    google analytic

目前在找工作，最理想的公司依序是
1.用react正在開發自己產品的公司
2.用react前端和node後端開發自己產品的公司
3.可以用react開發的其他任何公司
大家如果有認識的，幫我介紹一下吧。

了解更多=>自傳

關於網站：
    這個網站不是用wordpress架出來的
    是用現代的靜態網站framework配合多種cloud service
    安全、速度快、維護成本低
    發明溫度標籤

關於我
    原本保護改變世界的人
    跳進來想要自己改變世界

版權政策
git hub MIT 授權
隱私權
著作權

lighthouse 可以測試 seo 分數

做gatsby plug in
<https://dimitr.im/writing-gatsby-plugin>

contentful新版tag功能


測試：
<https://blog.yyisyou.tw/c91309fe/?fbclid=IwAR25hUvqOhmknGfg3zxnguAKCYEOmlHzbF15C0WgI7Qjm5PP4jA3gY0fpl8>

## 還沒整理的筆記

builtime vs runtime，以及global state的問題：<https://www.youtube.com/watch?v=GdsKB6_6pdk>

Link：
    <https://www.gatsbyjs.com/docs/reference/built-in-components/gatsby-link/>
    可以偵測是不是在本頁，如果是的話，可以放入class，讓顏色不同。
    想要可以跳到某個element，頗不容易：
        有plug in可以用：
            <https://www.gatsbyjs.com/plugins/gatsby-plugin-anchor-links/>
        不用plug in：
            先在要跳過去的element加上id
            Link網址加上hash和id
            在目的地主頁的location.hash抓到要去的id
            用useEffect來進行跳轉，否則網頁會被鎖在那個element，無法scroll
            要用'@reach/router'的navigate跳去那個hash，而且replace要設成true，如此才會忘記本頁經過跳轉的history，按上一頁時才會回到原來的Link
                <https://reach.tech/router/api/navigate>

gatsby-config是用來設定讓graphql能找到要的資料用的。

gatsby clean用來清理cache

gatsby-background-image
background-size:cover

pixels
    <https://www.pexels.com/zh-tw/>

contentful用Set收集tag

array.from從set轉回array

Prettier - Code formatter

buid hooks把netlify和contentful接在一起

coffee shop
    <https://youtu.be/oAVhEPey_qA>

買網域
<https://www.ewdna.com/2013/09/godaddy-domain-promo-code.html>
cheapname

bulma
<https://www.youtube.com/watch?v=LBzZLzu2GKo>
21:30 fr
.content可以放blog的內容，純html，例如markdown轉過來的。

navbar:51:42

grid
media
grid-template-areas
grid-row-gap

1:03:50

sass
    <https://www.youtube.com/watch?v=roywYSEPSvc>

### graphQL

<https://www.npmjs.com/package/sift>
$in後面接一個array代表，所有的edges至少是array裡面一個item才會被query出來。
$regex有某個字串的話，會被query出來

### complete react課程筆記

node.js裡
    node代表的是一個檔案
    export.onCreateNode是用來將那個檔案修改
    createFilePath是用來抓一個node的路徑
    createNodeField是用來把一個檔案多加一個field，讓graphQL可以搜尋到

component裡
gatsby的api會自動把export的graphql抓來給那個component使用

node.js裡面跑的是node.js的舊語法，所以graphql(``)要有小括號
component是給瀏覽器跑的，所以可以用es6的新語法graphql``不用小括號

dangerouslySetInnerHtml通常不在react用，但gatsby是靜態網站，所以非常安全。

---

架構：
    node.js的程式碼：
        用來幫人把Json檔案，依照templates寫成一般頁面，用的是comonJS，是在你的電腦上執行一次而已，deploy之後並不執行。
        這就是gatsby的主要功能，和最重要的觀念。
        主要靠的是gatsby-node.js和gatsby-config.js。
            gatsby-node依templates來自動產生多個app。
            gatsby-config來產生graphQL的source，讓靜態網頁的app能抓取資料。
    browser的程式碼：
        用來寫出virtual DOM，讓browser執行。
        一般頁面：放在src/pages裡面，依照檔案路徑進行router，和一般html靜態網站是一樣的。
        templates：放在src/templates裡面，router是由gatsby-node來產生。

免費圖案
<https://undraw.co/illustrations>

### buymeacoffee

相關參考資料
    <https://www.buymeacoffee.com/>
    <https://eshlox.net/2019/11/30/add-buy-me-a-coffee-widget-to-a-gatsbyjs-site>
    <https://stackoverflow.com/questions/64998681/create-buymeacoffee-component-for-gatsby-js>
    <https://betterprogramming.pub/4-ways-of-adding-external-js-files-in-reactjs-823f85de3668>
widget很高雅但是有個缺點：
    要用的話就每一頁都要用，因為他是載入在整份react app的head上，javascript一旦載入，就無法卸除。網路上有提到可以用新的script把舊的取代掉，我試並沒有成功。
    buymeacoffee也不讓人用iframe的方式使用。
    最後就用跳轉的方式放按鈕了。
    後來發現一個辦法，可以在netlify上設定，某些條件的uri，讓netlify自動加上某些header，可以把wedget用這種方式加上post的頁面上。

關掉eslint
<https://flaviocopes.com/how-to-disable-eslint-rule/>

### gdpr cookies

google analytic
    gtag最關鍵的地方在於：
        關閉cookies是：`window['ga-disable-G-4T61R4H6E8'] = true;`
            而且這要放在browser.js裡面
                因為window是瀏覽器的環境之下才會有的instance，gatsby在build的時候，在node環境之下是沒有windows的instance，所以會出現dindows是undefined的errror。
                放在layout裡面，似乎每route一次，就會mount layout一次，所以即使client已經設定同意cookie了，一route就又mount一次而被取消了。
        開啟cookies是：`window['ga-disable-G-4T61R4H6E8'] = false;`
    gtag主要是灌gatsby-plugin-google-gtag
        google的document主要設定都是用`gtag()`，gatsby底下主要都是用`window.gtag()`
react-cookie-consent：
    用來做banner，他會有一個cookie來記錄是不是有點過同意，點過之後，下次就不會再出現了。
    要先將cookie設成沒有，然後client端接受後，才啟動cookie，這才符合GPDR的規定。

gif檔錄製：
<https://www.cockos.com/licecap/?fbclid=IwAR26cL1gXX-NzqIOL1zxqUfC9XbOInXiTCWNP0HJ7I2tq10sOFNfJ3eoEik>

** HOW TO SEND CODE **
Following the guidelines below will help others read your code easily and be able to assist you much quicker
Small Code Snippets
For small code snippets consisting of a couple of lines, encase the code in backticks `
Larger Code
Head over to https://codepen.io/ and create a new pen.
React Code
If you need help with your react code, head on over to https://codesandbox.io/s/new


### 新的gatsby image api

<https://www.gatsbyjs.com/docs/how-to/images-and-media/using-gatsby-plugin-image/>

migration
    gatsby-image => gatsby-plugin-image
    <https://www.gatsbyjs.com/docs/reference/release-notes/image-migration-guide/#codemod-instructions>

`npm install gatsby-plugin-image gatsby-plugin-sharp gatsby-transformer-sharp`

新語法的優點：
    gatsby2.0靜態圖檔需要透過graphql，反而比html和create react app更麻煩，新語法不需要再透過graphql就可以做到，可以直接用API的StaticImage就直接用了，而且仍然擁有gatsby圖片的所有強大功能。
    gatsby2.0靜態圖檔設定兩個地方，option的部份，有些在graphql設定，有些在component設定，有些兩者都要設定，現在簡化成graphql設定就好了。

語法：
    index裡面的card裡面，原本有用到`import Img from 'gatsby-image'`，要改成`import { GatsbyImage } from "gatsby-plugin-image"`
    component的用法：
        原本：`<Img fixed={data.file.childImageSharp.fixed} />`
        改成：`<GatsbyImage image={data.file.childImageSharp.gatsbyImageData} />`
    graphql：
        index裡面的graphql：

        ```graphql
            fixed {
                ...GatsbyImageSharpFixed
            }
        ```

        改成：

        ```graphql
            gatsbyImageData(layout: FIXED)
        ```

        或是：

        ```graphql
            gatsbyImageData(
                width: 200
                placeholder: BLURRED
                formats: [AUTO, WEBP, AVIF]
            )
        ```
    
    GatsbyImage：
        從 contentful 過來的，因為CMS會動態傳適合大小的檔案過來，所以要用 GatsbyImage
        同一個component需要從props傳不同的圖進去，這個component裡面也要用 GatsbyImage
        是透過graphQL裡面的gatsbyImageData傳遞options。
        都是用graphql的enum value直接丟值，都是大寫的，也就是document裡面的Resolver prop value。
        document裡的options只能參考，主要是給sharps用的，CMS傳過來的options可能不一樣。

    StaticImage：
        因為都是從CMS抓圖片回來，而且很常用props在傳圖，所以我自己這個功能很少用，限制很多。
        這個component直接用本地檔案，裡面直接放相對位址，不用再用graphQL了
        直接用遠端檔案也可以用StaticImage
        gatsby會自動產生各種大小，所以遠端檔案如果變了，不會自動立即更新，rebuild後才會更新
        不可以從props裡面傳相片過去
        是透過props設定options
        有些option是吃string，要加上雙引號
        options都是用string不是enum，放在prop裡面
        document裡面Component prop value就是給StaticImage用的
    
    getImage()：
        不一定要用這種語法，可以.gatsbyImageData就拿得到圖片了，但是這種語法的好處是比較不會有null的錯誤。
        先`import { getImage } from 'gatsby-plugin-image'`
        如果收到null，不會出錯(null-safe)，會送出undefined
        回傳的東西等同：.childImageSharp?.gatsbyImageData，所以兩種寫法是一樣的：
            `const image = getImage(data.avatar)`
            `const image = data?.avatar?.childImageSharp?.gatsbyImageData`
        用來把graphql source到的資料，傳給component。

graqphql裡面的type：
    失效：
        maxWidth
        maxHeight
    fluid：
        fullWidth：
            適合hero，隨螢幕大小變化
            可以直接在這裡設定breakpoints
        constrained：
            會隨著container大小變化
            可以設定width或是height
    aspectRatio：
        可以設定比例
        可以用數字表示，例如：1.5
        也可以用分數表示，例如：`aspectRatio={16/9}`
    formats：
        沒設定就是原本的JPG,PNG等
        設定的話要放array進去[AUTO, WEBP]，會用WebP存檔
            這裡的AUTO指的是和源來一樣

要拿src來seo的話：

    ```
    import { getSrc } from "gatsby-plugin-image"

    const HomePage = ({ data }) => {
        const imagePath = getSrc(data.file)
        return (
            <>
                <SEO imageSrc={imagePath} />
            </>
        )
    }
    ```

## 開發環境

1. 有些功能，例如pwas要build，甚至需要https才能測試，這時建一個開發環境來測會比較方便。
2. https：
   1. let's encrypt
   2. 搭配[cerbot](https://certbot.eff.org/)
