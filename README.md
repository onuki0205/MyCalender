# MyCalender
了解です、峻輔さん！ここまでの構想を整理して、アプリの全体像をまとめました。  
「ローン＋クレジット支払い管理 × Googleカレンダー連携 × 自分専用DB」という、実用性と拡張性を兼ね備えた設計です。

---

## 🧩 アプリ概要：個人金融管理ダッシュボード

### 🎯 目的
- ローンとクレジット支払いを一元管理
- 支払い予定をGoogleカレンダーに自動登録
- 支払い遅延を検知して警告表示
- PC・iPhone両方からアクセス可能（PWA対応）

---

## 🛠 技術スタック

| 領域 | 使用技術 | 補足 |
|------|----------|------|
| フロントエンド | Vue 3 + Vuetify | 美しいUIと柔軟なコンポーネント設計 |
| バックエンド | Firebase（Firestore, Auth, Functions） | データ保存・認証・ロジック処理 |
| カレンダー連携 | Google Calendar API | OAuth2認証でイベント登録 |
| デプロイ | Firebase Hosting | PWA対応でPC・iPhone両方に最適化 |

---

## 📦 主な機能一覧

### 🔹 ローン管理
- 金額・支払日・回数・残高の登録
- 完済までの進捗バー表示
- 支払い履歴のタイムライン表示
- 支払い遅延の警告表示（赤バナー）

### 🔹 クレジット支払い管理
- カード名・利用日・支払日・金額の登録
- 月ごとの支払い履歴表示
- 支払い遅延の警告表示
- カードごとのタブ切り替え・色分け表示

### 🔹 Googleカレンダー連携
- 支払い予定をイベントとして自動登録
- リマインダー通知設定（前日・当日）
- イベントタイトルに金額・支払い種別を明記

---

## 🧠 Firestore データ構造（概要）

```json
loans: [
  {
    id: "loan001",
    name: "車ローン",
    amount: 3000000,
    paymentDate: "2025-09-10",
    frequency: "monthly",
    totalPayments: 36,
    paidCount: 12,
    isDelayed: false
  }
]

creditPayments: [
  {
    id: "credit001",
    cardName: "楽天カード",
    amount: 12000,
    paymentDate: "2025-09-27",
    isPaid: false,
    category: "食費"
  }
]
```

---

## 🎨 UI設計のこだわり（峻輔さん向け）

- ローン・クレジットを切り替えるタブ式ダッシュボード
- 支払い履歴はタイムライン or カード表示
- 遅延時は赤バナー＋⚠️アイコンで警告
- 完済時は「祝・完済！」の演出（猫アイコンで祝福もあり🐾）
- 進捗バーは Vuetify の `<v-progress-linear>` を活用

---

## 🚀 次のステップ案

1. Firestoreの詳細設計（フィールド・型・構造）
2. Vueコンポーネントの設計（フォーム・一覧・警告）
3. Google Calendar APIの認証フロー構築
4. UIモックの作成（Figma or Vueで直接）

---

この構成なら、峻輔さんの「分析力 × UIこだわり ×実用性」が全部活きますね。  
次はどこから着手しましょう？UIモックを一緒に描いてみるのも楽しいと思いますよ！


データベース
📘 Firebase Firestore データベース設計書
プロジェクト名： ローン・クレジット支払い管理アプリ
対象技術： Firebase Firestore, Firebase Authentication, Vue.js, Firebase SDK
ロケーション： asia-northeast1（東京）

🔹 1. Firestore データ構造
1.1 ローン管理
|  |  | 
|  |  | 


フィールド構成：
|  |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 


サブコレクション：payments
|  |  | 


|  |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 



1.2 クレジットカード管理
|  |  | 
|  |  | 


フィールド構成：
|  |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 


サブコレクション：creditPayments
|  |  | 


|  |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 
|  |  |  | 



🔐 2. セキュリティルール（概要）
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /loans/{loanId} {
      allow read, write: if request.auth != null && request.auth.uid == resource.data.uid;
    }
    match /creditCards/{cardId} {
      allow read, write: if request.auth != null && request.auth.uid == resource.data.uid;
    }
  }
}



🛠 3. 使用技術と操作例（Vue + Firebase SDK）
Firestore 初期化
import { initializeApp } from "firebase/app";
import { getFirestore } from "firebase/firestore";

const firebaseConfig = {
  apiKey: "...",
  projectId: "...",
};

const app = initializeApp(firebaseConfig);
const db = getFirestore(app);


ローン追加例
import { collection, addDoc } from "firebase/firestore";

await addDoc(collection(db, "loans"), {
  name: "車ローン",
  amount: 3000000,
  startDate: "2025-01-10",
  paymentFrequency: "monthly",
  totalPayments: 36,
  paidCount: 0,
  uid: user.uid
});


支払い履歴追加（サブコレクション）
import { doc, collection, addDoc } from "firebase/firestore";

const loanDoc = doc(db, "loans", "loanId123");
await addDoc(collection(loanDoc, "payments"), {
  paymentDate: "2025-09-10",
  amount: 83000,
  isPaid: false,
  isDelayed: false
});



📍 4. ロケーション設定
|  |  | 
|  | asia-northeast1 | 
|  | asia-northeast1 | 
|  | asia-northeast1 | 




