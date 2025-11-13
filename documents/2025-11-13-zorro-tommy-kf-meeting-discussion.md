---
title: "Knowledge Factory (KF) 項目討論 - Zorro & Tommy 會議"
date: 2025-11-13
tags:
  - type/meeting-minutes
  - topic/knowledge-factory
  - topic/mcp
  - topic/product-strategy
  - topic/team-collaboration
participants:
  - Zorro
  - Tommy
status: completed
access: private
---

# 會議紀錄：Knowledge Factory 策略討論

## 會議概要
- **日期**：2025-11-13
- **參與者**：Zorro (講者 1)、Tommy (講者 2)
- **主題**：Knowledge Factory (KF) 項目價值主張及團隊協調
- **時長**：約30分鐘

## 關鍵討論要點

### 1. Knowledge Factory 核心價值主張

Zorro 提出了 KF 的三個主要價值主張：

#### 價值一：資訊整合與易於存取
- 將零碎的資料和想法整合在一起
- 令資訊更容易記住和提取
- 實現方式：Shift+Command+Copy 功能，方便快速捕捉
- 相關指令：`/capture`

#### 價值二：數據擁有權與可攜性
- 使用 Obsidian + GitHub，所有數據都是通用 Markdown 格式
- 用戶擁有自己的數據
- 避免被供應商鎖定（vendor lock-in）

#### 價值三：AI 工具靈活性
- 可以在多個 AI 平台之間使用數據（ChatGPT、Claude、Gemini）
- 能夠整合知識而不會將所有東西都困在單一供應商
- 用戶可以自由地在不同 AI 工具之間移動數據

#### 價值四：知識分享（Tommy 補充）
- 容易分享精選的知識
- 以不同格式發布內容
- 兩個核心指令：`/capture` 和 `/publish`

### 2. 關鍵策略問題

Zorro 向團隊提出了一個根本性問題：

**"你想在未來五年花時間做什麼？"**

討論的選項：
- Knowledge Factory (KF)
- SmartGen
- VoiceBot
- MCP Proxy

核心見解：如果團隊成員不相信核心價值，就不應該勉強參與。這就是為什麼 Max 決定退出被視為積極的表現 - 清晰比強迫參與更好。

### 3. 產品定位與市場契合

#### 目標用戶人設
- 重視對工具的掌控和精通的用戶
- 可能使用 Obsidian 和 Claude Code
- 希望事情「在掌控之中」，有擁有感
- 欣賞知識管理的「mastership」（主導權）

#### 產品卓越性要求
如果將 KF 作為開源項目：
- 需要優秀的 GitHub 展示
- Plugin 命名需要改進（例如："Obsidian Vault Manager" 需要更名）
- 細節很重要，影響用戶體驗
- 需要市場推廣策略

### 4. 團隊協調挑戰

#### 當前團隊視角
- **Max**：表明不想繼續 - 被視為積極的清晰表態
- **Ross**：對 KF 開源方向說「不」；似乎在轉向 SmartGen 重點，提到了"Smart Search"概念
- **Chris (Kris)**：待確認；目前聽起來是正面的；需要更多時間去感受 KF 的價值

#### Master Concept (MC) 公司背景
- MC 本質上是 B2B 服務公司
- 但他們渴望做 B2C（市場）產品
- 創建了 Marketplace（Anderson 的項目）- 類似 Google Drive 市場
- 他們想「開法拉利，不只是奔馳」
- **Derek 和 Dennis**：MC 的老闆，尚未參與這些項目和開發的任何細節
- 正在計劃討論並向 Derek 和 Dennis 提出方案
- 如果價值主張引起共鳴，有協調的潛力

### 5. 技術架構討論

#### 當前 KF 架構
- 使用 Docker 作為 MCP Proxy
- Docker 的角色：主要用於 MCP Toolkit 功能
- 將所有 MCP 配置和工具放在 Docker 中
- 最近 Docker 4.5 升級包含動態 MCP Tools List
- 可能不用 Docker，只用 MCP Proxy 就能運作

#### 替代方案考慮
- 無論 KF 方向如何，MCP Proxy 都很重要
- KF 需要團隊顯著更多的努力和時間投入
- 與標準 B2B 項目相比需要不同的執行方法

### 6. 商業模式理念

Zorro 提出了一個基本問題：

**"你想走一條清晰賺錢的道路，還是找到很多支持者的道路？"**

觀察：
- 快速賺錢的項目往往變成定制工作
- 定制項目必須遵循客戶要求 → 更多修改/變更
- 這導致複雜性（就像 MC 的網絡架構沒有 UI）
- 不同的執行模式需要不同的工具

### 7. 數據擁有權缺口與團隊能力

Tommy 識別了一個關鍵缺口：
- 現有工具對於他們的願景來說不完整
- 討論中出現了安全性擔憂
- 提到了 AI/Airforce 考慮
- KF 可以幫助解決這些缺口

Zorro 關於團隊能力的見解：
- 過去某些專業知識領域（expert knowledge domain）難以掌握
- 現在有 AI 的幫助，相信團隊可以克服知識障礙
- 只要願意投入時間做研究，就能找到答案
- 重要的是找到合適的人（如 Alfred）來組建團隊

## 行動項目

### 團隊領導層
1. **明確「果籃」內容** - 向 Derek 和 Dennis (MC 老闆) 展示什麼具體價值主張
2. **進行價值協調會議** - 從團隊成員那裡獲得關於5年承諾的明確答案
3. **停止強迫參與** - 只包括真正認同願景的團隊成員
4. **定義清晰定位** - B2C vs B2B，支持者驅動 vs 收入驅動
5. **給 Chris (Kris) 時間** - 讓他有機會更深入理解和感受 KF 的價值
6. **理解 Ross 的立場** - 為何對 KF 開源方向說不

### 產品開發
1. **改進 plugin 命名和 UX** - 使捕捉指令更直觀
2. **制定市場策略** - 如果走開源路線
3. **記錄核心價值主張** - 基於今天的討論
4. **評估 Docker 依賴性** - KF 能否只用 MCP Proxy 運作？

### 下次團隊會議
1. **準備價值主張演示** - 給 Derek 和 Dennis (MC 老闆) 的「果籃」提案
2. **解決團隊成員擔憂** - 關於承諾的個別對話
   - Chris (Kris)：需要更多時間感受價值
   - Ross：了解他拒絕開源方向的原因
3. **定義成功指標** - 5年後的成功是什麼樣子？
/
## 關鍵引述

> "如果你本身都覺得張明，你唔好成日睇我睇 SmartGen 點睇呀，你自己睇 SmartGen 點睇"
> 「如果你自己已經看得清楚，不要總是看我怎麼看 SmartGen - 你自己看 SmartGen 怎麼看」

> "我哋自己搵唔到個價值，我哋就唔識 sell 㗎嘛"
> 「如果我們自己找不到價值，我們就不懂得怎麼賣」

> "你想食蘋果菠蘿定皮果... 你唔想食咁唔好放啦"
> 「你想吃蘋果、菠蘿還是提子... 你不想吃就不要放進去」

## 決策要點

### 延遲決策
1. 是否繼續完整的 KF 願景還是縮減到 MCP Proxy
2. 下一階段的團隊組成
3. 與 MC 和 GameC 的關係

### 待定輸入
1. 個別團隊成員的5年願景聲明
2. **Chris (Kris)** 對 KF 價值的最終確認
3. **Derek 和 Dennis**（MC 老闆）對 KF 方案的反饋
4. MC 對 B2C 產品方向的興趣程度

## 下一步

1. **停止當前 KF 討論**，直到達成團隊協調
2. **準備團隊對話**，關於個人承諾
3. **定義「果籃」內容**，準備向 Derek 和 Dennis (MC 老闆) 提出方案
4. **個別討論**：
   - 與 Chris (Kris) 討論，給他更多時間感受 KF 價值
   - 了解 Ross 對 SmartGen 的方向和為何拒絕 KF 開源方向
5. **準備 MC 高層提案**，向 Derek 和 Dennis 展示 KF 價值主張

---

# 關鍵術語與定義

## 產品與項目
- **Knowledge Factory (KF)**：正在討論的主要項目 - 使用 Obsidian + GitHub 的知識管理系統
- **SmartGen**：Ross 的現有項目 - 有客戶基礎的 B2B 平台
- **VoiceBot**：討論的替代產品選項
- **MCP Proxy**：Model Context Protocol 代理實現
- **Marketplace**：MC 的市場產品（由 Anderson 領導）

## 工具與技術
- **Obsidian**：使用 Markdown 的知識管理應用
- **Obsidian Vault Manager**：當前 plugin 名稱（需要重新品牌化）
- **MCP (Model Context Protocol)**：AI 工具整合協議
- **Docker**：用於 MCP Toolkit 的容器平台
- **GitHub**：版本控制和託管平台
- **Claude**：Anthropic 的 AI 助手
- **ChatGPT**：OpenAI 的 AI 助手
- **Gemini**：Google 的 AI 助手

## 關鍵指令
- `/capture`：捕捉內容到 vault 的指令
- `/publish`：從 vault 發布內容的指令
- **Shift+Command+Copy**：快速捕捉的鍵盤快捷鍵

## 組織與人員
- **Master Concept (MC)**：服務公司（B2B 重點，渴望做 B2C）
- **Derek 和 Dennis**：MC 的老闆，尚未參與項目細節，將收到 KF 提案
- **GameC**：客戶/合作夥伴組織
- **Zorro**：會議參與者（講者 1）
- **Tommy**：會議參與者（講者 2）
- **Max**：決定退出的團隊成員，已明確表態
- **Ross**：從事 SmartGen 的團隊成員，對 KF 開源方向說「不」
- **Chris (Kris)**：團隊成員，待確認，目前正面但需要更多時間感受 KF 價值
- **Anderson**：領導 Marketplace 項目
- **Alfred**：提到的良好團隊契合範例

---

# 修正後的會議記錄

## 講者 1 (Zorro)：
咁我哋而家就傾下 Knowledge Factory 呢個嘅核心 subject 係唔值得做落去。咁第一個呢，係可以將我哋好零碎嘅資料嘅 idea 可以放埋一齊，咁令到嗰個人呢覺得啲嘢容易去記住。咁從呢個 idea 呢我哋就諗到用一個叫做 Shift+Command+Copy 嘅嘢，就係可以令到一 copy 嘅嘢就擺落去，好似有 capture command。

第二個價值就是我們用了 Obsidian 和 GitHub，全部 data 都是 generic Markdown format，我們就可以控自己的 data，這個就不會被 vendor lock-in。

第三個價值就是因為我們現在我哋想將啲嘢搬嚟搬去，我哋可以搵 ChatGPT，搵 Claude，搵 Gemini，我哋都可以將佢整合成嘅嘢放返去自己嗰度，唔使 stall 哂所有嘢喺個 vendor 度。咁就系我而家睇到嘅嘢啦。咁你覺得仲冇啲咩價值？係咪呢？

## 講者 2 (Tommy)：
咁我 share 啲嘢出去嘛。

## 講者 1 (Zorro)：
係喇，我哋好容易去 share 到我哋想 share 嘅嘢出去。去 publish 出去，share knowledge 出去，可以產生不同的 format，令它製成不同的東西。最主要兩個 command 就是 copy capture、capture 同 share。

如果沒有我們就 KF discussion 停在這裏。其實這個 round 我想最重要跟你談的東西就 value proposition。我们暫時談到這裏。我們最重要是我們現在，我覺得明天他問開不開會，其實我就想不開會。Chris 說開30分鐘，但其實他們三個人都不會說話，都不會帶來。我覺得我如果下次要跟他講的就是將花紙，我決定要包果籃。咁即係件事點包，咁我即係我就要諗究竟最後放入個籃係咩囉。

而我諗我定向係向 master concept 導向嘅就係，我問返你嚟緊五年你想 spend 你嘅 time 做啲咩咁樣嘅，就即係呢個就係你嘅志向呀，你想做啲咩呢？呢係緊嗰五年係做啲咩呢？你會同佢一齊生活五年，五年啦，定係你會做個 KF，定係做 SmartGen，定係做 VoiceBot，你會同佢一路生存五年呢？當然平時你唔會諗嘅，你每日都係咁返工囉。但係而家叫你咁樣去諗一諗。

點解要咁諗呢？因為你我哋決定做果樣嘢，我哋嚟緊五年要 delay 我哋時間、心力去定好囉。

## 講者 2 (Tommy)：
你要湊一個 BB。

## 講者 1 (Zorro)：
如果那樣你看不到核心價值，所以我覺得 Max present 了他不想做是好的，因為他 present 了他不 share 同意價值，我不用盡情叫他負上責，拉上責，清晰就好了。沒關係的，你如果真的不想做就不要做，但不要盡情來嘛。

我哋可以唔喺呢度 wrap up 㗎，但我依家有一個好重要嘅 conversation 囉。

## 講者 2 (Tommy)：
咁但不過而家喺呢個階段覺得呢個係最好㗎，咁都手錶唔會變嘅。係呀，但係可能真係大家發現咗另一個，或者係 mint。

## 講者 1 (Zorro)：
咁我未有 KF 之前，我都應該做 MCP Proxy，我現在都覺得 MCP Proxy 要做。但 KF 要大家加倍努力，加倍投資時間。跑法都不同，哪裏 C 哪裏 B。

其實我要做一個靚的 Github，做一個靚的 open source Github 分享這個 plugin，我都要想怎樣炒作。譬如我現在的 plugin 名叫做 Obsidian Vault Manager 要改，我令到你打個 command 出來不是打 Obsidian Vault Manager Capture，那個都要執的。我唔知個 plugin 落咗你手會咁樣㗎嘛，我喺我嗰度真係就咁打 CAPTURE，嗰度要改㗎嘛。嗰啲細微細眼嘅嘢要執㗎嘛，好多。

即係會用 Obsidian、會用 Claude Code 嘅人，佢好啱用呀，好舒服呀，同埋件事 under control 呀。即係好有自己嗰個意欲想有一定嘅，嘅，叫咩呀，mastership 呀。Mastership 嗰件事係重點㗎嘛個 persona。

咁我哋嘅志向係邊？當我哋知道志向係邊，我哋個籃就會包得好囉。即係 at least 個籃人哋唔 buy，人哋唔要，人哋唔鍾意，我哋覺得我自己 sell 啱我哋嘅嘢囉。我哋只不過將個籃裏面嘅嘢再扭返去睇下點樣 sell，但係我哋自己搵唔到個價值，我哋就唔識 sell 㗎嘛。即係 Ross 都唔識 sell 㗎嘛。

所以正如我所講，呢一個 conversation 我哋可以喺度停咗去唔 wrap up 嘅。但係呢個係我哋我下一個同成條 team 嘅 conversation，係依賴住我哋最終放乜嘢喺個果籃度。果籃係給 GameC 個果籃，但係呢個果籃首先我哋自己想放啲乜嘢水果先。你想食蘋果、菠蘿定提子？

## 講者 2 (Tommy)：
你唔想食咁唔好放啦。如果兩個都唔想食，我們怎樣放？

## 講者 1 (Zorro)：
他們兩個都是果籃裏面的水果，我們決定他們不想食就連他們...

## 講者 2 (Tommy)：
因為他們都很專業。

## 講者 1 (Zorro)：
如果是這樣我們就不專業。我們會放，你們不放我們就不放你們。對，就是這樣。我們可以這樣去定位。

## 講者 2 (Tommy)：
嗯。

## 講者 1 (Zorro)：
不是因為我不喜歡你，是因為這件事大家不同。我覺得有價值在這些地方，你不覺得有，咁你可能 carry 唔到，咁你 carry 唔到我哋，所以我開頭我唔想做 influences。

如果你本身都覺得清楚，你唔好成日睇我睇 SmartGen 點睇呀，你自己睇 SmartGen 點睇。

## 講者 2 (Tommy)：
可能佢出現嘅時候可能係一個時機。

## 講者 1 (Zorro)：
時機係好嘅。又聽到 Ross 不斷轉換他的 codename。昨天他提及了 Smart Search，問 KF 是否 Smart Search。開會時，他閃過。其實他在想 SmartGen 如何進行到另一個階段，可能他現在在想 Smart Search 還是怎樣。他沒有開聲告訴我們。

他知道我沒有興趣聽 SmartGen 如何進行，因为在他們當中，使 SmartGen 強化，將特點、將一些 core value 推到那邊，而在他們的 platform，即是固定 user 推他們的 lead，這樣轉了過去。這樣 pivot SmartGen 是合理成立的一件事。因為已經有客底，他又賣得到，他的 lead 又回來了，將他推到另一邊就解決了。

## 講者 2 (Tommy)：
如果 KF 放入去，但本身 Derek and Dennis，MC 都唔係做 B2C，好似又唔係好啱嘅理念。

## 講者 1 (Zorro)：
呢個我想停一停，關於究竟 MC 係 B2C 定唔係 B2C。原來想停，MC 你老本唔係 B2C，但佢哋好想做 more。

## 講者 2 (Tommy)：
即係點解佢整咗個 Marketplace 出嚟，即係 Anderson 做咗個上網可以買陸續嘅 Google Drive。

## 講者 1 (Zorro)：
MC 係一間 servicing company，servicing company 個 foundation 已經好實㗎喇。

## 講者 2 (Tommy)：
即係做 solution。

## 講者 1 (Zorro)：
佢哋唔係淨係想，佢哋唔係淨係想揸奔馳，佢想揸法拉利。

## 講者 2 (Tommy)：
所以之前整 Goodhand。

## 講者 1 (Zorro)：
係咪。佢好想做呢啲嘢嘅，我哋嘅 nature 同佢做嘢嘢都成立。

## 講者 2 (Tommy)：
好似我哋都覺得有啲奇怪嘅。

## 講者 1 (Zorro)：
即係好想做啲 inbound 嘅嘢，佢唔好想做啲 inbound 嘅嘢，好想嗰啲嘢交流。正如 Max 所講，做 inbound 嘢做 B2C 係合理好多嘅。所以你問佢有冇興趣？有嘅，肯定比次唔知，興趣一定好濃厚。

但係你話如果你 sale 一個 B2C idea 比佢呢，係以返個 B2B，as long as 佢鍾意嘅話。但係我都唔係第二種考慮。

第一種考慮係我同你覺唔覺得我哋有啲核心價值喺 KF 裏面，或者係 MCP Proxy，或者喺 Obsidian 裏面我哋見到，我哋覺得呢件事係會成功嘅，我哋想佢成功嘅，我哋覺得呢件事會出現嘅，應該會出現嘅。咁我哋願意投資我哋自己落去。

然之後我哋就將呢個呢個咁嘅層次升華去組隊，組個強啲嘅隊，去搵資金去支持佢，成立咯。

其實正如你所說可能 Ross 心裏面嗰個，但係 B2B 嘅 core value 同埋佢嘅形式出現係想搭哂啲嘢做，係一個一出嚟就一個 bundle，fast & comprehensive 嘅一個 tools。我就唔想咁樣入，啲嘢都係佢嗰度，centralize 又等唔到嘅活性。我一拉上一條邊就不想談下去。

第一個就是理念。講得再簡單點，我想你自己去想，給一個 answer 我，你想走一個很清楚賺錢的方法，還是走一個很容易找到很多 supporter 的方法？就是 follow 你想行的層面呢？

我都好想搵一個好快搵到錢嘅嘢，但係好快搵到錢嘅嘢跟住就變咗 project 囉。因為你個金額要大就一定要跟住個客要求去變囉，咁就好多 cut 事囉。

## 講者 2 (Tommy)：
嗰個咪 MC 咁樣多啲囉。

## 講者 1 (Zorro)：
嗰個 architecture 嘅 router，如果那些是 networking，呢個都冇咁多 UI 嘅嘢俾你用呀，冇 UI 又冇得俾你住咗去 cut 呀。即係個 user 都用唔到，係 admin 用嘅啫嘛。

## 講者 2 (Tommy)：
可能有唔同嘅，佢哋要用唔同嘅 tools。

## 講者 1 (Zorro)：
同一個做法嚟嘅咋。如果 KF 唔用 Docker 呢用 MCP Proxy 搞掂㗎。聽唔聽得明呀？要裝 Obsidian 同埋要裝 Docker 嘅。Docker 成個位置係放 MCP Proxy，我冇用過 Docker 任何其他 feature，我就用 Docker 個 MCP Toolkit。

佢嘅作用就係將用一個 MCP 嘅 config 放哂所有 tools 畀佢，呢個就係 MCP Proxy 做嘅。我今日見到 Docker upgrade 咗4.5呢，系 dynamic MCP Tools List。

## 講者 2 (Tommy)：
Dynamic 系 based on 呢啲乜嘢？

## 講者 1 (Zorro)：
我唔知呀，我見到係咁。佢話呢，現在 natively 落 MCP Toolkit 嗰個 tools 係有四五個 tool 呢，畀你 list 有啲乜嘢 MCP tools。即係你一開始可能要做兩集，佢而家做嘢呢，佢就會先要去 Docker 度搵下你有啲乜嘢 tools provide 畀我，跟住再喺個 catalog 再揀嗰隻 fit 佢嘅 job。

可能多了一陣就慢了，但正如我所說 context 就會直銷了，不用 preload 所有 MCP tools 上去。所以我想在最後兩分鐘說，你覺得你自己想怎樣走這條路？

這條路可以有 SmartGen 份，可以有 VoiceBot 份，跟他們的條路不一致，可以只是 KF 和 MCP Proxy，改兩粒不要的，沒有他們的份。沒有他們的份，我們出來的自己做什麼？我們要怎樣 enforce team？再聽下去，不是沒有，我們沒有那個人的，我們做不到。

其實今天更容易說的就是因為有些 expert knowledge domain 我們 master 不到，現在有 AI 我不相信我們 master 不到，是忙而已。我浪費一點時間去做研究都能夠找到。

最重要是那些人。坦白說好像 Alfred 這樣好夾就可以了。

## 講者 2 (Tommy)：
一開始最原先都是覺得去 paste，但係不過呢係 existing truth 入面呢有啲嘢唔完善喎，都仲未係做到我哋想做嘅嘢。

## 講者 1 (Zorro)：
呢個就開 gap 囉。

## 講者 2 (Tommy)：
講返呢個 gap，咁你就想有一樣嘢，我哋發現咁嘅問題。其中一個方向就係 security 嘅問題，咁就係講到 AI Force。咁如果你話係啲，但係 KF 好幫忙。

## 講者 1 (Zorro)：
我一同你講少少嘢，我呢度就停咗錄音先啦。我哋呢度就完咗會議紀錄。

---

## 相關筆記
- [[Knowledge Factory Architecture]]
- [[MCP Proxy Implementation]]
- [[Team Alignment Strategy]]
- [[Product Value Propositions]]

## 後續行動
- [ ] 安排與團隊成員的個別5年願景對話
  - [ ] 與 Chris (Kris) 討論，給予更多時間感受 KF 價值
  - [ ] 了解 Ross 拒絕 KF 開源方向的具體原因
- [ ] 定義向 Derek 和 Dennis (MC 老闆) 演示的「果籃」內容
- [ ] 準備 MC 高層提案，展示 KF 價值主張
- [ ] 檢討和改進 plugin 命名和 UX
- [ ] 記錄 KF 解決的安全和數據擁有權缺口
- [ ] 評估 Docker vs 純 MCP Proxy 架構
