# Taiwan Housing Intelligence Agent — 期末專題提案工作紀錄

整理日期：2026-11-11
主題：結合實價登錄、空間特徵與可解釋 AI 之房地產決策代理人（ReAct + RAG + Evidence Gate）

> 本文件整理自一次工作對話，內容包含：資訊圖表檢查與重畫、A–F 各段的佐證資料、預期貢獻與學術價值、500 字摘要定稿，以及待辦事項。
> 文中的外部數據來自搜尋摘要，尚未逐篇核對原文，**定稿前請回原始來源確認數字與頁碼**。

---

## 1. 資訊圖表：檢查與重畫

### 1.1 原圖的問題

對照《房產AI提案評估》分析報告後，原圖有以下缺漏：

| # | 問題 | 說明 |
|---|---|---|
| 1 | 底部被裁切 | 第 6 節標題寫「6.1–6.4」，只看得到 6.1、6.2；畫布高度固定，右欄溢出 |
| 2 | 第 7 節不存在 | O 列寫「見 7.4①」，但圖上沒有第 7 節 |
| 3 | 缺「繳交資源」 | 報告二.6 的 GitHub 私有儲存庫、DESIGN.md、34 頁 PDF 簡報沒有畫出 |
| 4 | 缺「總結評語與改進建議」 | 報告第四節的 3 項亮點、2 項建議整節沒有 |
| 5 | A2Z 定義與報告不一致 | 報告按 6 章節分組並給評等；圖上逐字母列出，且 N、O、Q、R、S、X、Y、Z 的定義與報告不同 |
| 6 | 流程圖少一條分支 | 4.2 沒畫「條件檢查不通過 → Safe Stop」，與 Pseudocode 第 03 行不一致 |
| 7 | 圖例位置 | 「橘底＝待補強」只放在第 5 節旁，其他節也用了橘色 |

### 1.2 重畫版的改動

輸出：`Taiwan_Housing_Agent_infographic.png`（3174×2244，A4 橫式 2×）

- **第 5 節 A2Z**：改為報告的 6 章節分組與字母定義，每組附評等；L、O 以橘色標為待補。
- **新增第 6 節「資料可行性與繳交」**：6.1 資料盤點（10,638 → 9,152 筆，Conditional Go）、6.2 交付資源。
- **新增第 7 節「總結評語與改進建議」**：7.1 三項亮點、7.2 兩項建議（含 Article Summary Table 的 8 個欄位與 List Organization 示範句）。
- **4.2 流程圖**：補上「條件檢查不通過 → Safe Stop」。
- **原第 6 節（評估設計）**：收進 5.1，對應 A2Z 的 Q–V。
- **圖例**：移到頁首。

### 1.3 重畫版中待確認的三處

- Pseudocode 沿用 11 行版本，提案原文是 01–08 行摘要；有原文可替換。
- 報告在 Q–V 段只列 Q、R、S、V，T、U 無定義，圖上未放。
- 7.2② 示範句後半「第 4 節說明系統設計」是補寫的，報告原句在「第 3 節深入研究…」中斷。

---

## 2. A–F 各段的佐證資料

三個主軸：

- **Important**：臺灣購屋負擔在國際比較中偏重，臺中市正處於價格修正期。
- **Interesting**：加了 RAG 之後，模型在資料不足時反而更容易硬答。
- **Innovation**：既有房產 Agent 研究著重多代理人推薦與諮詢；「有界迴圈＋證據閘門＋安全停止」用在實價登錄上可區隔。

### A. Attention Getter

| 重點 | 數據 | 來源 |
|---|---|---|
| 全國房價所得比 | 2025Q4 為 9.32 倍，連 5 季下降；高點 2024Q3 為 10.82 倍 | [自由財經 2026/06](https://ec.ltn.com.tw/article/breakingnews/5466043) |
| 國際比較 | 央行資料：台北市 14.6 倍，高於香港 14.4 倍 | [工商時報 2026/07](https://www.ctee.com.tw/news/20260731700863-430601) |
| 累計漲幅 | 2020 年以來名目漲幅：國泰房價指數 86.0%、信義 54.9% | [央行理監事會後參考資料](https://www.cbc.gov.tw/dl-226940-658c58fd1c4f4f2b999da39dbc431250.html) |
| 臺中情境 | 2025Q4 房價指數年減 6.65%，六都最大 | [經濟日報](https://money.udn.com/money/story/5621/9529955) |
| 車位失真 | 北市地政局試算：2,000 萬／40 坪含車位，未拆算每坪 50 萬，拆算後 60 萬 | [理財周刊](https://www.moneyweekly.com.tw/Magazine/Info/理財周刊/72835) |

### B. But

| 重點 | 內容 | 來源 |
|---|---|---|
| 官方工具也修正過 | 北市地政雲 2020 年版以 300 公尺內車位交易推估，與實際有差異，後推出 2.0 | [工商時報](https://ctee.com.tw/news/real-estate/640670.html) |
| 登錄資料品質 | 110 年度內政部預售屋聯合查核，逾 7 成案件不合格 | [監察院調查報告](https://cybsbox.cy.gov.tw/CYBSBoxSSL/edoc/download/62973) |
| 同數據不同解讀 | 樂居質疑央行跨國比較的統計口徑 | [樂居 2026/08](https://www.leju.com.tw/page_blog/view/2927) |
| 單一估價數字的代價 | Zillow 2021 年關閉演算法買房業務，單季虧損約 3.8 億美元 | [Star Tribune](https://m.startribune.com/zillow-had-a-small-presence-in-twin-cities-real-estate-as-it-exits-home-deals/600112672/) |
| LLM 估價過度自信 | LLM 給的價格區間偏窄，低估不確定性 | [arXiv 2506.11812](https://arxiv.org/html/2506.11812v1) |

### C／D. Cure、Development

| 重點 | 內容 | 來源 |
|---|---|---|
| RAG 不等於不幻覺 | Stanford 實測法律 AI 工具：Lexis+ AI 錯誤率超過 17%，Westlaw 超過 34% | [Stanford HAI](https://hai.stanford.edu/news/ai-trial-legal-models-hallucinate-1-out-6-queries) |
| Evidence Gate 的依據 | ICLR 2025〈Sufficient Context〉：RAG 降低模型拒答能力；Gemma 錯答率由 10.2% 升到 66.1%；建議生成前加充分性檢查 | [Google Research](https://research.google/blog/deeper-insights-into-retrieval-augmented-generation-the-role-of-sufficient-context/) |
| 同領域系統 | HabitatAgent（四角色多代理人） | [arXiv 2604.00556](https://ideas.repec.org/p/arx/papers/2604.00556.html) |
| 同領域系統 | PropGenie（EACL 2026 Demo） | [ACL Anthology](https://preview.aclanthology.org/credits/2026.eacl-demo.3) |
| 同領域系統 | EXPRESS（LLM 逐特徵估價解釋） | [arXiv 2025](https://aitopics.org/doc/arxivorg:B34927EA) |

### E. Experiments

| 重點 | 內容 | 來源 |
|---|---|---|
| Workflow vs. Agent | Workflow 提供可預測性與一致性；Agent 適合需要彈性的情境 | [Anthropic: Building effective agents](https://www.anthropic.com/research/building-effective-agents) |
| 可靠性看重複執行 | τ-bench：GPT-4o 零售任務單次約 61%，pass^8 低於 25% | [τ-bench 摘要](https://papers.lunadong.com/paper/4601) |
| 有界迴圈的實務佐證 | 306 位實務者調查：68% 的生產環境 Agent 最多執行 10 步（**二手引述，需找原始研究**） | [tinyfish](https://current.tinyfish.ai/tinyfish/issue/33/foundations/article/22078) |

### F. Findings

| 重點 | 內容 | 來源 |
|---|---|---|
| Safe Stop 的學術定位 | 拒答（abstention）綜述 | [TACL 2025](https://arxiv.org/html/2407.18418v3) |
| 指標對應 | selective accuracy、coverage 可對應「引用支持率」「安全停止率」 | [Sufficient Context 論文頁](https://research.google/pubs/sufficient-context-a-new-lens-on-retrieval-augmented-generation-systems-2/) |

### 引用時的注意事項

- 車位試算（2021）、監察院報告（110 年度資料）、Zillow（2021）是舊案例，適合當背景，不要寫成「最新」。
- 房價所得比各來源季別不同（2025Q2 臺中 11.9 倍、2025Q4 全國 9.32 倍），引用時標明季別。

---

## 3. 預期貢獻與學術價值

所有貢獻都是**預期**，成立與否取決於後續實驗。

### 3.1 預期貢獻

1. **方法**：把「證據是否足夠」做成明確的系統元件（Evidence Gate ＋ Safe Stop），檢查來源、有效時間、樣本數。
2. **設計**：提出房產回答的輸出規格——每則回答固定附「數值＋樣本範圍＋日期＋來源＋限制」。
3. **實證**：在相同條件下比較有界 ReAct Agent 與固定 Workflow，提供房產領域的對照數據。
4. **資料與應用**：以臺中市 115 年 Q3 的 9,152 筆住宅候選建立可重現基線，公開篩選條件與限制。

### 3.2 學術價值

| 面向 | 價值 | 對應文獻脈絡 |
|---|---|---|
| 可信任 AI | 把「拒答」變成可量測的系統指標 | 拒答綜述（TACL 2025）；selective accuracy／coverage |
| Agent 可靠性 | 以有界迴圈限制自主性，可加測重複執行一致率 | τ-bench |
| 領域 AI | 高風險決策領域的 RAG 案例 | Stanford 法律 RAG 實測 |
| 可解釋性 | 區分「解釋」與「因果」，SHAP 結果附限制 | 提案 Q1 設計原則④ |
| 研究方法 | Proposal 階段不虛構成績，效益寫成可否證假設 | 提案誠信聲明 |

### 3.3 寫作建議

- 貢獻用可檢驗的句子寫，例如「H1：在相同查詢集上，有界 ReAct Agent 的引用支持率不低於固定 Workflow」。
- 負面結果也算貢獻：若 Agent 未優於 Workflow，結論是「此類限定任務用 Workflow 即可」。
- 單一城市、單一季度的樣本只支持「初步情境」的結論。

---

## 4. 摘要定稿（約 520 字）

臺灣購屋負擔沉重，2025 年第四季全國房價所得比為 9.32 倍，臺中市同季房價指數年減 6.65%，為六都最大。住宅決策須同時比較價格、面積、位置與法規，而實價登錄單價易受車位與建物型態影響，依官方試算，拆算車位後之單價可高出二成。然而，公開成交紀錄不等於完整市場行情，傳統估價僅輸出單一數字，無法呈現樣本範圍、時效與資料限制；既有研究亦指出，檢索增強生成在脈絡不足時，模型傾向給出錯誤答案而非拒答。為此，本研究提出 Taiwan Housing Intelligence Agent，以 ReAct 結合 RAG 建構房地產決策代理人，並設計 Evidence Gate，於生成前檢查證據是否充分，不足時啟動 Safe Stop 並說明資料缺口。系統由查詢驗證、有界 ReAct 迴圈、實價登錄結構化資料工具、法規與資料說明知識庫、Evidence Gate 及 Safe Stop 組成，每則回答固定附數值、樣本範圍、日期、來源與限制。驗證以臺中市 115 年第三季買賣案件 10,638 筆中篩出之 9,152 筆住宅樣本為情境，於相同查詢集下比較有界 ReAct Agent 與固定 Workflow，指標包含 MAE、RMSE、R²、引用支持率與安全停止率。本階段尚無實測成績，所有效益均為待驗證假設；預期貢獻為可量測之證據閘門機制、房產回答之輸出規格，以及 Agent 與 Workflow 之領域對照基線。

**關鍵詞**：實價登錄、ReAct、檢索增強生成（RAG）、Evidence Gate、Safe Stop、可解釋 AI

### A–F 對應

| 段 | 內容 | 相較原提案的加強 |
|---|---|---|
| A | 購屋負擔、臺中跌幅、車位失真 | 補三個數字 |
| B | 成交紀錄 ≠ 行情；RAG 脈絡不足時硬答 | 補 ICLR 2025 研究依據 |
| C | ReAct + RAG + Evidence Gate | 寫明為「生成前的充分性檢查」 |
| D | 六個元件與固定輸出規格 | 補輸出規格 |
| E | 9,152 筆樣本、相同查詢集、五項指標 | 寫明樣本數與對照條件 |
| F | 無實測成績；三項預期貢獻 | 貢獻收斂為三項 |

---

## 5. 待辦事項

- [ ] 回原始來源核對第 2 節所有數字與頁碼
- [ ] 補 Article Summary Table（Ref., Year, Problem, AI Method, Domain Knowledge, Dataset, Findings, Limitation），放在 Related Work 末段
- [ ] 在 Introduction 1.3 後補 1–2 句章節導覽（List Organization）
- [ ] 確認 Pseudocode 是否換回提案原文的 01–08 行版本
- [ ] 決定是否加入「重複執行一致率」指標（提案原文沒有）
- [ ] 找「306 位實務者調查」的原始研究，或移除該引述
- [ ] 新一季房價所得比公布後更新 A 段數字
