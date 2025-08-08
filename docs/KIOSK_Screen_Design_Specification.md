# KIOSK画面設計書

## 1. 概要

本書は、Round1 Food Hall KIOSKシステムの画面設計仕様を定義します。各画面の詳細なレイアウト、UI要素、動作仕様を記載し、HTML/CSSによる実装を可能にすることを目的とします。

## 2. 画面一覧

1. Screen Saver（スクリーンセーバー）
2. Here or To Go（店内/テイクアウト選択）
3. Main Menu（メインメニュー画面）
4. Detail Menu（商品詳細画面）
5. Pop-up（アップセル画面）
6. Order Summary（注文サマリー）
7. Customer Field（顧客情報入力）

## 3. 共通仕様

### 3.1 画面サイズ
- **縦向き**: 540px × 960px
- **タッチ対応**: 全ての操作要素は指での操作を前提

### 3.2 色彩パレット
```css
:root {
  --primary-red: #dc3545;
  --primary-orange: #fd7e14;
  --primary-green: #28a745;
  --primary-blue: #007bff;
  --background-white: #ffffff;
  --text-dark: #212529;
  --text-light: #6c757d;
  --border-gray: #dee2e6;
}
```

### 3.3 フォント仕様
- **主要フォント**: Arial, sans-serif
- **見出し**: 24px - 32px
- **本文**: 16px - 18px
- **注記**: 12px - 14px

### 3.4 ボタン仕様
- **最小高さ**: 60px
- **最小幅**: 120px
- **フォントサイズ**: 18px
- **タッチエリア**: ボタンサイズ + 10px余白

## 4. 画面別詳細仕様

### 4.1 Screen Saver（スクリーンセーバー）

#### レイアウト構成
```
+----------------------------------+
|          [ブランドロゴ]           |
|                                  |
|          ORDER HERE              |
|                                  |
|     TOUCH SCREEN TO BEGIN        |
|                                  |
|        [ラーメン画像]             |
|                                  |
| Rotate menu photos with          |
| restaurant's logos               |
+----------------------------------+
```

#### UI要素詳細

| 要素名 | 内容 | スタイル |
|--------|------|----------|
| ブランドロゴ | 日本語ロゴ + "YUU" | 上部中央配置、高さ100px |
| メインメッセージ | "ORDER HERE" | font-size: 48px, font-weight: bold |
| サブメッセージ | "TOUCH SCREEN TO BEGIN" | font-size: 24px, color: --text-dark |
| 商品画像 | ラーメンボウル写真 | 幅: 400px, 高さ: 300px, border-radius: 10px |
| フッターテキスト | "Rotate menu photos..." | font-size: 14px, color: --text-light |

#### 動作仕様
- タッチ操作で次画面（Here or To Go）へ遷移
- 商品画像は10秒ごとに自動切り替え
- ブランドロゴも画像と連動して切り替え

### 4.2 Here or To Go（店内/テイクアウト選択）

#### レイアウト構成
```
+----------------------------------+
| [ロゴ]                           |
|                                  |
|          Welcome!                |
|       Select to begin            |
|                                  |
|   +--------+    +--------+       |
|   | HERE   |    | TO GO  |       |
|   +--------+    +--------+       |
|                                  |
| *Customers need to choose        |
| "Here" or "To Go" before         |
| starting orders.                 |
+----------------------------------+
```

#### UI要素詳細

| 要素名 | 内容 | スタイル |
|--------|------|----------|
| ヘッダーロゴ | ブランドロゴ | 左上配置、高さ: 80px |
| ウェルカムメッセージ | "Welcome!" | font-size: 36px, text-align: center |
| サブメッセージ | "Select to begin" | font-size: 20px, color: --text-light |
| HEREボタン | "HERE" | 幅: 200px, 高さ: 80px, border: 2px solid |
| TO GOボタン | "TO GO" | 幅: 200px, 高さ: 80px, border: 2px solid |
| 注記 | 選択必須の説明 | font-size: 12px, font-style: italic |

#### 動作仕様
- HEREボタン: 店内飲食モードでメインメニューへ
- TO GOボタン: テイクアウトモードでメインメニューへ
- 選択情報は注文完了まで保持

### 4.3 Main Menu（メインメニュー画面）

#### レイアウト構成
```
+----------------------------------+
| [ロゴ]                    🛒 1   |
|----------------------------------|
| All | Vegan | Beef | Chicken    |
|----------------------------------|
| +------+ +------+ +------+       |
| |餃子  | |ラーメン| |チキン |     |
| +------+ +------+ +------+       |
| +------+ +------+ +------+       |
| |寿司  | |ドリンク| |その他|      |
| +------+ +------+ +------+       |
|                           | PROMO|
|                           | AREA |
|----------------------------------|
| [Back to menu]                   |
+----------------------------------+
```

#### UI要素詳細

| 要素名 | 内容 | スタイル |
|--------|------|----------|
| ヘッダーロゴ | ブランドロゴ | 左上配置、高さ: 60px |
| カートアイコン | ショッピングカート + 数量 | 右上配置、color: --primary-red |
| カテゴリータブ | All, Vegan, Beef, Chicken | 横並び、アクティブ: background: --primary-blue |
| 商品グリッド | 3列 × n行 | gap: 20px, padding: 10px |
| 商品カード | 画像 + 商品名 | 幅: 150px, 高さ: 180px, border-radius: 5px |
| プロモーション | 販促バナー | 右側固定、幅: 200px |
| 戻るボタン | "Back to menu" | 左下配置、text-decoration: underline |

#### 商品カード詳細
```css
.product-card {
  width: 150px;
  height: 180px;
  border: 1px solid var(--border-gray);
  border-radius: 5px;
  overflow: hidden;
}

.product-image {
  height: 120px;
  width: 100%;
  object-fit: cover;
}

.product-name {
  padding: 10px;
  font-size: 14px;
  text-align: center;
}

.customizable-icon {
  position: absolute;
  top: 5px;
  right: 5px;
  width: 20px;
  height: 20px;
}
```

#### 動作仕様
- カテゴリータブ: タップでフィルタリング
- 商品カード: タップで詳細画面へ
- カートアイコン: タップで注文サマリーへ
- スクロール: 縦スクロール対応

### 4.4 Detail Menu（商品詳細画面）

#### レイアウト構成
```
+----------------------------------+
|                           🛒 1   |
|----------------------------------|
| +-------------+  Kuromon Ramen   |
| |             |  Large           |
| | 商品画像     |                  |
| |             |  [Allergen Alert]|
| +-------------+  詳細説明...      |
|                                  |
| Calories: 900 kcal               |
|                                  |
| Description:                     |
| A rich and creamy pork bone...   |
|                                  |
| Quantity: [-] 1 [+]              |
|                                  |
| [    Add to cart    ]            |
|                                  |
| Back to menu                     |
+----------------------------------+
```

#### UI要素詳細

| 要素名 | 内容 | スタイル |
|--------|------|----------|
| 商品画像 | 高解像度商品写真 | 幅: 300px, 高さ: 200px |
| 商品名 | "Kuromon Ramen Large" | font-size: 24px, font-weight: bold |
| アレルギー情報 | "Allergen Alert" + 詳細 | color: --primary-red, font-size: 14px |
| カロリー表示 | "Calories: 900 kcal" | background: #f8f9fa, padding: 10px |
| 商品説明 | 詳細説明文 | font-size: 16px, line-height: 1.5 |
| 数量選択 | - / 数量 / + | ボタン幅: 40px, font-size: 20px |
| カート追加ボタン | "Add to cart" | background: --primary-orange, 幅: 100% |
| 戻るリンク | "Back to menu" | color: --primary-blue, text-decoration: underline |

#### 動作仕様
- 数量ボタン: +/- で数量変更（最小1、最大10）
- Add to cart: カートに追加後、ポップアップ表示
- アレルギー情報: タップで詳細表示

### 4.5 Pop-up（アップセル画面）

#### レイアウト構成
```
+----------------------------------+
|         半透明オーバーレイ          |
|    +----------------------+      |
|    | Would you like to add?|      |
|    |                      |      |
|    | +-----+    +-----+   |      |
|    | |ドリンク| |サイド |   |      |
|    | +-----+    +-----+   |      |
|    |                      |      |
|    | 商品説明テキスト...    |      |
|    |                      |      |
|    | [   No, Thank you   ]|      |
|    | [   Add to cart    ]|      |
|    +----------------------+      |
+----------------------------------+
```

#### UI要素詳細

| 要素名 | 内容 | スタイル |
|--------|------|----------|
| オーバーレイ | 半透明背景 | background: rgba(0,0,0,0.5) |
| モーダルボックス | 白背景コンテナ | width: 80%, max-width: 600px, border-radius: 10px |
| 見出し | "Would you like to add?" | font-size: 20px, text-align: center |
| 推奨商品 | 2商品横並び | 幅: 150px, 高さ: 150px each |
| 商品説明 | 推奨理由 | font-size: 14px, color: --text-light |
| 拒否ボタン | "No, Thank you" | background: --border-gray |
| 追加ボタン | "Add to cart" | background: --primary-orange |

#### 動作仕様
- 表示タイミング: 商品カート追加後
- No, Thank you: ポップアップを閉じる
- Add to cart: 選択商品をカートに追加
- 背景タップ: ポップアップを閉じない

### 4.6 Order Summary（注文サマリー）

#### レイアウト構成
```
+----------------------------------+
|       Order Summary       🛒     |
|----------------------------------|
| [画像] Kuromon Ramen Large   🗑️  |
|        $36                       |
|----------------------------------|
| [画像] Ika Tentsuji Karaage  🗑️  |
|        Breast $16                |
|----------------------------------|
| [画像] Horoyoi Boco          🗑️  |
|        Strawberry $45            |
|----------------------------------|
|                                  |
| Subtotal (Before Tax):    $97   |
|                                  |
| [     Check out     ]            |
|                                  |
| Cancel All | Back to menu        |
+----------------------------------+
```

#### UI要素詳細

| 要素名 | 内容 | スタイル |
|--------|------|----------|
| ヘッダー | "Order Summary" | font-size: 24px, font-weight: bold |
| 商品リスト | 画像 + 名前 + 価格 | border-bottom: 1px solid --border-gray |
| 商品画像 | サムネイル | 60px × 60px, border-radius: 5px |
| 削除ボタン | ゴミ箱アイコン | color: --text-light, hover: --primary-red |
| 小計 | "Subtotal (Before Tax)" | font-size: 18px, font-weight: bold |
| チェックアウトボタン | "Check out" | background: --primary-green, 幅: 100% |
| キャンセルリンク | "Cancel All" | color: --primary-red |
| 戻るリンク | "Back to menu" | color: --primary-blue |

#### 動作仕様
- 削除ボタン: 確認ダイアログ後、商品削除
- Check out: 顧客情報入力へ
- Cancel All: 確認後、全商品削除
- 商品0個の場合: メニューへ自動遷移

### 4.7 Customer Field（顧客情報入力）

#### レイアウト構成
```
+----------------------------------+
|   Get updates on your order      |
|                                  |
| We'll use this to notify you     |
| about your order. Please note,   |
| standard messaging rates apply.  |
|                                  |
| First Name*                      |
| [                    ]           |
|                                  |
| Last Name*                       |
| [                    ]           |
|                                  |
| Phone Number*                    |
| [                    ]           |
|                                  |
| *Mandatory Fields                |
|                                  |
| [     Continue     ]             |
|                                  |
| Cancel All | Back to menu        |
+----------------------------------+
```

#### UI要素詳細

| 要素名 | 内容 | スタイル |
|--------|------|----------|
| 見出し | "Get updates on your order" | font-size: 20px, text-align: center |
| 説明文 | 通知に関する説明 | font-size: 14px, color: --text-light |
| 入力フィールド | テキストボックス | 高さ: 50px, border: 1px solid --border-gray |
| 必須マーク | * | color: --primary-red |
| 必須説明 | "*Mandatory Fields" | font-size: 12px, color: --text-light |
| 継続ボタン | "Continue" | background: --primary-blue, 幅: 100% |
| キャンセルリンク | "Cancel All" | color: --primary-red |
| 戻るリンク | "Back to menu" | color: --primary-blue |

#### 入力検証
```javascript
// 検証ルール
const validation = {
  firstName: {
    required: true,
    minLength: 2,
    pattern: /^[a-zA-Z\s]+$/
  },
  lastName: {
    required: true,
    minLength: 2,
    pattern: /^[a-zA-Z\s]+$/
  },
  phoneNumber: {
    required: true,
    pattern: /^\d{10}$/,
    format: "(xxx) xxx-xxxx"
  }
};
```

#### 動作仕様
- 入力検証: リアルタイム検証
- エラー表示: 入力フィールド下に赤文字
- Continue: 全項目有効時のみ活性化
- 電話番号: 自動フォーマット適用

## 5. レスポンシブ対応

### 5.1 ブレークポイント
```css
/* キオスク専用（固定サイズ） */
@media (width: 1080px) and (height: 1920px) {
  /* 縦向き設定 */
}

@media (width: 1920px) and (height: 1080px) {
  /* 横向き設定 */
}
```

### 5.2 タッチ最適化
```css
/* タッチターゲット最小サイズ */
.touchable {
  min-width: 44px;
  min-height: 44px;
  padding: 10px;
}

/* タッチフィードバック */
.touchable:active {
  opacity: 0.8;
  transform: scale(0.98);
}
```

## 6. アクセシビリティ

### 6.1 色彩コントラスト
- 文字と背景: 最小4.5:1
- 大文字（18px以上）: 最小3:1
- エラー表示: 赤以外の識別方法併用

### 6.2 フォントサイズ
- 最小フォントサイズ: 14px
- タッチデバイス推奨: 16px以上
- 重要情報: 18px以上

### 6.3 エラーハンドリング
- 明確なエラーメッセージ
- 修正方法の提示
- 視覚的 + テキストでの通知

## 7. パフォーマンス最適化

### 7.1 画像最適化
- 形式: WebP（フォールバック: JPEG）
- 商品画像: 最大500KB
- サムネイル: 最大100KB
- 遅延読み込み実装

### 7.2 アニメーション
```css
/* パフォーマンス優先設定 */
.animated {
  will-change: transform;
  transform: translateZ(0);
}

/* 減速モード対応 */
@media (prefers-reduced-motion: reduce) {
  .animated {
    animation: none;
    transition: none;
  }
}
```

## 8. 実装チェックリスト

### 8.1 各画面共通
- [ ] レイアウト実装
- [ ] スタイル適用
- [ ] タッチイベント実装
- [ ] 画面遷移実装
- [ ] エラーハンドリング

### 8.2 機能別
- [ ] カート機能
- [ ] 商品フィルタリング
- [ ] 入力検証
- [ ] アップセル表示
- [ ] 注文確定処理

### 8.3 テスト項目
- [ ] タッチ操作確認
- [ ] 画面遷移確認
- [ ] データ保持確認
- [ ] エラー処理確認
- [ ] パフォーマンス測定

## 9. 付録

### 9.1 アイコンセット
- ショッピングカート: 🛒
- 削除: 🗑️
- 追加: ＋
- 削除: －
- 戻る: ←
- 進む: →

### 9.2 参考実装
```html
<!-- 基本構造 -->
<div class="kiosk-container">
  <header class="kiosk-header">
    <!-- ヘッダー要素 -->
  </header>
  <main class="kiosk-content">
    <!-- メインコンテンツ -->
  </main>
  <footer class="kiosk-footer">
    <!-- フッター要素 -->
  </footer>
</div>
```

### 9.3 状態管理
```javascript
// 注文状態管理
const orderState = {
  mode: 'HERE', // or 'TO_GO'
  items: [],
  customer: {
    firstName: '',
    lastName: '',
    phoneNumber: ''
  },
  subtotal: 0,
  tax: 0,
  total: 0
};
```