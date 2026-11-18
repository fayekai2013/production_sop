# ffmpeg-skill（kajisho5）：給 coding agent 的本地影片剪輯引擎

入庫：2026-11-05（作者指定了解；repo 與 README 已查證，GitHub API：4,575 stars、367 forks、2026-09-03 建立、MIT、Python，最後 push 2026-11-05；版本已到 3.0.0）

## 是什麼

一個 Agent Skill（Claude Code／Cursor／Codex／任何讀 SKILL.md 或 MCP 的 agent），把 FFmpeg 包成 42 個結構化工具，讓 agent 在本地剪片：剪接、去靜音、字幕與 karaoke、reframe、HDR→SDR 與 LUT、音訊清理與 dynamics、多機同步（含 drift 校正）、響度、平台交付檢查、整支 project 渲染、批次處理。全本地、無雲端、無 API key、只用 Python 標準函式庫。安裝：`npx ffmpeg-skill`；另有 MCP server 與機器可讀 contract。

作者刻意把定位切清楚：這個 repo 是「手」，只負責剪、量、輸出、回報結構化 JSON；它不決定剪點、不判定成品可否核准、不改寫字幕文字（使用者的 cue 原文照燒）。那些「腦」的工作在同作者的姊妹 repo（video-production-agent、AI-video-production-OS 等），依賴單向。

## 設計（值得記的部分）

- **固定工作流**：probe（先量實檔：時長、fps 含 VFR 偵測、解析度、色彩、音軌）→ 能無損就無損（stream copy 優先）→ plan／dry-run → 執行 → verify（probe 回量＋check.py 對平台規格＋look.py 產 contact sheet 由 agent 自己看）。每支工具都有 --dry-run、--plan、--json、--timeout、--overwrite（覆寫要明示同意），輸入永不被覆寫。
- **SPEC（Self-Producing Execution Contract）**：每支工具的 input_schema 不手寫，而是執行時從該 script 自己的 argparse parser 衍生；MCP 的 tools/list 也從同一 contract 翻譯。加一個 flag，CLI、contract、MCP 定義同步更新；文件或中繼資料落後會被 CI 的 test_contract 打回。作者把這個模式寫成立場論文〈What Is Verification-Driven Coding? — A Discipline for Not Trusting Generation〉（Zenodo, DOI 10.5281/zenodo.23720216），並且自評 ffmpeg-skill 本身對該紀律只是部分符合。
- **doctor 能力偵測**：先讀本機 ffmpeg build 的 encoders／filters／bsfs，逐工具回答 usable 與缺什麼、怎麼補；三態 available／missing／unknown，偵測失敗是 unknown，絕不報成 missing 或假裝 available。還會比對 FFmpeg 高嚴重度 CVE 表給升級建議（只警告、不擋）。
- **量測型驗證的誠實邊界**：check.py 驗黑幀、凍結幀、靜音、掉音、閃幀與平台規格；每個寫入型工具會在結果裡列出自己沒量到的 `unverified` 項目（畫面內容、字幕可讀性、mix 平衡、lip_sync 等），明說那些要靠 agent 看 look.py 的圖或聽。響度走兩段 loudnorm 並回量到 TP 達標為止（有損編碼會 overshoot，這點有處理）。
- **平台交付模板**：tiktok／reels／shorts／youtube／x／linkedin／facebook／podcast／audiogram，含各平台 UI 安全區（字幕避開 TikTok 說明欄與按讚欄）、響度目標與 check；render.py 可從同一個 edit 一次出七個平台版本並附對照表，也能 --export-timeline 輸出 FCPXML／EDL／OTIO 給 FCP、Resolve、Premiere 接手。
- **render.py 整支渲染**：宣告式 project.json（clips、轉場、字幕、overlay、音樂與 stem 電平、響度、匯出、章節、check），有 stage cache，只重跑變動的階段。

## 證據與成熟度

- 自測體系在 skill 類 repo 裡少見地完整：逐版 agent eval（prompt 集從 24 題長到 100 題，含多語言與必須拒絕的題目），由獨立模型評分（regex grader＋Opus grader 實際打開每張 contact sheet、回量每個輸出），結果逐 iteration 公開，**包含失敗**：例如 eval 21 路由漏找五個新 flag、eval 18 發現 --fit-size 在模板路徑上不可達、字幕邊界 bug 藏了三個版本才被 eval 抓出並修。真實裝置語料（GoPro、DJI、iPhone 含 Dolby Vision、Android 錄影等 10 檔）驗證步數 92/92；sync 誤差、silence、scenes 都有 benchmark 數字。
- FFmpeg 6.1／7.1／8／9 在 CI 測；Linux／macOS／Windows 每 PR 跑測試套件。
- 限制（作者自述）：真實裝置語料全集尚未在 Windows 跑完，非維護者在 Windows 的安裝未被重現（issue #143 追蹤中）；轉錄是可選的本地引擎（faster-whisper 等），不裝就拒絕並給安裝指令，不假裝能做；highlights 只是量測代理（音量、時長），不是內容判斷；look.py 只產圖，判讀是呼叫端 agent 的視覺，不是本 repo 的能力。

## 與技術庫內其他項目的對照（僅庫內對照）

- 在 code-video 家族裡佔「工具層／執行層」的位置：HyperFrames 是渲染平台，video-director 與 motion-video-kit 是方法論與 QA 規格，ffmpeg-skill 是把剪輯執行與量測驗證做成 agent 可呼叫工具的那一層，正是先前 DeepSeek 對照裡點到的「工具層產品化」的一個現成範例（contract、doctor、SPEC 的防漂移機制）。
- 與 TokenEconomizer 同屬「給 coding agent 的成本與可靠性外掛」思路，但面向不同：那件省 token，這件省猜測（probe first、typed tools 取代 shell 字串）。
