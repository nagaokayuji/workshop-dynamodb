# おわりに

## その他のトピック

今回詳しく触れられなかったものとしては次のようなものがあり、ここで軽くご紹介します。

### PartiQL
DynamoDB で使用できる SQLライクな問合せ言語です。

SQLに慣れている場合は書きやすいという利点がありますが、DynamoDBの特性を理解していないと効率の悪いクエリになってしまう懸念があります。

### 設計について
詳しい設計の注意点やベストプラクティスについてはこちらにまとまっています。

- [Best Practices for Designing and Architecting with DynamoDB - Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/best-practices.html)

### DynamoDB Streams
テーブル内のデータ項目に対する変更をイベントとしてキャプチャする機能です。
変更をトリガーとしてリアルタイムな処理が可能になります。

過去24時間以内の変更が保持されます。

### TTL
設定した期限(TTL)になると自動で項目を削除する機能です。（厳密にはTTLを過ぎてから48時間以内）
期限は **エポック形式** の値にする必要があります。
