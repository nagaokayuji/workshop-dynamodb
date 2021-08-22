# テーブル設計における注意点

# テーブル設計における注意点

## 正規化しすぎない

できるだけ少ないテーブル数を維持するのがベストプラクティスとして挙げられています。

[https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/best-practices.html](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/best-practices.html)

## 結果整合性

読み込み時に書き込み要求が反映されていない結果が返されることがあります。

※だいたい1秒以内には反映されます。

[読み込み整合性](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html)

デフォルトでは**結果整合性のある読み込み**が使用されますが、必要に応じて**強力な整合性のある読み込み**も使用できます。（ConsistentRead=true 指定）
