# キャパシティユニット


書き込み・読み込みリクエストに対して**キャパシティユニット**という概念があり、

消費したキャパシティユニットに応じた料金が請求されます。

[DynamoDB のプロビジョンされた容量テーブルの設定の管理](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/ProvisionedThroughput.html)

- RCU
    - 1 RCU - 最大サイズ 4 KB の項目について、1 秒あたり 1 回の強力な整合性のある読み込み、あるいは 1 秒あたり 2 回の結果整合性のある読み込み
- WCU
    - 1 WCU - 最大サイズが 1 KB の項目について、1 秒あたり 1 回の書き込み
    - トランザクション書き込みリクエストでは2倍消費