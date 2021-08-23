# キャパシティユニット


DynamoDB には**キャパシティユニット**という概念があり、
読み込み・書き込みリクエストに対してそれぞれ RCU(Read Capacity Unit), WCU(Write Capacity Unit) という容量が消費されていきます。

消費したキャパシティユニットに応じた料金が請求されます。

[DynamoDB のプロビジョンされた容量テーブルの設定の管理](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/ProvisionedThroughput.html)

- 1 RCU
    - 最大サイズ **4 KB** の項目について、 1 秒あたり 2 回の結果整合性のある読み込み
    - 強力な整合性のある読み込みでは 2 倍消費
- 1 WCU
    - 最大サイズが **1 KB** の項目について、1 秒あたり 1 回の書き込み
    - トランザクション書き込みリクエストでは 2 倍消費