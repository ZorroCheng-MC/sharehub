---
date: 2026-01-14
status: in-progress
project: ccf-shave-for-hope
type: technical-plan
related: "[[2025-12-31-ccf-shave-for-hope-firebase-studio-project]]"
---

# 剃亮希望 技術開發計劃
# Shave for Hope - Technical Implementation Plan

> 📋 高層次專案計劃請參閱 [[2025-12-31-ccf-shave-for-hope-firebase-studio-project]]

---

## 專案摘要 Project Summary

| 項目 | 內容 |
|------|------|
| **專案名稱** | 剃亮希望 Shave for Hope |
| **機構** | 兒童癌病基金 CCF Hong Kong |
| **活動日期** | 2026年3月7日 Head Shaving Day |
| **上線目標** | 2026年2月底 |
| **活動標籤** | #shaveforhopehk |
| **語言** | 繁體中文 + English |

---

## 目前狀態 Current State

| 組件 | 狀態 | 備註 |
|------|------|------|
| 8 個 UI 頁面 | ✅ 完成 | 使用模擬數據 |
| AI 圖片變身 | ✅ 運作中 | Gemini 2.5 Flash |
| Instagram 分享流程 | ✅ 運作中 | |
| shadcn/ui 組件 | ✅ 就緒 | 45+ 組件 |
| Firebase 後端 | ❌ 未整合 | 待開發 |
| 用戶認證 | ❌ 僅 UI | 待整合 Firebase Auth |
| 影片製作 | ❌ 未開始 | Veo 3.1 |
| 雙語支援 | ❌ 僅中文 | 待開發 |

---

## 用戶流程圖 User Flows

### Flow A: 訪客用戶 (無需登入)

```mermaid
flowchart TD
    subgraph ANONYMOUS["🌐 訪客用戶流程"]
        A[🏠 首頁<br/>/] --> B[查看示範<br/>前後對比滑桿]
        B --> C{點擊<br/>'開始變身'}
        C --> D[📸 變身頁面<br/>/transform]
        D --> E[上載照片<br/>拖放 / 相機]
        E --> F[⏳ AI 處理中<br/>2-5 秒]
        F --> G[✨ 查看結果<br/>光頭預覽]

        G --> H[📥 下載圖片]
        G --> I[📤 立即分享]

        I --> J[Instagram<br/>下載 + 複製文案]
        I --> K[WhatsApp<br/>wa.me 深層連結]
        I --> L[Facebook<br/>分享對話框]

        J --> M{💡 CTA:<br/>'想製作影片?'<br/>註冊製作影片!}
        K --> M
        L --> M
        H --> M

        M -->|是| N[前往 Flow B]
        M -->|否| O[結束 / 稍後回來]
    end

    style A fill:#FDF2C1
    style G fill:#90EE90
    style M fill:#F5A623
```

**分享模板:**
```
我為希望剃頭！🎗️ 支持兒童癌病基金
立即參與：shaveforhope.ccf.org.hk
#shaveforhopehk #剃亮希望
```

---

### Flow B: 註冊用戶 (Virtual Shaver)

```mermaid
flowchart TD
    subgraph SIGNUP["🔐 註冊流程"]
        A[從 Flow A CTA<br/>或直接訪問] --> B[認證頁面<br/>/auth]
        B --> C{選擇方式}
        C -->|Google| D[🔵 Google OAuth<br/>一鍵登入]
        C -->|Email| E[📧 電郵/密碼<br/>註冊]
        D --> F[首次用戶?]
        E --> F
        F -->|是| G[完成個人資料<br/>顯示名稱 必填<br/>IG 帳號 選填<br/>語言偏好]
        F -->|否| H[控制台]
        G --> H
    end

    subgraph DASHBOARD["📊 控制台"]
        H[控制台<br/>/dashboard] --> I[查看統計<br/>變身次數、分享數]
        I --> J{操作}
        J -->|新變身| K[前往 /transform]
        J -->|製作影片| L[影片流程]
        J -->|設定| M[編輯個人資料<br/>/settings]
    end

    subgraph VIDEO["🎬 影片製作"]
        L --> N[選擇語言<br/>廣東話 / English]
        N --> O[確認製作<br/>預覽前後照片<br/>預覽台詞]
        O --> P[⏳ 製作中...<br/>30秒 - 6分鐘]

        subgraph WAITING["等待畫面內容"]
            P --> P1[進度條]
            P --> P2[📅 Head Shaving Day 資訊<br/>2026年3月7日<br/>中環街市]
            P --> P3[💡 分享小貼士<br/>#shaveforhopehk]
        end

        P --> Q[✅ 影片完成]
        Q --> R[🎥 預覽影片<br/>8 秒短片]
        R --> S[📥 下載影片]
        R --> T[📤 分享影片]

        T --> U[IG Reels]
        T --> V[WhatsApp 動態]
        T --> W[Facebook]

        S --> X[🎉 感謝頁面]
        U --> X
        V --> X
        W --> X

        X --> Y{捐款?}
        Y -->|是| Z[↗️ 跳轉至<br/>CCF 捐款頁面]
        Y -->|否| AA[結束]
    end

    style H fill:#FDF2C1
    style Q fill:#90EE90
    style X fill:#F5A623
    style Z fill:#4A90D9
```

---

### Flow C: 回訪用戶

```mermaid
flowchart TD
    subgraph RETURNING["🔄 回訪用戶流程"]
        A[任何頁面] --> B{已登入?}
        B -->|是 - Session| C[自動跳轉至<br/>控制台]
        B -->|否| D[顯示登入按鈕]

        C --> E[控制台<br/>/dashboard]
        E --> F[查看進度<br/>• 籌款統計<br/>• 過往變身<br/>• 分享次數]

        F --> G{操作}
        G -->|變身| H[新變身<br/>/transform]
        G -->|影片| I[製作新影片]
        G -->|個人資料| J[編輯設定<br/>/settings]
        G -->|分享| K[分享過往作品]
    end

    style C fill:#FDF2C1
    style E fill:#90EE90
```

---

### 完整用戶旅程概覽

```mermaid
flowchart LR
    subgraph ENTRY["入口點"]
        A1[社交媒體連結]
        A2[直接 URL]
        A3[朋友分享]
    end

    subgraph ANONYMOUS["訪客"]
        B[首頁] --> C[變身]
        C --> D[分享圖片]
    end

    subgraph REGISTERED["註冊用戶"]
        E[註冊] --> F[個人資料]
        F --> G[控制台]
        G --> H[製作影片]
        H --> I[分享影片]
    end

    subgraph CONVERSION["轉換"]
        J[捐款給 CCF]
        K[出席活動<br/>3月7日]
    end

    A1 --> B
    A2 --> B
    A3 --> B

    D -->|CTA| E
    D --> J
    I --> J
    I --> K

    style D fill:#F5A623
    style I fill:#F5A623
    style J fill:#4A90D9
    style K fill:#90EE90
```

---

## 頁面地圖 Page Map

| 路徑 | 頁面 | 認證 | 描述 |
|------|------|------|------|
| `/` | 首頁 | 否 | 著陸頁、示範、統計、CTA |
| `/transform` | 變身 | 否 | 上載照片 & AI 變身 |
| `/auth` | 登入/註冊 | 否 | Google OAuth + 電郵/密碼 |
| `/dashboard` | 控制台 | 是 | 用戶統計、歷史、製作影片 |
| `/settings` | 設定 | 是 | 編輯個人資料 |
| `/leaderboard` | 排行榜 | 否 | 籌款排名 |
| `/u/[userId]` | 公開個人頁 | 否 | 可分享的籌款頁面 |

---

## 個人資料欄位 Profile Fields

| 欄位 | 必填 | 時機 |
|------|------|------|
| 顯示名稱 | ✅ **必填** | 首次註冊 |
| 電郵 | ✅ 自動 | OAuth/註冊時 |
| Instagram 帳號 | ❌ 選填 | 設定頁面 |
| 籌款目標 | ❌ 選填 | 設定頁面 |
| 個人故事 | ❌ 選填 | 設定頁面 |
| 語言偏好 | ❌ 選填 | 設定 (預設: 繁中) |

---

## 社交分享矩陣 Social Sharing Matrix

| 平台 | 圖片 | 影片 | 方式 |
|------|------|------|------|
| **Instagram** | ✅ | ✅ | 下載 + 複製文案 |
| **WhatsApp** | ✅ | ✅ | `wa.me/?text=` 深層連結 |
| **Facebook** | ✅ | ✅ | FB 分享對話框 |
| **複製連結** | ✅ | ✅ | Clipboard API |

---

## 開發階段 Implementation Phases

### Phase 1: Firebase 基礎建設 (優先級: 高)

**新增檔案:**
```
src/lib/firebase/
├── config.ts          # Firebase 應用程式初始化
├── auth.ts            # 認證輔助函數
├── firestore.ts       # CRUD 操作
├── storage.ts         # 檔案上載函數
└── types.ts           # TypeScript 類型

src/context/
└── AuthContext.tsx    # 認證狀態 Provider

src/hooks/
└── useAuth.ts         # 認證 Hook
```

**需修改檔案:**
- `src/app/auth/page.tsx` - 連接 Firebase Auth
- `src/app/layout.tsx` - 加入 AuthProvider
- `src/components/layout/header.tsx` - 真實認證狀態

**Firestore Schema:**
```
users/{uid}
├── displayName: string (必填)
├── email: string
├── avatarUrl: string?
├── instagramHandle: string?
├── fundraisingGoal: number?
├── personalStory: string?
├── language: 'zh' | 'en'
├── createdAt: timestamp
└── updatedAt: timestamp

transformations/{id}
├── userId: string
├── originalImageUrl: string
├── transformedImageUrl: string
├── videoUrl: string?
├── videoStatus: 'pending' | 'processing' | 'complete' | 'failed'
├── videoLanguage: 'zh' | 'en'
├── shareCount: number
├── isPublic: boolean
└── createdAt: timestamp
```

---

### Phase 2: 核心功能 (優先級: 高)

**社交分享 (`src/lib/share.ts`):**
- Instagram: 下載圖片 + 複製文案
- WhatsApp: `wa.me/?text=` 深層連結
- Facebook: FB 分享對話框
- 複製連結: Clipboard API

**分享模板:**
```
我為希望剃頭！支持兒童癌病基金 🎗️
立即參與：{url}
#shaveforhopehk #剃亮希望
```

**需修改檔案:**
- `src/components/transform-form.tsx` - Storage 整合、分享按鈕
- `src/app/dashboard/page.tsx` - 真實 Firestore 數據
- `src/app/u/[userId]/page.tsx` - 從 Firestore 讀取公開頁面
- `src/app/leaderboard/page.tsx` - 真實排名
- `src/app/settings/page.tsx` - 儲存至 Firestore

**新增組件:**
```
src/components/
├── share-buttons.tsx       # IG, WhatsApp, FB, 複製
├── event-info-card.tsx     # Head Shaving Day 資訊
└── countdown-timer.tsx     # 距離 3月7日 倒數
```

---

### Phase 3: 影片製作功能 (優先級: 中)

**技術:** Gemini Veo 3.1 圖片轉影片

**方法:** 使用描述性提示詞的圖片轉影片:
- 輸入: 有頭髮的照片 + 光頭照片
- 提示詞描述: 優雅的變身 + 說出台詞
- 讓 Veo 處理動畫和語音生成

**新增檔案:**
```
src/ai/flows/
└── generate-transformation-video.ts

src/components/
├── video-generator.tsx
└── video-waiting-screen.tsx
```

**影片規格:**
- 開始: 原始照片 (有頭髮)
- 結束: 光頭照片
- 動態: 優雅的頭髮到光頭變身
- 語音: 說出「剃亮希望。我支持！」或 "Shave for Hope. I support!"
- 時長: 約 8 秒
- 用戶選擇: 廣東話或英語

**示範提示詞:**
```
Create a video showing a person gracefully transforming from having
hair to a shaved head. The person should speak [SCRIPT] at the end
of the video while smiling confidently. The transformation should
feel hopeful and empowering.
```

**等待時間內容 (30秒-6分鐘):**
- 顯示 Head Shaving Day 活動資訊
- 中環街市位置
- CCF 使命故事
- 進度指示

**風險緩解:**
| 風險 | 緩解措施 |
|------|---------|
| 處理時間長 | 隊列系統、等待時顯示活動資訊 |
| 成本高 | 限制次數 (每用戶3條)、僅限註冊用戶 |
| 質量不一 | 允許重試、分享前預覽 |

---

### Phase 4: 雙語支援 (優先級: 中)

**新增檔案:**
```
src/lib/i18n/
├── index.ts
├── zh.ts              # 繁體中文
└── en.ts              # English

src/context/
└── LanguageContext.tsx

src/components/
└── language-toggle.tsx
```

**需修改檔案:**
- `src/lib/constants.ts` - 重構為 i18n
- `src/components/layout/header.tsx` - 加入語言切換

---

### Phase 5: 上線準備 (優先級: 高)

- [ ] 從 ccf.org.hk 擷取 CCF 品牌素材
- [ ] SEO meta 標籤和 OG 圖片
- [ ] 手機優化測試
- [ ] 錯誤監控設置
- [ ] 效能優化
- [ ] 部署至新 GCP 專案

---

## 環境變數 Environment Variables

```env
# Firebase (從新 GCP 專案)
NEXT_PUBLIC_FIREBASE_API_KEY=
NEXT_PUBLIC_FIREBASE_AUTH_DOMAIN=
NEXT_PUBLIC_FIREBASE_PROJECT_ID=
NEXT_PUBLIC_FIREBASE_STORAGE_BUCKET=
NEXT_PUBLIC_FIREBASE_MESSAGING_SENDER_ID=
NEXT_PUBLIC_FIREBASE_APP_ID=

# Google AI
GOOGLE_API_KEY=

# App
NEXT_PUBLIC_APP_URL=
NEXT_PUBLIC_CCF_DONATION_URL=https://ccf.org.hk/zh-hant/support/donation/
```

---

## 檔案摘要 File Summary

**新增檔案 (26):**
- 5 個 Firebase lib 檔案
- 2 個 Context providers
- 2 個自訂 hooks
- 3 個 i18n 檔案
- 3 個 Server actions
- 1 個 AI flow (影片)
- 7 個組件
- 1 個分享工具
- 1 個 .env 模板

**需修改檔案 (12):**
- `src/app/auth/page.tsx`
- `src/app/dashboard/page.tsx`
- `src/app/u/[userId]/page.tsx`
- `src/app/leaderboard/page.tsx`
- `src/app/settings/page.tsx`
- `src/app/layout.tsx`
- `src/app/transform/page.tsx`
- `src/components/transform-form.tsx`
- `src/components/layout/header.tsx`
- `src/lib/constants.ts`
- `src/lib/types.ts`
- `next.config.ts`

---

## MVP vs 加分項 MVP vs Nice-to-Have

**MVP (必須上線):**
1. ✅ Firebase Auth (Google + Email)
2. ✅ 圖片變身 + Firebase Storage
3. ✅ 社交分享 (WhatsApp + 複製連結 最低要求)
4. ✅ 基本控制台配合真實數據
5. ✅ 活動資訊組件

**加分項 (上線後可補):**
1. 影片製作 (如 Veo 3.1 就緒)
2. 完整雙語切換
3. 排行榜篩選功能
4. 公開籌款頁面

---

## 外部依賴 Dependencies on External Teams

1. **CCF**: 品牌素材審批、捐款頁面 URL
2. **GCP**: 新專案憑證
3. **捐款頁面團隊**: 籌款目標頁面 (範圍外)

---

## 技術棧 Tech Stack

| 類別 | 技術 |
|------|------|
| **框架** | Next.js 15 + React 19 |
| **語言** | TypeScript |
| **樣式** | Tailwind CSS |
| **UI 組件** | shadcn/ui |
| **AI 圖片** | Gemini 2.5 Flash (gemini-2.5-flash-image-preview) |
| **AI 影片** | Gemini Veo 3.1 (待整合) |
| **AI 框架** | Google Genkit |
| **後端** | Firebase (Auth, Firestore, Storage) |
| **表單** | React Hook Form + Zod |
| **圖標** | Lucide Icons |

---

## GitHub Repository

**URL:** https://github.com/ZorroCheng-MC/shaveforhope

**分支:** main

**開發指令:**
```bash
# 安裝依賴
npm install

# 啟動開發伺服器 (port 9002)
npm run dev

# TypeScript 類型檢查
npm run typecheck

# 生產環境建置
npm run build
```

---

*文件版本: 1.0 | 建立日期: 2026-01-06 | 最後更新: 2026-01-06*
