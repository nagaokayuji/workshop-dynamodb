# オペレーション

DynamoDB で使用できる API について紹介します。

詳しくは以下のドキュメントをご覧ください。

[Actions - Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_Operations.html)

## テーブル関連

- CreateTable
    - 新しいテーブルを作成
- DescribeTable
    - テーブルに関する情報を取得
- ListTables
    - リストのすべてのテーブルの名前を取得
- UpdateTable
    - テーブルのインデックス設定を変更
- DeleteTable
    - テーブルを削除

## データ取得

- GetItem
    - テーブルから単一の項目を取得
    - **プライマリキーを指定**する
- BatchGetItem
    - 1度の呼び出しで GetItem を複数回リクエストできる
    - 最大100項目まで取得可能
- Query
    - **パーティションキーを指定**して項目を取得
    - 複数取得できる
- Scan
    - 指定テーブル/インデックスのすべての項目を取得
    - 遅い
    - filter, limit 指定可能だが注意が必要

## データ作成

- PutItem
    - 項目の属性を追加
    - **プライマリキーを指定**する
    - プライマリキー以外の属性は**任意**

## データ更新

- UpdateItem
    - 項目の属性を更新
    - **プライマリキーを指定**する

## データ削除

- DeleteItem
    - 単一の項目を削除
    - **プライマリキーを指定**する

## その他
- BatchWriteItem
  - 1度の呼び出しで複数の書き込み・削除ができる
  - 大量データの扱いに便利
  - 最大25リクエストまで
