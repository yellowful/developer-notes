# 2021 mopcon

## jetpack compose

## 你懂javascript也不懂javascript

1. 面試：
   1. hosting
   2. this
   3. prototype
   4. 
2. 重要：
   1. 寫程式會用到
   2. 不知道會寫出bug
   3. 例如：
      1. 非同步的程式
         1. callback不等於非同步
         2. callstack執行完才會執行event loop的東西
         3. 用callback或promise拿`.then()`裡面的資料
      2. type：number跟string
         1. 例如：`[1,5,3,11,7].sort();`是字典序
         2. input收到的數字要先parseInt
         3. 浮點數誤差：解決方式`number.EPSILON`
         4. 數字有範圍大小：Number.MAX_SAFE_INTEGER，解決方式BigInt。
      3. hoisting
         1. js在執行以前就會做一些處理，不是純script一行一行執行。
      4. this
         1. this是怎麼呼叫有關，就是xxx.method()之前的xxx，沒有的話會是widow或是undefined
         2. 解決方式`.bind()`綁定
      5. prototype：
         1. 例如：`(1).toString()`js會找`__prototype__.toString()`也就是`Number.toString()`來用
         2. 知道prototype這件事可以用`Array.prototype.slice.call(arr)`的用法。
3. 不重要：用不到，但有趣，和spec有關的。

## SigNoz

1. dev ops分散式架構愈來愈複雜，出事愈來愈難監控。
2. 監控演化錢可觀察性(observability)，用全貌來看。
3. observability
   1. metrics：系統有狀況嗎？
   2. traces：哪裡有問題？
   3. logs：問題是什麼？
4. 著重已知的未知問題
5. SigNoz：著重可觀察性
6. demo：
   1. 可以看到各種不同服務的資訊
   2. 圖形化可以看到redis、frontend、driver、route、db相互的關係
7. SigNoz是透過open telemetry達成
8. 不用再靠工程師的直覺解決問題

## type script 的泛型

1. [參考系列文章](https://ithelp.ithome.com.tw/users/20103315/ironman/4764?page=1)
2. 泛型：
   1. 型別可以當成變數傳進去型別裡
   2. 用`< >`

      ```ts
      type ConferenceWithGenerics<T> = {
         name: string;
         year:number;
         isAddToCalendar:boolean;
         price:T;
      }
      ```

   3. 傳進去`T`的型別是any，如果要限制：
      1. 可以用`extends`和`|`加以限制
      2. 用`=`給予預設值

         ```ts
         type ConferenceWithGenerics<T extends string | number = number> = {
            name: string;
            year:number;
            isAddToCalendar:boolean;
            price:T;
         }
         ```

3. utility type
   1. 例如內建的utility type：NonNullable type，可以把我們的型別裡面有null的東西拿掉
   2. typeof `實際的js object`，可以把和那個object所有value相同的type設定成聯集。
   3. `keyof typeof 實際的js object`，可以取得所有key的聯集(任選一個key的type)。
   4. 練習的utility type：
      1. indexed access types
      2. index signatures: 前身是Mapped Types
      3. Mapped Types
      4. Conditional Types
      5. Infer
      6. recursion
      7. 
4. Q & A：
   1. 官方建議儘量用interface而不是type，但是type功能較多。
   2. ts expect error暫時可以把ts lint的error comment掉。
   3. vscode裡的copilot extension可以預判你要輸入什麼程式碼。

## UX/UU

1. 如何讓新手和老手都好用的ux：
   1. 多元操作：
      1. 例如時間訂票和車次訂票。
      2. 多元支付：信用卡和現金都可以。
   2. 數據了解使用者多數用哪種操作。
      1. 例如多數都不印印手據：列印手據需要去點才會列印，不用詢問要不要列印。
2. UU：
   1. 通用使用性
   2. 目的是要克服這三大挑戰：
      1. 技術變化性，例如電腦和手機的差異，是不是能支援大部份硬體。
      2. 使用者多樣性：
         1. 不同年齡、感官能力、使用情境。
         2. 使用情境困難性：例如：某些情況被中斷要怎麼辦？條碼要加強亮度。
      3. 使用者的知識差距
3. 減少選項的方式：
   1. 分類
   2. 待某操作之後才顯現相關選項
   3. 善用位置，例如：近期最熱門的選項，可以放在最容易按到的地方。
4. 其它改善方式：
   1. 利用手機的輔助功能讓disability的人可以使用，用手機自有的功能，可以減少開發時間。
   2. 聊天機器人
   3. vr或ar輔助訓練
5. ui vs ux通常ui希望小，ux希望大，如何權衡：
   1. 愈大愈好
   2. 大的情況下去設計視覺，或是在視覺ok的情況下愈大愈好。
6. 前端工程師要入門的話，可以從案例多的書開始。

## SCRUM

1. 「死亡行軍」來自於一本書的書名。
   1. bug常增加
   2. sprint goal達不到
2. crunch mode：
   1. 長時間加班
   2. 只有短時間有效，長時間不明顯
3. 為什麼加班？
   1. 不用花腦筋解決
   2. 立即生效
4. cargo cult：貨物崇拜，二戰土人崇拜飛機的歷史，也是不了解真的SCRUM的精神。
5. Kent Beck：extreme programing反對加班，加班是已經發生根本性的問題了
6. ron jeffries：維持節奏
7. Kanban中如何維持節奏：舊工作沒完成不能一直加新工作。
8. sprint是團隊決定做多少，不壓樵員工。
9. PO：
   1. 對沒有價值的東西說NO，考慮這功能真的能賺錢嗎？
   2. 假設驅動開發：
      1. 假設xxx
      2. 訂定衡量指標，預訂率比以前增加5%
   3. 排好優先順序：
      1. 老闆通常不會願意cut feature
      2. 把不重要的排後面，後來來不及的話，不重要的有機會被cut掉
   4. 實例化需求：
      1. 需求太不明確
      2. 提範例，把範例先變成測試，可以減少需求錯誤。
      3. 寫出feature通過測試就ok了。
   5. 團隊體溫計，例如讓大家投票評估：
      1. 程式碼健康狀況
      2. 團隊開心指數
10. 休息時間
11. 專注：
    1. 重要的先做
    2. daily goal
    3. 不要開分心軟體
12. 找出瓶頸和浪費
    1. 記錄時間
    2. 通常：
       1. 會議太多
       2. 寫程式太少：超過4小時/天，就超過60%工程師。
       3. 問題知道太晚
13. 設定時間：
    1. 番茄鐘
    2. 開會時間
    3. 專屬開發時間
14. 問題知道太晚：
    1. pair programing大家討論過程可以更了解spec
    2. CI會自動程式
    3. CD確定環境是否有問題
    4. 可以提早知道問題
15. 不是提倡加班而是提倡可持續性的開發。

### Q & A

1. 會議scrum和其它會議時間要分開算，scrum會議不超過花13%的時間。
2. 開發40小時是不含會議時間。
3. 估計時間的重點在說清楚驗收條件，會比較準。

## 持續架構

1. 持續架構就是可以一直接受改變的架構
2. mainframe ... => micro service (分散式架構)
3. 軟體工程師容易落入只注意技術細節，而沒注意到bussiness context，但是會無法和stake holders溝通，所以架構師可以做這件事，將業務和技術綁在一起，不要脫勾。
4. 不同的架構師傳遞下去：enterprise, solutions data, security, software
5. 架構師要走進關懷了解工程師。
6. 架構師會被要求show me the code，但是在這之前應該要記錄show code之前的討論，這部份也很重要。
7. continuous architecture 6個原則：
   1. 從一個小project帶到整個架構
   2. 延後決策
8. 和傳統架構設計的不同：end to end
9. 雲端也需要managing failure

## 如何面對學不完的技術

- 葉丙成：
  1. 學校學習是structure learning，這個時代最重要的是unstructure learning，已經成熟舊了才會出書，unstructure learning才學得到最新的東西。
  2. 業界很缺會投臉書廣告的人，因為大學沒教。
- jserv：
  1. 以前學到不讀書，只背考題，很糟，但是很有效。
  2. 任何人都可以不管從理論或從實作，抓住一個根去學習。
- Denny Huang
  1. 學校老師能教的有效，仰賴開放原始碼的學習幫助非常大，但要習慣用英文取得資訊。
  2. 學習都是一個點一個點，由開放原始碼串起來，分享給別人時可以內化。
- csc camera：
    看第二手的教學文件外，還會去看第一手的官方文件。
- jserve：
  1. linux的原始碼已經3千萬行。
  2. 除了興趣，除了了解原始碼，後來需要去了解更基礎的編驛器，才能作出更有衝擊性的東西。
  3. 很多都想學，但是現在是高度分工的時代，要思考自己專長想學的，其它可以外包，增加效率。
- csc
  1. 選擇技術的原則。
  2. 看社區大小。
  3. 提交頻率高。
  4. 前膽性，不會去用全新的。
- 葉丙成：
  1. 台灣教育都是習慣準備好才上，常學得不紮實，或是學了一堆用不到的。
  2. 應該要先上戰場，再回來學習需要的，學習動機和速度會好很多。
  3. 要克服時間焦慮感。
- Denny Huang
  1. 原本英文很爛，但是讀英文文件時，才認真學英文。
  2. 技術社群研討論，要學的不只是技術，要學很多不相關的事，例如：行銷、財務。
- jserv
  1. 20年前的時代前、後端都得學。
  2. 現在愈來愈複雜分工愈來愈細，產業特性不同，自己要看清楚自己的定位，去學習所有和自己要克服的問題的東西。
  3. 時機點差很多，jserv當時linux很缺人，所以他根本不用寫履歷。
- 葉丙成
  1. 什麼是上戰場？
     1. 老闆要求
     2. 自己覺得工作上應該不得不用上
  2. 戰場上需要自己判斷事情的重要性的能力。
- Denny Huang
  1. 從社群可以被動的吸收這些資訊
  2. 加入社群也很容易找到對的人。
  3. 沒有戰場要怎麼學習，有一些工具可以產生高流量模擬。
- csc
  1. 如何找隊友，帶資淺的人，或花錢找高手。
- 葉丙成
  1. 從小功課很好。
  2. 但是創業開始遇到很多困難，教授創業沒有前例，要衝撞。
  3. 在台灣找資金很難，教授頭銜用不上，需reset成no body，說服別人自己企業有價值很不容易。
- jserv
  1. 最大的失敗是做非常多準備，但是沒有應用上。
  2. 刷題是一個短時間認識人的方式。
  3. 刷題要兼顧於應用上，比較值得。
  4. 除了執行的能力，要有選擇的能力。
- 葉丙成
  1. 興趣很重要，玩得很投入，比硬苦逼自己來的好。
  2. 再苦也沒問題。
- Denny Huang
  1. 很多事情不得不學習，例如學英文。
  2. 抱持好奇心。

## open policy agent

1. 原因：
   1. 邏輯複雜
   2. 跨multiple services
   3. multisystem：例如docker
2. 這套優點：
   1. golang高效率
   2. 容易testing
   3. 容易整合go app
   4. restful api可以跨service
3. agent判斷權限
4. policy engine需要
   1. data
   2. rule (polocy)：用rego特殊寫的，寫法很像go
   3. 設定運作格式
      1. input json
      2. result json
5. rego playground
   1. return
      1. true false
      2. string
      3. variable
   2. and or
   3. default
   4. 其它邏輯
6. testing
   1. 帶資料進來
   2. 驗證是否符合
7. group：可以處理繼承
8. 內嵌：比restful api快速，少掉latency
9. 驗證方式和jwt一樣

## deployment

1. cloud native 和 DevOps 最火紅
2. SRE是google提出的
   1. deploy and maintain
   2. 管理指標和預算
   3. 設定監看和alert
   4. be on call：錯誤不會無緣無故發生
   5. build tools和automations
   6. production issues要監控
   7. 各種雲的測試
   8. chaos engineering：netflix有名的chaos monkey
3. DevOps範疇：
   1. sdlc automation
   2. 管理infra透明
   3. monitoring and logging：朝向bussiness方向更有價值
   4. 規範和前置的流程自動化
   5. 出錯了如何找出原因
   6. 如何高容忍
4. 目前：
   1. 鬆散、彈性的系統
5. 系統設計商業和平衡
   1. 例如新創不注重安全
   2. 慢慢後來思考安全和deploy的速度
6. deploy automation的影響
   1. 出錯
   2. 管理瓶頸
   3. 透明度
   4. 成本
   5. feedback速度
7. CI / CD
   1. 各家都有CI, CD, issue tracking
   2. 更新更火的詞是progressive deployment：feature toggle
8. deploy要做到frequently and predictably
   1. frequently：順但不一定是快
   2. predictably：透明
9. 部署和交付差在哪？
10. infra as service：功能最全面，但是要了解限制在哪。
11. backend as service：最有名的是fire base
12. elastic service：注意價格
13. function as service：超紅
14. container as service：azure, k8s
15. 不要過早最佳化，挑對地方最佳化。
16. 根據商業需求目前商業規模剛剛好最好。
17. 根據商業驅動技術。
18. 大家要解決的問題是差不多的，痛點是差不多的。
19. 前端要學到docker為止，部署上跟大家很接近。純client side rendering就差比較多。
20. 後端要會哪些devOps？小公司沒有SRE，所以要全會，逃不過docker的。

## MySQL Operator for k8s

## cscheckin

1. 顏色挑選

## web vital

1. RAIL
   1. 100ms
   2. 10~12ms
   3. 50ms
   4. 1秒
2. 網站指標web vitals
   1. load speed：載入速度
   2. load responsivenes：能動嗎？
   3. visual stability：舒服嗎？
3. 工具：lighthouse…等
   1. 模擬
   2. 實地：使用者網路慢分數也差
4. Load Speed：
   1. FCP
   2. LCP
5. 範例：
   1. MIXTINI網站很多大圖
      1. responsive images/webp
      2. 圖檔放CDN上
      3. preconnect / dns prefetch
   2. 趨勢產品
      1. 編譯費時
      2. 載入速度慢
         1. 打包程式太大
         2. 解決方式
            1. route-base code splitting
            2. webpack
            3. 結果也讓編譯加快
      3. 效能流程化
6. 反應時間範例：露天的問與答
   1. 移除肥大lib用輕量
   2. code splitting
   3. defer async load js
   4. prefetch
7. visual stability
   1. 位移的比例
   2. 視覺流暢性
   3. 解決方式：
      1. image size要固定
      2. 預瀏太小
      3. foit / foet
      4. 露天：
         1. 預留
         2. 把footer移到預留之外
8. 用使用者的角度測出來的指標
   1. search console：全站關注的議題，網頁體驗報告
   2. lighthouse
   3. crux
   4. web.dev取得範例
   5. pagespeed
9. 影響SEO
10. 未來
    1. 執行互動性
    2. lighthouse會有treemap
11. 時程：切小工作
12. 凝聚團隊目標：使用者體驗
13. 要出書「打造高速網站，從網站指標開始」，裡面有練習          題
14. lightouse ci要觀測哪些path？找商業價值高的？哪些feature客戶愛用的？
15. 改效能之前要先記錄數據，改完才有辦法說服主管改是有效的，不然改完很難找到數據了。
16. popup對SEO有不良影響，但比較難說服PM或業務。
17. 網站優化練習題？新書裡，用side project，偷加新feature進公司的功能，規模有成績時才跟主管提完整計畫，排進下一個sprint裡，因為主管都要看report。
18. 平常常談常交流，比起突然直提來得比較不會害怕。
19. [用來量測數值的library](https://github.com/GoogleChrome/web-vitals)

## flutter開發windows程式

1. why？客戶希望的平台是windows和ios。
2. desktop可以大量呼叫system call
3. flutter不含installer，要買install shell或是要有寫installer的知識，其它平台直接發行在app store所以也不用管installer。
4. installer：
   1. 要注意的事
      1. 檔案位置
      2. 安裝位置寫入registry，未來才有辦法反安裝。
      3. 刪除需要刪除registry。
      4. 單用戶、整台機器？
      5. 如果有driver，要寫入系統目錄
      6. 是否要重開機？
   2. wix toolset
5. flutter是c++寫的
6. 和.net連接難，倒不如把.net的東西轉成cli，再跟cli連結。

## lightening show

1. dakuo
2. koko bot (chatbot)
3. rex - pycon tw 2022
4. jojo - 烏托邦計畫 podcast
5. 潘奕濬 - 快速學習新技術
6. recca chao - 後端可以用kotlin

