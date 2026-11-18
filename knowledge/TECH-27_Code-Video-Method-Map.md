# Code-Video 方法地圖與手機 Demo 指南

整理：2026-10-31（應作者要求：需要了解這批影片製作方法，特別是做手機 Demo 時）
材料：技術庫已入庫的六項（HyperFrames、huashu-motion、Lemo-Opuscar、Motion Engineering、video-director、motion-video-kit）＋ the author 現行生成式管線（Nano Banana 生圖 → OpenRouter Kling v3.0 Pro／HeyGen 圖生影片）。全部屬實務者方法論，無第三方 benchmark，品質宣稱多為作者自述，本圖是定位與選型參考，不是效果保證。

## 一、先分兩大家族

**生成式（作者現行管線）**：模型直接生成像素。擅長真人演出、情緒、氛圍、實景感。弱點：文字與 UI 會糊會亂、產品本體形狀不穩、同一角色跨鏡頭臉會漂移（已用多關鍵幀錨定緩解）、規格數字不可信。

**程式碼渲染（本批六項的共同路線）**：模型寫程式碼（HTML／Canvas／SVG／Three.js／GSAP／Remotion），逐幀確定性渲染。擅長精確：文字、數字、版面、產品幾何、品牌色全部可控、可重現、改一個數字重渲即可。弱點：做不出真人演出與實景質感，角色表演先天弱。

選型第一刀：畫面主角是「人與情緒」走生成式；主角是「產品、介面、資訊」走程式碼。很多成品是混合：情境鏡頭生成、本體與資訊鏡頭程式碼。

## 二、六個方法各佔一格（是分層，不是競品）

- **HyperFrames**（渲染平台層）：HTML/CSS＋seekable 動畫確定性渲成 MP4，本機可跑；agent 流程含 lint、抽幀檢查、Studio 預覽、核准後才 render。角色＝引擎。
- **huashu-motion**（導演方法論層）：做片前先定節奏（treatment、beat map、能量曲線、hook ≤3 秒、連續性鏈），聲音落點在創作時就標進時間軸，修訂用指點式回饋，完片 harvest 教訓。它的節奏數據量自 21 部 launch 片。角色＝導演的大腦。
- **Lemo-Opuscar**（風格資產層）：59 種風格即 prompt（講效果、不鎖參數），可改造、固化成自己的風格檔。角色＝可重用的視覺語言庫。
- **Motion Engineering**（組織流程層）：一個 agent 分飾導演／美術／動畫／渲染／Critic 五部門，各自有文件與驗收標準，Critic 用全新 context 只看 brief 與渲染幀，錯誤結晶成規則與 skill。角色＝團隊作業系統。
- **video-director**（量測 QA 層）：先審時間軸方案才准寫程式碼；渲完用腳本量測黑屏、文字對比、閃爍、凍結幀、循環接縫、響度。角色＝儀器檢驗員，管下限。
- **motion-video-kit**（商業交付層）：Gauntlet 審查迴圈（做的人≠審的人、逐項驗證留 ledger）、從 28 部 SaaS launch 片量的 motion grammar、產品 hero 寫實手法，外加產業購買證據與報價錨點的 business playbook。角色＝接案與出貨標準。

一條完整管線的組裝順序：風格與節奏先行（Lemo／huashu）→ 渲染（HyperFrames）→ 量測 QA（video-director）→ critic 審查（Gauntlet／Motion Engineering 的 Critic）→ 商業交付標準（motion-video-kit）。

## 三、手機 Demo 專節

以下「手機 Demo」指手機產品／app 的演示片（規格片、功能 demo、提案演示）。這是程式碼路線的主場，原因很具體：

1. **螢幕內容必須精確**：UI 版面、文字、數字、價格、規格，生成模型一律會糊字亂版，這類鏡頭只能程式碼或真實截圖合成。
2. **裝置本體必須像實物**：摺疊角度、轉軸、邊框、鏡頭模組位置，生成模型每鏡頭都在變，觀眾一眼出戲。
3. **改版成本**：規格一改（容量、顏色、價格），程式碼改參數重渲即可；生成式要整段重抽。

**建議的手機 Demo 製作堆疊**（每層對應庫裡的方法）：

- **裝置動作先量測，不憑空想**（motion-video-kit 的 product hero realism）：拿實機拍一段參考動作，量出關鍵角度與時間（measured angle keys），補間用 monotone cubic 保證不超程，相機鎖定，motion blur 用確定性累積式，不要隨機糊。燈光的任務是「揭露角度」不是打閃。
- **本體用 Three.js 建模、場景做成 seekable**：任一時間點畫面可重現，抽幀檢查才成立；爆炸圖分層＋投影標註線（projected callouts）是規格揭露的現成 pattern。
- **螢幕內容獨立製作再合成**：用真實 app 截圖／錄屏，或 HTML 重建 UI，貼回螢幕面做透視合成；螢幕的反光與霜面（screen frost）在合成層處理，維持跨鏡頭連續性。這條紀律與 the author 先前做片的經驗一致：UI 交給生成模型必壞，永遠後期合成。
- **節奏照 launch 片文法**：前 3 秒 hook、規格一個一個揭露（每個揭露要有所本，reveal cued to something）、hero moment 留給最貴的那個功能；節奏數字可查 huashu（21 部）與 motion kit（28 部）兩套量測。
- **聲音在時間軸階段就定**：選一首完整曲目做 back-timing，讓曲子的結尾落在 end card、切點對拍點；規格揭露與 hero moment 的音效要明確可聞，不要埋在音樂下。
- **QA 兩段都要**：量測段（video-director 式：文字對比、品牌色、凍結幀、響度）抓技術缺陷；critic 段（Gauntlet 式：全新 context 只看實際渲染、逐項對 brief 驗證、留 ledger）抓一致性與說服力。兩段都過才出貨，critic 說了算的是下限，品味與客戶框架仍是上限（motion kit 案例：critics 說 SHIP 後客戶仍改七輪）。
- **生成式只用在外圍**：生活情境、人物手持的氛圍鏡頭、情緒 B-roll 可以生成；凡是看得清螢幕或裝置本體的鏡頭不用生成。人物鏡頭沿用 the author 現行多關鍵幀錨定流程控制臉漂移。

**依 demo 類型速選**：
- 手機規格／發表片：Three.js hero＋HyperFrames＋量測 QA＋Gauntlet（即 motion-video-kit 的主場打法）
- app 功能 demo：真實錄屏為主體，程式碼做包裝（轉場、標註、節奏），不需要 3D 本體
- 概念情境片（給提案用）：生成式情境鏡頭＋程式碼資訊鏡頭混合，風格先用 Lemo 類方法定調
- 數據／資訊型短片：HyperFrames 或 Remotion 直做，huashu 的節奏設計先行

## 四、使用本圖的注意

- 六項全是實務方法論，互相引用、彼此綜整（video-director 的方法論就含 lemo-opuscar），不要把任何單項當權威標準。
- 所有「效果很好」的證據都是作者樣片或案例自述；真正採用前，先用一支最難的鏡頭做 pilot 驗證再放大（沿用 huashu 的 hardest shot first）。
