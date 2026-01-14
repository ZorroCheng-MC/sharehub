---
title: "企業 AI 培訓及 Google Workspace 銷售會議"
tags: [idea, AI, productivity, business, tools, Google, workflow, actionable]
date: 2026-01-14
type: idea
status: inbox
priority: high
client: 製造業公司（沙田）
---

# 企業 AI 培訓及 Google Workspace 銷售會議

## 會議概要

**日期**：2026年1月3日（記錄日期：2026年1月14日）
**性質**：銷售探索會議 — AI 培訓及 Google Workspace 遷移
**客戶概況**：香港製造業公司，香港約 20-30 人，內地設有生產廠房

---

## 客戶需求摘要

### 現況
- **員工概況**：香港 20-30 人，電腦知識水平一般
- **現有電郵**：On-premises Microsoft Exchange（已用 10 年）
- **ERP 系統**：SAP S/4 HANA（On-premises）
- **痛點**：會計部 paperwork 過多（報表、客戶訂單、單據處理）
- **地點**：沙田（香港營運）+ 內地（生產）

### 期望成果
1. **AI 生產力工具**：學習用 AI 整報表、整 PowerPoint
2. **減少 paperwork**：會計部單據電子化、bookkeeping 流程改善
3. **電郵遷移**：升級至 Google Workspace (Gmail)
4. **AI 功能應用**：
   - Google Meet AI 會議摘要（廣東話支援重要）
   - RAG chatbot 搜尋電郵
   - NotebookLM 文件分析

---

## 建議方案

### 培訓套餐

<table>
<thead>
<tr><th>套餐</th><th>時數</th><th>內容</th></tr>
</thead>
<tbody>
<tr><td>基本</td><td>4 小時</td><td>NotebookLM + Gmail 基礎使用</td></tr>
<tr><td>進階</td><td>8 小時</td><td>全面 AI 培訓</td></tr>
</tbody>
</table>

**授權結構**：
- 每個 bundle 包含 15 個 license
- 可額外購買 license，培訓人數可超過 15 人
- 培訓內容可按需求 customize

### 升級路徑
1. **基礎 Bundle** → 包含 NotebookLM
2. **Gemini Enterprise 升級** → 加差價升級，功能更強
3. **Google Workspace** → 獨立產品，需另外報價

### 兩張報價單
1. **報價單 1**：AI 培訓 Bundle + Gemini Enterprise 升級
2. **報價單 2**：Gmail 遷移服務（由專責團隊處理）

---

## 技術考量

### 電郵遷移
- **現狀**：On-premises Exchange，10 年電郵數據，每月約 30GB
- **建議**：
  - ❌ 不建議將舊電郵搬上 Gmail（「好容易爛」）
  - ✅ 保留 Exchange 做 archive，供日後 retrieve
  - ✅ 新舊系統並行 1-2 個月（兩邊都收）
  - ✅ 之後只用 Gmail 收新電郵
- **客戶痛點**：現有 Exchange 搜尋舊電郵非常慢

### SAP 整合
- **現狀**：SAP S/4 HANA On-premises，內部自行維護
- **挑戰**：Google 暫時無 SAP connector（API 連接器）
- **原因**：SAP 未上雲，Google 服務全在雲端，無法直接對接
- **臨時方案**：每日從 SAP export CSV → 放入 Google Drive → AI 分析
- **長遠方案**：需做 System Analysis Study（另一個項目）

### 內地使用
- **要求**：需安裝專線（合規跨境互聯網）
- **範圍**：今次先 focus 香港營運

---

## 時間表及行動項目

### 目標時間表

<table>
<thead>
<tr><th>里程碑</th><th>日期</th></tr>
</thead>
<tbody>
<tr><td>交付報價單</td><td>本週內（1月3-10日）</td></tr>
<tr><td>客戶簽署</td><td>兩週內</td></tr>
<tr><td>項目啟動</td><td>1月中</td></tr>
<tr><td>首次培訓</td><td>農曆新年前（2月）</td></tr>
</tbody>
</table>

### 行動項目

**我方**
- [ ] 準備 Gmail 遷移報價單（由專責同事處理）
- [ ] 準備 AI 培訓 Bundle + Gemini Enterprise 升級報價單（Daniel 跟進）
- [ ] 定義 4 小時培訓範圍
- [ ] 為 Gmail 遷移開獨立 case

**客戶**
- [ ] 與老闆商討並取得批准
- [ ] 提供具體需求（例如「將單據變成表」）
- [ ] 確認用戶數量（15 人 vs 20+ 人）
- [ ] 確認舊電郵 archive 需求

---

## 關鍵洞察

### 銷售機會
1. **升級路徑**：基礎 Bundle → Gemini Enterprise → 完整 Google Workspace
2. **附加服務**：Exchange 至 Gmail 遷移（獨立團隊/報價）
3. **未來機會**：SAP 雲端遷移以實現完整 AI 整合

### 客戶顧慮
- 員工電腦知識有限（需 user-friendly 方案）
- 廣東話 AI 功能支援
- 舊電郵搜尋速度慢
- 成本及審批時間

### 已提供建議
- 舊電郵不遷移至 Gmail，保留 Exchange 做 archive
- 以 15 人 pilot 開始，跨部門抽選
- 用 CSV export 作為 SAP 數據橋樑
- 建議先上 Gmail，再學進階 AI 功能

---

## 會議重點語錄

> 「員工對電腦嘅認識就可能唔好咁好」

> 「會計部佢哋會將一啲報表傳來傳去，或者客人嘅單」

> 「廣東話類比十分之好」（客戶關注 Cantonese 支援）

> 「SAP 可以唔使住先，暫時我哋都係想用 Google Workspace 相關產品」

> 「舊 email 就真係唔建議你搬返上去，因為咁樣呢件事係會好容易爛」

---

**完整逐字稿：**
咁我哋嘅員工大概都係20口30人咁樣嘅

咁就

佢哋對電腦嘅認識就可能

唔好咁好嘅

咁我哋而家老闆就想佢哋用一啲生產力嘅工具啦

即係同AI去合作去整啲

即係例如整啲報表呀,整啲PowerPoint呀

想他們學會用AI

有沒有短期有什麼想做給

哪個department 有什麼流程可以做出來

暫時沒有想做給哪個department

流程方面就想

多時都是paperwork比較多

想有個system

去減少他們的paperwork

可不可以講一講你所謂的paperwork是哪類的document

例如

會計部他們

會計部他們會將一些報表傳來傳去,或者客人的單

咁係咪如果我就咁一路你講乜我就聽咩嘅

咁我估第一個想去

想去使用嘅或者enable department就係會計部

可唔可以咁去理解

可以

可以

咁即係將會計部嘅而家嘅單據嘅

點樣去做我哋叫bookkeeping嘅嘢

就變成一啲

數表同埋

一啲

用電子處理多少少嘅嘢啦

我明白

有冇話係,我講咗bookkeeping啦,但係唔係準確,定你有其他嘅

唔同種類嘅operation會

個process會更加係想evolve埋呢度

因為

暫時我哋都係好初步想

試下用AI,好似Google Workspace上面的功能都想試下用

例如Google Meeting,可以用AI去做summary,是不是有些這樣的東西

但是廣東話類比十分之好

或者

之後我們可能會想將我們的email升去Google

跟住我們看Google有一個

有個RAG有個checkbot可以

用AI幫我們找email

都想做這方面的

是有的

你們現在我剛剛去網站看過了,你們的地址就在沙田城

全街127號

因為香港沒有什麼廠

我經常都認住廠,就rework了大陸的operation

咁你地冇大陸的operation嘅?

有嘅有大陸嘅

我想問下

大陸會唔會用到google服務呢?

都可以

要裝專線

要裝那片線

專線

專線

合規嘅那個

因為當來聯網嘛,專線係來聯網嘅

哦,明白

我哋都有間菜喺大陸嘅

香港就做Operation,大陸就做生產

咁今次我哋可唔可以暫時係當group咗喺香港先啦

可以呀可以呀

咁大概你香港有幾多人到呀

大概20至30人到

20至30人到

咁你想今次如果呢個項目係全部20至30人都involved呀,定係

一部分嘅人先行咗先

先啦可能你先睇你哋嘅Course話可以15個User

即係想試下15個User先啦即係我哋抽15個User

喺唔同嘅Department

咁好好呀咁如果咁樣

咁

其實啦我諗你

若果咁樣啦

如果你都仲記得我哋package啦

我哋package就唔同嘅功能呢就基本上可以集埋一齊就少一個

培訓啦咁我哋會跟埋notebook lm洗個bundle裏面啦

等你哋可以用到嘅

咁但係你頭先就有講過你哋想用埋google嘅gmail

Google workspace

係

冇緊係

我想問下

如果

整個enterprise係花埋notebook lm

如果你想再用心一點就用German Enterprise

就由我們現在的Bundle幫你

再升級到Notebook LM

加回差價

可以

但是German Enterprise都不跟Google Workspace,都是兩個產品

兩個產品

通過Sandy資源

平時你又用開Gmail

是的

我們現在的產品叫做Google Workspace

就是收費版的Gmail

所有Gmail的Google Drive、Google Meet、Google Doc

都包裝了,但是都是收費的

這個就是Google Workspace的地方

咁而Germanize,我哋嘅window就BioNobo LM啦,你而家想再

上去German Enterprise

咁嗰度就要再加錢

加15個license入去

咁但係如果你全個香港想20人呢,咁就最

我哋係跟15個license㗎嘛,你可以買到

20幾個license再upgrade上去都得

如果你買夠license,其實個training去到20幾人,我哋都ok嘅

咁只不過我哋想話畀你聽,如果你

我哋可以提供嘅license,跟個bundle就係15個

你加license,多啲人上課,我哋唔介意

可以可以

我再同我老闆傾下

咁其實我諗

如果你

有最好啦,我哋個bundle

我哋有四個鐘八個鐘嘅唔同嘅bundle

咁其實淨係做Balance都有

四個鐘啦咁樣講啦

咁嗰四個鐘就會教你點用

用個

用個Notebook LM去做到一啲嘅AI嘅功能啦

咁但係由於你地連Gmail都未開始有啦

咁我哋都咁而我們可以正是圍繞住Balance嘅範疇

又唔係講Notebook LM就講一啲基本嘅Gmail嘅使用同埋點樣同佢

相關嘅Germany Enterprise嘅使用

因為你用German Line Enterprise用到嘅嘢就

可以在German Line Enterprise加上

Gmail去

做一啲嘅

交

交錯使用嘅

因為German Line Enterprise可以睇到Gmail

即係都建議我哋上咗Gmail先

將啲嘢

跟住先

學Costco

問下我同事

咩叫e-mail

我哋用緊on-premises exchange

On-premises exchange

我都有條team係幫手做呢啲migration嘅

我哋可以

叫佢提供返相關嘅費用同埋啲

Quotation畀你哋

可以可以

咁其實對於我哋嚟講嗰個scope of work

咁我哋可以再propose四個鐘裏面可以做啲乜嘢俾你

你哋ok咁就

其實可以兩張quotation跟開簽嘅

一張你先簽,再一張upgrade先

然之後當你哋

睇下我哋個training scope係乜

咁finall咗,你哋想要四個鐘變八個鐘

都無所謂嘅

最重要是你想學的內容

想在那四個鐘包含什麼

沒問題,即是可以customize那些

可以的

個module裏面每一個item都可以

按你需要去改的

譬如因為你們用German Enterprise,我們就可以based on German Enterprise的功能去改給你們

但你們最好去

即是如果你們有

有期望的

將一些單據變成一些的表

你們就這樣寫下,這是一個希望得到的東西

希望包括的,我們在訓練裡面一定包括

包括了這些東西

可以可以

我們本身有用SAP的東西

想知道SAP和Google有沒有AI的解決方法

我的理解呢,Google Germanized Enterprise

可以connect很多systems

但是我的理解

Germanized Enterprise今日

就沒有

一個SAP,我們叫connector

如果你知道什麼叫connector,就是一些

API

令到系統之間可以互相的聯絡這樣的東西

這個我就照我的,因為他們更新得很快的

所以我現在一直跟他們說,我都想立刻查一查

究竟那個連接器出現了沒有

那個連接器如果出現了,那就有了

但是如果要做,我想問你SAP是怎麼說呢

是有哪一家公司和你們去開發和維持的

因為都很久之前了,可能資料會回到同事

已經沒有人再跟了

我現在自己搞,維持一下,migrate upgrade那些,內部搞

是,但SAP你們是on-prem還是

on-prem

on-prem

如果on-premise就未必接到的

未必接到,我們用S4 HANA的版本

最重要是因為你沒有一個

放在web的地方

即是沒有上cloud

我們google的全部在雲端

明白

你要那個都在雲端先得囉

這些就是我們稱之為integration service

這樣就算

這樣東西就會有個

要跟你做system analysis study的

這個可以找同事

但是就不是一些很

我們這次這個program所有東西

其實都是希望這件事

給到客戶用的

現在所有的東西,我想提醒你

未必是即時

因為要做Analysis、做Consultation

這些東西,未必會很快做得到,如果你一搭上SAP的期望

哦,SAP可以不到住线,即是暂时我们都是想用Google Workspace,相关产品

那些线,因为你现在内部就没有这样的生产力工具

但是你们有一个简单的东西就可以即时做到的,就是如果你们SAP有一些

有一些的data是可以每日export出来一个

CSV

咁就

咁就叫offline connection

即是batch update,咁我哋呢啲就可以收喺個AI度

哦好

即係譬如放,將佢變成google super,放喺google drive度,咁就得

咁件事就通

哦好啊,我見解下先呢個

咁我大概明白你就需要,你有沒有覺得個time frame係

點樣行法

Make progress

我想

大概一個月內

做唔做到

即係你頭先講嗰啲

AI

AI training

做到嘅

但係你要兩個禮拜落簽

咁我要同我老闆傾下先

你簽嘢我哋先開工做嘢

係咪要耐啲呀

其實都好簡單

因為我哋每個月嘅email數量

即係每個月都有30GB的email數量

即係呢個size呀,我唔知做upgrade會唔會好耐

做mygrade會唔會好耐

一個月30G

整個公司嘛

係咪

唔多

好,我先問一下

因為我們可能

想賣10年

10年的

10年的email

這班我不是專家,但是我們通常就不建議你將email搬上去GNL

我們建議你留回你的local exchange做一個archive,讓你們做retrieve

分開兩個系統做,一個是舊Email,一個是新Email

中間有一段Panel的階段,譬如一個月至兩個月

兩邊都收的

因為我們現在有個情況就是

想找回舊的Email,可能想找回三四年前的Email

但我們現在會,找回舊Email再找,就很慢

找個Exchange的系統就很慢

用gmail會快點嗎?

舊email就真係唔建議你搬返上去

因為咁樣呢件事係會

好容易爛

你留返舊系統度

將佢如果我唔係專家呀不過從呢個方面講少少

如果你

你將嗰個archive分

唔同嘅

我哋嗰個叫咩呀

即係

咁你自己就分別唔同年份搵唔同

唔同file

開一part我等我最熟知咩同事同你傾

好啊好啊無問題

咁即係

一個月係絕對可能嘅但係你都要預你自己所有嘅approval係兩個禮拜內

給我哋囉

好好,我盡快同我老闆傾傾

咁我哋不如target今個禮拜我哋

畀你啲quotation畀你攞曬proposal先

攞曬approval先

一般你可以搵我哋再攞detail囉

好啊,冇問題

咁我諗大致上我第一輪文件

想收集嘅資料都差唔多㗎,唔知你有沒有啲咩能提供呢

暫時都冇呀

都清楚

好啊,不如我將今天我們聊的事情,盡快

有些action item,就是找我們Google

做Gmail的同事跟你們

跟一跟個case,去開個個案,我要個別同你開個call

Daniel會將你

那些training想做的

item,整理一下,然後將

加上Germanite Enterprise的upgrade的bundle,就send給你,作一個 quotation

這樣

總之盡快有兩張quotation在你手上

你们尽快签给我们,我们就target两个礼拜后

即是今天是

1月3日

之前

你就签给我们,我们就target

月中,当中有些农历新年好像是2月尾的,我们在农历新年前

就基本上有第1个brush up做了,如果Gmail搞得定的话

明白

好,那我們再跟進吧

好啊

感謝Cam,我們下次見

拜拜



**記錄日期**：2026-01-14
**來源**：會議逐字稿（Google Chrome capture）
**跟進**：等待報價單，安排審批會議
<!-- Trigger rebuild -->
