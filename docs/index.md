# 打造你的 AI Agent：用 Hermes Desktop + OpenRouter

*2026 年 6 月 16 日*

---

## 為什麼要學這個？

### 一般 LLM 和 AI Agent 有什麼不同？

你可能已經用過 ChatGPT、Claude 或 Gemini 這些 AI 聊天工具。它們很聰明，能回答問題、寫文章、翻譯；但它們**只能跟你聊天**，沒辦法真的幫你做事。

**AI Agent 不一樣。** 它不只會說，還會**動手做**：

Agent 的核心差異不在「能做什麼」，而在「如何做」——它能自主規劃、使用工具、並根據結果迭代調整。

| 能力維度 | 一般 LLM | AI Agent |
| --- | --- | --- |
| 回答問題 | ✅ 主要能力：根據輸入生成文字回答 | ✅ 具備：也能回答問題，但通常不只停在回答 |
| 自主規劃與多步驟執行 | ⚠️ 可協助分解任務，但多半需要使用者逐步指示與確認 | ⭐ 核心能力：能自行規劃步驟、根據中間結果調整策略，並持續執行到目標完成 |
| 使用外部工具 | ⚠️ 可使用工具，但通常流程固定，工具由開發者預先設計 | ⭐ 能根據任務需求動態選擇工具，例如搜尋、查資料庫、呼叫 API，並整合結果 |
| 執行程式碼／操作檔案 | ⚠️ 多半限於平台提供的沙盒環境，難以直接操作使用者本機 | ✅ 可連接本地電腦、伺服器或開發環境，執行指令、讀寫檔案並根據結果調整下一步 |
| 記憶與上下文 | ✅ 可利用目前對話內容，也可能搭配記憶或 RAG 來回答問題 | ⭐ 更強調任務狀態管理：能記錄任務進度、更新中間結果，並在後續步驟中主動取用 |
| 串接外部平台 | ⚠️ 多半只能在平台允許的範圍內使用，串接能力有限 | ✅ 可串接外部平台，例如 Telegram、Discord、Slack 等，接收訊息、發送回覆或觸發自動化流程 |

想像你要跑一個耗時的實驗或備份，不想乾等在電腦前。你掏出手機，傳一句話給你的 Bot：「開始備份桌面資料夾」或「執行這個分析程式」。Bot 立刻在你的電腦上動起來，你去吃飯、去上課，回來就完成了。這不是告訴你怎麼做，而是**直接幫你做完**。

### 為什麼選 Hermes Agent？

[Hermes Agent](https://github.com/NousResearch/hermes-agent) 是由 [Nous Research](https://nousresearch.com/) 開發的開源 AI Agent 框架，有幾個特色讓它適合學習和使用：

- **開源授權（MIT）**：可自由使用、修改與商用，方便教學與二次開發。
- **不綁定單一模型**：可搭配雲端 API 或本地模型使用。
- **多平台串接**：可透過 Telegram、Discord、Slack、WhatsApp 等平台與 Agent 互動。
- **記憶與技能系統**：能從過去的任務中學習，把成功的流程自動整理成「技能」，下次遇到類似任務可以直接複用。

### 今天的目標

---

## 介紹介面

*帶大家認識 Hermes Desktop 的主畫面與各區塊功能。*

!!! note "📷 截圖：主介面"
    在此插入 Hermes Desktop 主畫面截圖，並標註：對話區、subagent 面板、設定入口等重點區塊。

<!-- TODO:
  - 逐一說明畫面上的主要區塊
  - 標出待會會用到的按鈕／入口
-->

## 設定檔（config.json）

*提供一份準備好的 `config.json`，讓大家直接 import，省去逐項手動設定。*

<!-- TODO: 放上下載連結／檔案，並說明 import 的步驟（從哪個選單匯入）。 -->

```json
// TODO: 貼上範例 config.json 內容
{
}
```

### Backend：Docker vs. Local

*說明兩種後端執行方式的差別，協助大家依環境選擇。*

| 比較項目 | Docker | Local |
| --- | --- | --- |
| 安裝與環境隔離 | <!-- TODO --> | <!-- TODO --> |
| 啟動速度 | <!-- TODO --> | <!-- TODO --> |
| 適合情境 | <!-- TODO --> | <!-- TODO --> |
| 注意事項 | <!-- TODO --> | <!-- TODO --> |

!!! tip "如何選擇"
    <!-- TODO: 一句話總結：什麼情況用 Docker、什麼情況用 Local。 -->

## 委派任務（Delegate Tasks）的概念

*解釋「委派任務」是什麼、為什麼要把工作拆給多個 subagent，以及 context 為何無法跨 subagent 共享。*

<!-- TODO:
  - 什麼是 subagent？跟主對話的關係
  - 為什麼要委派：分工、各自專精的 toolset
  - 關鍵限制：subagent 看不到前面的對話，必要資訊要「完整貼進」它的 context
-->

## Phase 1：用 Subagent 製作教學投影片

*第一次體驗：用兩個 subagent（Researcher → Slide-maker）做出一份解釋 multi-agent 合作的投影片。*

請直接複製以下 prompt 給 Hermes：

```text
請用兩個 subagent 完成一份「解釋當前multi-agent 合作」教學投影片：

步驟 1 派一個 Researcher（toolsets: web）：
搜集「LLM multi-agent 合作」的重點，回傳：5 個關鍵概念 + 每個一句說明
+ 3 篇代表性論文（標題/年份/連結）。用條列回傳。
請將檔案存在/Users/amberber/Downloads/hermes-out

步驟 2 收到 Researcher 摘要後，把那份摘要「完整貼進」下一個 subagent的 context（因為它看不到前面的對話），派一個 Slide-maker
（啟用 PowerPoint skill，toolsets: 程式執行 + 檔案）：
根據摘要做一份 8 頁 .pptx：封面 + 5 個概念各一頁 + 參考文獻頁。
```

### 內建 Skill：產生 PPT

*說明 Slide-maker 如何透過內建的 PowerPoint skill 把摘要轉成 `.pptx`。*

<!-- TODO:
  - PowerPoint skill 在做什麼
  - 啟用方式、需要哪些 toolset（程式執行 + 檔案）
  - 產出檔案會放在哪裡
-->

### 討論

*帶大家檢視成果並反思流程。*

<!-- TODO:
  - 兩個 subagent 的分工是否清楚？
  - 「完整貼進 context」這一步如果沒做會發生什麼？
  - 產出的投影片品質如何、可以怎麼改進？
-->

## Phase 2：探索 skills.sh

*進一步引入 [skills.sh](https://www.skills.sh) 上的社群技能，讓 agent 先「規劃」再執行，並改用 Slidev 產出簡報。*

### 安裝技能

[skills.sh](https://www.skills.sh) 是由 Vercel 維護的開源「技能目錄」，可以想成 **AI agent 的 App Store**：別人寫好的能力（技能），一行指令就能裝進你的 agent。這正是前面提到的「技能系統」——只是這些技能來自社群。

!!! info "`hermes skills install` 接受多種來源"
    同一個指令可以吃不同形式的識別碼，依技能來源而定（所以下面三個指令長得不一樣）：

    ```bash
    hermes skills install obra/superpowers                  # GitHub 短名 owner/repo
    hermes skills install official/security/1password       # 內建官方目錄
    hermes skills install skills-sh/vercel-labs/json-render # skills.sh slug
    hermes skills install https://github.com/.../SKILL.md   # 直接指向 SKILL.md 的網址
    ```

    加上 `--yes` 可略過確認提示，方便 agent 在自動化流程中免互動安裝。

本工作坊會用到這三個技能：

- [slidev](https://www.skills.sh/slidevjs/slidev/slidev) — 用 Markdown 撰寫、產生網頁式簡報。
- [ask-questions-if-underspecified](https://www.skills.sh/trailofbits/skills/ask-questions-if-underspecified) — 需求不明確時，讓 agent 先發問釐清再動手。
- [superpowers](https://www.skills.sh/obra/superpowers) — 一組通用工作流程技能（腦力激盪、系統化除錯、TDD 等）。

安裝指令如下（每個技能的來源不同，故格式不一；`--yes` 表示免互動安裝）：

```bash
hermes skills install https://github.com/slidevjs/slidev/blob/main/skills/slidev/SKILL.md --yes
hermes skills install https://github.com/trailofbits/skills/blob/main/plugins/ask-questions-if-underspecified/skills/ask-questions-if-underspecified/SKILL.md --yes
hermes skills install obra/superpowers --yes
```

安裝完成後，技能會出現在兩個地方：

**1. 輸入 `/` 時自動補全**

在輸入框打 `/` 再接技能名稱（例如 `/superpowers`），就會跳出建議清單，選擇後即可套用該技能。

![輸入 / 時，已安裝的 superpowers 技能會自動補全](images/skill-prompt.png){ width="380" }

**2.「技能與工具」分頁**

切到左側的「技能與工具」分頁，可以看到所有已安裝的技能、用搜尋框過濾分類，並用右側開關啟用或停用。

![技能與工具分頁中列出已安裝的 superpowers 技能，右側有啟用開關](images/skills-menu.png){ width="700" }

### 建立與修改研究計畫

*先請 agent 產出一份研究計畫，再逐步調整需求。*

依序給出這些指示：

1. Create a plan to research multi-agent systems for collaboration.
2. Update the plan: 不只涵蓋研究論文，也要納入「如何在真實世界實作 multi-agent 系統」的部落格文章。
3. 確認最終報告以**台灣繁體中文**呈現。
4. 以一份 **Slidev** 簡報作為最終交付成果。

<!-- TODO: 說明為什麼要先規劃再執行（ask-questions-if-underspecified / superpowers 的角色）。 -->

### 修改後的 Prompt

*在 Phase 1 的基礎上，加入「下載技能」與「先規劃」兩個步驟。*

```text
請用兩個 subagent 完成一份「解釋當前multi-agent 合作」教學投影片：

步驟 1 請下載這些技能：
https://www.skills.sh/trailofbits/skills/ask-questions-if-underspecified
https://www.skills.sh/obra/superpowers

步驟 2 請先研究計畫如何解釋當前multi-agent 合作

步驟 3 根據你的計畫派一個 Researcher（toolsets: web）：
搜集「LLM multi-agent 合作」的重點，回傳：5 個關鍵概念 + 每個一句說明
+ 3 篇代表性論文（標題/年份/連結。用條列回傳。
請將檔案存在/Users/amberber/Downloads/hermes-out

步驟 4 收到 Researcher 摘要後，把那份摘要「完整貼進」下一個 subagent的 context（因為它看不到前面的對話），派一個 Slide-maker
（啟用 PowerPoint skill，toolsets: 程式執行 + 檔案）：
根據摘要做一份 8 頁 .pptx：封面 + 5 個概念各一頁 + 參考文獻頁。
```

### 產出 Slidev 簡報

*說明 Slidev 與 PowerPoint 的差異，以及如何預覽／輸出最終簡報。*

<!-- TODO:
  - Slidev 是什麼、為什麼適合工程取向的簡報
  - 如何啟動預覽
  - 如何匯出（PDF / 網頁）
-->

---

*最後更新：2026 年 6 月*
