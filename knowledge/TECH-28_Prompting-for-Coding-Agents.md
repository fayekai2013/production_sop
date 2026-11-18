# 下 Prompt 的藝術：從 napali 與 Athenaion 拆解「不浪費、直達有效」的方法

記錄：2026-11-07（作者指定記錄：下 prompt 的藝術、規則、寫法，如何不浪費而直接用最有效的方法、充分利用 agent 的 coding 能力）。

證據來源（一手材料）：Leon Lin（Leonxlnx）沒有公開 napali 的逐字 prompt，但他的姊妹作 Athenaion（同作者、同期 2026-10-16 至 18、同一套方法：Claude Code＋Opus 5.5、參考圖、專家分工、judge 迴路）公開了完整的一手文件：原始 prompt（docs/PROMPT.md）、美術聖經（docs/ART_BIBLE.md）、工程契約（docs/CONTRACT.md）、導演構圖計畫（docs/COMPOSITION.md）。napali 自己的 repo 痕跡（347 處 round 標記註解、tweaks patch 帳、URL 旋鈕系統）與這套文件系統逐項對得上。以下規則全部出自這兩份一手材料，不是泛論。

## 一、原始 prompt 全文（Athenaion，作者自述為該 session 全部訊息合併）

> Build me a massive, ultra-detailed 3D world in three.js based on the 9 reference images I attached.
>
> Rules:
> - Zero external assets. Every mesh, texture, shader and effect has to be generated in code.
> - Match the references as closely as possible (aim for 99.9%): composition, scale, materials and especially the colors. Cinematic and realistic, not oversaturated, but also not grey or washed out.
> - Detail everywhere. Nothing should look copy-pasted or simple. Every building, tree, rock and waterfall gets care, and the world has to read as ultra detailed even from far away.
> - Waterfalls and water have to look realistic.
> - My GPU is weak (integrated Radeon 780M), so keep it efficient and get the quality from offline rendering instead.
> - Work only inside this project folder.
>
> Process:
> - Iterate for as long as it takes (hours or days). Don't stop before it matches the references.
> - Render the matching shots as stills and show them to me first. Don't render the video before I approve.
> - After I approve: render a cinematic ~1 minute showcase video in 4K at 30fps with slow camera moves through the different scenes, no text on screen, impressive from the very first frame, and free cinematic background music from the web (credit the artist).

全文約 200 字。注意它**幾乎不描述世界長什麼樣**，因為規格不在文字裡，在 9 張參考圖裡。文字只負責四件事：目標（match references）、硬規則（zero assets 等）、品味邊界（用負面句劃出：not oversaturated, not grey, not copy-pasted）、流程與交付（先靜幀、我核准、才出片；出片規格精確到 4K/30fps/約 1 分鐘/無字幕/第一幀就要好）。

## 二、真正的系統：prompt 只是第一層，後面有三份文件

作者的方法不是「一條神 prompt」，是 prompt 點火、文件接力：

1. **PROMPT（人寫，一次，短）**：目標、硬規則、品味邊界、流程、交付規格。如上。
2. **ART BIBLE（agent 建、人核准方向，持續維護）**：把「像參考圖」翻成可量測的規格。實例：曝光目標直接給 sRGB 亮度百分位數（p02 ≈ 3–17、p50 ≈ 93–115、p98 ≈ 227–242，橫跨 9 張參考圖量出來的），並指定驗證工具（`colorstats.mjs`）；旗幟顏色給 hex（navy ≈ #1b2550、gold ≈ #c9a24e）；大理石受光色 ≈ #e9dfcc。每個地點、每種植物、每樣道具都有規格段落。品味一旦變成數字，後面每一輪就不用再用形容詞溝通。
3. **CONTRACT（工程契約，每個專家 agent 動工前必讀）**：怎麼跑 render、模組介面（每個模組 export build/test 兩個函數）、**所有權**（自己的檔案才能改；共用檔案只有 owner 能改，別人只能最小增補、改前重讀；別人的檔案有問題寫進自己的最終報告，不代改）、READY 標記（API 穩定就放標記讓別人開始依賴，之後保持簽名相容）、效能預算寫死（draw calls ≤ ~3000、三角形 ≤ ~25M、每模組建置 ≤ ~10 秒）、決定性（seeded RNG，禁用 Date/Math.random，同 code 同圖，輪次之間才可比）、工作習慣（迭代用 preview 品質、驗證才用 high；不要 spam render，先想再渲；自己造成的 error/warning 自己修完）、交付報告格式（做了什麼、檔案、API、耗時、已知缺口、給其他模組的建議）。
4. **COMPOSITION（導演的量測計畫）**：把參考圖**反向工程成數字**：用針孔相機 solver 擬合參考圖裡神廟的角點（推出柱高 10.4 m、神廟總高 18.1 m），跨 9 張圖三角測量地標位置，逐鏡給距離與方位角（例：鏡頭距神廟 210–240 m、vfov 35–40°），並記錄「第 0 輪以為是 600+ m，量完改正」這種用證據推翻第一猜測的修正。第 4 節是「給其他模組的需求清單」，第 5 節是狀態與變更帳。

napali 的對應物在 repo 裡一模一樣：Art Bible 的角色由參考照片＋judge 承擔；旋鈕系統（數百個 URL `num()` 參數）是「品味變數字」的執行面；tweaks.cjs 的 patch 帳與 347 處 round 註解是變更帳，註解裡全是量測句：「r10 的地形槽紋把裸岩比例從 7.3% 推到 12.7%，參數改回讓它回到約 7.8%（以 hd3 讀數為準）」、「r8：swell bands .1 → .12，因為量到照片的 64px 區塊標準差 1.32、我方 0.88」。還有回歸教訓直接寫在 patch 旁：「r4 那次放寬把沙畫到懸崖腳、拔掉海灘前的灌木、長出一片灰 flats，已撤回」。

## 三、規則（拆成四組）

### A. Prompt 本身的寫法

1. **規格放在參考物裡，不放在形容詞裡。** 一句「match the references」加 9 張圖，勝過三頁描述。形容詞每一輪都要重新解釋，參考物每一輪都自己說話。
2. **硬規則短、絕對、可驗證。** 「Zero external assets」沒有詮釋空間，一眼能查。規則要少而硬，不要多而軟。
3. **品味用負面清單劃界。** not oversaturated / not grey or washed out / nothing copy-pasted：先說不要什麼，比說要什麼更能防 agent 的預設傾向（模型有已知的slop 預設，負面句是直接對著預設打）。
4. **環境限制在第一句就講。** GPU 弱 → 策略直接變成「效率優先、品質靠離線渲染」。限制晚講，前面所有輪次都在錯的假設上浪費。
5. **流程寫進 prompt，不靠臨場指揮。** 先靜幀、給我看、核准後才出片；迭代到吻合為止、不要停。這幾句把人的介入點固定在昂貴動作之前，其餘時間 agent 自走。
6. **最終交付規格精確到數字與形式。** 4K、30fps、約 1 分鐘、慢鏡頭、無字幕、第一幀就要 impressive、音樂要免費且標註出處。交付定義模糊，收尾就會來回。

### B. 不浪費的機制（省下的都是回合與重做）

7. **品味量化，一次到位。** 亮度百分位、hex、百分比讀數（裸岩 7.3% vs 12.7%）一旦寫進文件，後續溝通全部變成「對數字」，不再有「再亮一點、再自然一點」的來回。
8. **參數旋鈕化。** look 參數全部外露成可調旋鈕（napali 是 URL 參數，含每個子系統的微調值）。調參是轉旋鈕，不是重下 prompt、不是改 code 結構。prompt 只處理結構性決定，旋鈕處理品味微調，兩種工作分開就不互相污染。
9. **量測工具先行。** 開工早期就建 crop（局部對照）、sample（取色）、colorstats（曝光統計）、render/session（快速出圖）這些工具。沒有量測，judge 只能憑感覺，迭代就變成抽獎。工具是 agent 自己建的，prompt 要授權並要求這件事。
10. **一輪只修 judge 點名的，用 patch 不用重寫。** tweaks 是字串 patch 累積：被點名的部位才動，其餘凍結。全面重寫會帶來回歸（r4 的教訓就是放寬一處、壞了三處，而教訓本身也被寫回 patch 旁邊，下次不會再犯）。
11. **知識寫回 repo，不留在對話裡。** 每輪的判讀、理由、量測值寫進註解與文件。下一輪（或下一個 agent）讀檔就懂，不必重新考古、不必人再講一次。對話會消失，檔案不會。
12. **迭代預算用時間給，不用次數催。** 「iterate for as long as it takes (hours or days). Don't stop before it matches」是把品質門檻設成停止條件，而不是把回合數設成停止條件。agent 不會為了交差而提前收尾。

### C. 充分利用 coding 能力的分工

13. **按自然子系統切專家，不按方便切。** water / sky / terrain / vegetation 是物理上自洽的子系統，各自的 specialist 可以深挖該領域的方法（FFT 海洋、侵蝕模擬都是這樣來的）；切法對了，agent 的深度才出得來。
14. **介面與所有權先於產出。** 模組契約、檔案所有權、READY 標記、簽名相容，讓平行工作不互相踩壞。平行而不設契約，只會平行地製造衝突。
15. **效能預算是規格的一部分。** draw calls、三角形、建置時間寫死在契約裡，agent 會自己用 instancing、baking、LOD 去達標。預算不寫，品質衝上去之後才發現跑不動，整段重做。
16. **決定性是迭代的地基。** seeded RNG、固定時間、禁止隨機來源：同 code 同圖，每一輪的差異才完全歸因於這一輪的改動，judge 的判讀才有意義。

### D. 人的角色

17. **人做三件事：給參考、定邊界、當最終裁判。** 不下場改參數、不逐行指揮。人的時間花在核准點（靜幀）與品味裁決，這是 agent 取代不了的兩件事。
18. **核准點設在昂貴且不可逆的動作之前。** 先靜幀後影片、先 pilot 後放大。便宜的階段多迭代，昂貴的階段只執行已核准的版本。

## 四、可直接套用的骨架（下次給 coding agent 的任務）

```text
目標：做 [成品]，對齊 [參考物：圖/連結/數據]，吻合度以 [參考物] 為準。
硬規則：
- [絕對禁令 1，例如：只用程式生成，不用外部素材]
- [絕對禁令 2，例如：只在這個資料夾內工作]
- [環境限制：硬體/時間/API 額度，先講]
品味邊界（不要什麼）：
- 不要 [模型常見 slop 1]、不要 [slop 2]、不要 [slop 3]
流程：
- 先把 [參考物] 量測成規格文件（數字、色碼、比例），給我看過再動工。
- 先建量測與預覽工具，再寫功能。
- 先出 [便宜的驗證物：靜幀/原型/單一模組]，我核准後才做 [昂貴的成品]。
- 迭代到與參考吻合為止；每輪只修評斷點名的問題，改動與理由寫回文件。
- 參數做成旋鈕，不要寫死在結構裡。
交付：
- [形式、尺寸、規格、長度、格式，全部數字]
- 附最終報告：做了什麼、已知缺口、量測對照。
```

## 五、反模式（浪費的來源，對照）

- 用形容詞堆規格（「要很壯觀、很真實」）→ 每輪重新解釋，永遠對不齊。改法：給參考物＋量測。
- 第一版就要求全成品 → 錯誤在昂貴階段才暴露。改法：靜幀／pilot 先行＋核准關卡。
- 每輪全面重寫 → 回歸四起、舊教訓重犯。改法：patch 累積＋教訓寫回。
- 參數寫死在 code → 調品味要動結構。改法：旋鈕化。
- 沒有量測工具就開工 → judge 憑感覺、迭代變抽獎。改法：工具先行。
- 平行分工不設所有權 → 互相覆寫、merge 地獄。改法：契約＋所有權＋READY。
- 用回合數當停止條件（「做三版給我」）→ agent 交差式收尾。改法：用吻合度當停止條件、時間當預算。

## 六、與作者既有標準的關係

這套和 2026-10-24 立的「創島提示詞」品質標竿是同一家族：創島是**單次規格書**路線（規格即程式、數值寫死、一次到位），napali 法是**迭代迴路**路線（規格在參考物與量測文件裡、靠輪次收斂）。簡單可完全預寫的成品用前者；品味重、無法預寫、要對齊參考物的成品用後者。the author 既有的 G1 核准＋pilot 先行（影片製作線）與此法的「靜幀先行、核准後出片」同構，已在用，只是這份把迴路其餘部件（量化品味、旋鈕、所有權、教訓寫回）補齊成完整系統。
