# 設計

家計簿アプリの作成を目的とする．

## 全体設計

- 各月の収支が見られるようにする
    - 収入: 仕事の収入
    - 支出
        - 毎月固定支出: 携帯料金，ローン
        - カテゴリ別: 食費，必要経費，趣味...etc
    - 移動(振替): 銀行 → パスモ，PayPayなど

- 銀行，PayPay，パスモ，現金，それぞれの動きがみられるようにする

- 収支のグラフ表示

- 日，週，月ごとの金額推移線グラフ
    - 固定支出を含めての表示，含めないでの表示

- カテゴリ別棒グラフ: 前日，先週，先月比

- 詳細の確認
    - カテゴリ別でソート
    - マウスオーバーで概要確認

- 毎週の自己フィードバック: どのようにお金を使ったのか，無駄な買い物の分析

- ログインページの設計

## データベースの設計

- Users

|カラム名|型|KEY|NOT NULL|ユニーク制約|説明|
|---|---|---|---|---|---|
|user_id|VERCHER(50)|PRIMARY KEY|〇|〇|ユーザーID|
|username|VERCHER(50)||〇||表示名|
|email|VERCHER(50)||〇||メールアドレス|
|password|VERCHER(50)||〇||パスワード|
|created_at|DATETIME||〇||登録日時|

- Accounts

|カラム名|型|KEY|NOT NULL|ユニーク制約|説明|
|---|---|---|---|---|---|
|account_id|VERCHER(50)|PRIMARY KEY|〇|〇|口座ID|
|user_id|VERCHER(50)|FOREIGN KEY|〇|〇|ユーザID|
|account_name|VERCHER(50)||〇||口座名|
|account_type_id|VERCHER(5)||〇||口座タイプID|
|remining_amount|INTEGER||〇||口座残高|

- AccountsTypes

|カラム名|型|KEY|NOT NULL|ユニーク制約|説明|
|---|---|---|---|---|---|
|account_type_id|VERCHER(3)|PRIMARY KEY|〇|〇|口座タイプID|
|account_type|VERCHER(50)||〇||口座タイプ名|

- Categories

|カラム名|型|KEY|NOT NULL|ユニーク制約|説明|
|---|---|---|---|---|---|
|category_id|VERCHER(3)|PRIMARY KEY|〇|〇|カテゴリーID|
|user_id|VERCHER(50)|FOREIGN KEY|〇|〇|ユーザID|
|category_name|VERCHER(50)||〇||カテゴリー名|

- Transactions

|カラム名|型|KEY|NOT NULL|ユニーク制約|説明|
|---|---|---|---|---|---|
|transaction_id|VERCHER(50)|PRIMARY KEY|〇|〇|取引ID|
|user_id|VERCHER(50)|FOREIGN KEY|〇|〇|ユーザID|
|account_id|VERCHER(50)|FOREIGN KEY|〇|〇|口座ID|
|category_id|VERCHER(3)|FOREIGN KEY|〇|〇|カテゴリーID|
|amount|INTEGER||〇||金額|
|description|TEXT||||詳細|
|transaction_date|DATETIME||〇||取引日|

- Transfers

|カラム名|型|KEY|NOT NULL|ユニーク制約|説明|
|---|---|---|---|---|---|
|transfer_id|VERCHER(50)|PRIMARY KEY|〇|〇|振替ID|
|user_id|VERCHER(50)|FOREIGN KEY|〇|〇|ユーザID|
|from_account_id|VERCHER(50)|FOREIGN KEY|〇|〇|振替元口座ID|
|to_account_id|VERCHER(50)|FOREIGN KEY|〇|〇|振替先口座ID|
|amount|INTEGER||〇||金額|
|description|TEXT||||詳細|
|transfer_date|DATETIME||〇||振替日|

## 画面遷移図