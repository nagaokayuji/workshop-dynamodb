# テーブル設計における注意点
## 正規化しなくてよい

できるだけ少ないテーブル数を維持するのがベストプラクティスとして挙げられています。

- [DynamoDB を使用した設計とアーキテクチャの設計に関するベストプラクティス - Amazon DynamoDB](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/best-practices.html)

>DynamoDB では、最も一般的で重要なクエリをできるだけ速く、安価にするために、具体的にスキーマを設計します。データ構造は、ビジネスユースケースの特定の要件に合わせて調整されています。


### JOIN できない

RDB のような JOIN の概念はありません。

関連するデータはまとめて格納しておくのが良い手法とされています。

## 結果整合性

読み込み時に書き込み要求が反映されていない結果が返されることがあります。

※だいたい1秒以内には反映されるようです。

[読み込み整合性](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html)

デフォルトでは**結果整合性のある読み込み**が使用されますが、必要に応じて**強力な整合性のある読み込み**も使用できます。（ConsistentRead=true 指定）
