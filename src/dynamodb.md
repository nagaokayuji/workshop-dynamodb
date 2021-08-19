# DynamoDBハンズオン

## DynamoDBとは？

AWSマネージドのNoSQLデータベース。

### NoSQL (Not Only SQL)

RDBMS以外のDBMSを指します。

扱うデータによっては非常に高速になります。

NoSQLは色々分類できます。

- Key-Value store
    - Keyに対してValueを格納する構造
- Document store
    - 自由なデータ構造で「ドキュメント」として格納する
    - XML, YAML, JSON
    - key-value store に含まれる
- Graph
    - グラフ構造
- Column-oriented store （列指向）
    - 列方向にデータを扱う
- Object database
    - オブジェクト指向的にデータを扱う

**DynamoDB は Document store / Key-Value store** に該当します。

## 抑えておくべき概念

### テーブル・項目・属性

- テーブル(Tables)
    - データのコレクション
    - RDBのテーブルと同様
- 項目(Items)
    - 一意に識別可能な属性のグループ
    - RDBの行・レコードに対応
- 属性(Attributes)
    - データ要素
    - RDBの列・フィールドに対応

### キー

- パーティションキー
    - 必ず必要
    - ハッシュ関数への入力として使用される
    - 物理的に**項目が保存されるパーティション**が決まる
- ソートキー
    - 指定しなくても良い
    - パーティションキーと合わせて複合主キーとなる必要がある
    - 同じパーティションキーの項目はソートキーでソートされて保存される

**パーティションキーとソートキーで一意**になる必要があります。

→ RDBのように3列以上での複合主キーは設定できません。

### インデックスについて

非常に重要な概念です。

Query, Scan オペレーションのために使用します。

- GSI(グローバルセカンダリインデックス)
    - インデックスのプライマリキーを任意に指定可能
    - 一意にならなくてもよい
    - **キャパシティユニットを消費**する
    - テーブル作成後に追加可能
- LSI(ローカルセカンダリインデックス)
    - テーブルのパーティションキーを使用
    - ソートキーは任意に指定可能
    - **テーブル作成時**に指定する（テーブル作成後はLSIを追加できない）

### キャパシティユニット

書き込み・読み込みリクエストに対して**キャパシティユニット**という概念があり、

消費したキャパシティユニットに応じた料金が請求されます。

[DynamoDB のプロビジョンされた容量テーブルの設定の管理](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/ProvisionedThroughput.html)

- RCU
    - 1 RCU - 最大サイズ 4 KB の項目について、1 秒あたり 1 回の強力な整合性のある読み込み、あるいは 1 秒あたり 2 回の結果整合性のある読み込み
- WCU
    - 1 WCU - 最大サイズが 1 KB の項目について、1 秒あたり 1 回の書き込み
    - トランザクション書き込みリクエストでは2倍消費

## テーブル設計における注意点

### 正規化しすぎない

できるだけ少ないテーブル数を維持するのがベストプラクティスとして挙げられています。

[https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/best-practices.html](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/best-practices.html)

### 結果整合性

読み込み時に書き込み要求が反映されていない結果が返されることがあります。

※だいたい1秒以内には反映されます。

[読み込み整合性](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/HowItWorks.ReadConsistency.html)

デフォルトでは**結果整合性のある読み込み**が使用されますが、必要に応じて**強力な整合性のある読み込み**も使用できます。（ConsistentRead=true 指定）

## オペレーション

### テーブル関連

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

### データ取得

- GetItem
    - テーブルから単一の項目を取得
    - **プライマリキーを指定**する
- Query
    - **パーティションキーを指定**して項目を取得
    - 複数取得できる
- Scan
    - 指定テーブル/インデックスのすべての項目を取得
    - 遅い
    - filter, limit 指定可能だが注意が必要

### データ作成

- PutItem
    - 項目の属性を追加
    - **プライマリキーを指定**する
    - プライマリキー以外の属性は**任意**

### データ更新

- UpdateItem
    - 項目の属性を更新
    - **プライマリキーを指定**する

### データ削除

- DeleteItem
    - 単一の項目を削除
    - **プライマリキーを指定**する

---

## ハンズオン

ローカル環境でDocker, JavaScript SDK を使用します。

[https://github.com/nagaokayuji/workshop](https://github.com/nagaokayuji/workshop/tree/main/dynamodb)

- docker-compose.yml

```yaml
version: "3.8"
services:
  dynamodb-local:
    image: amazon/dynamodb-local:1.16.0
    container_name: dynamodb-local
    ports:
      - "8000:8000"
    volumes:
      - "./docker/dynamodb:/home/dynamodblocal/data"
    working_dir: /home/dynamodblocal
    command: "-jar DynamoDBLocal.jar -sharedDb -optimizeDbBeforeStartup -dbPath ./data"
```

- 起動

```bash
docker-compose up
```

- [http://localhost:8000/shell/](http://localhost:8000/shell/) にアクセス

### テーブル一覧を取得(ListTables)

```jsx
var params = {
// 今回は指定なし
};
dynamodb.listTables(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});
```

`athlete` というテーブルのみが存在すると思います。

以下のサイトからダウンロードしたものから1000行を抽出したものです。

[120 Years of Olympic History](https://www.kaggle.com/mysarahmadbhat/120-years-of-olympic-history)

### テーブルの詳細を取得(DescribeTable)

このテーブルの詳細を見てみます。

```jsx
var params = {
    TableName: 'athlete',
};
dynamodb.describeTable(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});
```

![https://res.cloudinary.com/ddaz9etkx/image/upload/v1628485639/ot/describe-table_fudpz9.png](https://res.cloudinary.com/ddaz9etkx/image/upload/v1628485639/ot/describe-table_fudpz9.png)

※ DynamoDB の型について

[サポートされているデータの種類](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/DynamoDBMapper.DataTypes.html)

### テーブルのデータを取得(scan)

3件取得してみます。

```jsx
var params = {
    TableName: 'athlete',
    Limit: 3, // 3件取得
    ReturnConsumedCapacity: 'TOTAL', // 消費したキャパシティユニットも返す
};
dynamodb.scan(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});
```

### テーブルのデータを取得(Query)

`ID` が `5` のデータを取得します。

```jsx
var params = {
    TableName: 'athlete',
    KeyConditionExpression: 'ID = :val', // :val のようなプレースホルダが必要
    ExpressionAttributeValues: { 
        ":val": 5, // :val の値
    }, 
    ScanIndexForward: true, // ソートキーの昇順
};
docClient.query(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});
```

この `ID` がパーティションキーであることに注意してください。

例えば、以下のようにパーティションキーを指定しないクエリはエラーとなります。

```jsx
var params = {
    TableName: 'athlete',
    KeyConditionExpression: 'Year = :val',
    ExpressionAttributeValues: { 
        ":Year": 1994, 
    }, 
    ScanIndexForward: true, 
};
docClient.query(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});
```

### GSIを追加する

UpdateTableオペレーションにより、

`Year` をパーティションキーとしたGSIを追加します。

```jsx
var params = {
  TableName: "athlete",
  AttributeDefinitions: [
    { AttributeName: "Year", AttributeType: "N" },
    { AttributeName: "ID", AttributeType: "N" },
  ],
  GlobalSecondaryIndexUpdates: [
    {
      // GSIを更新
      Create: {
        // 追加
        IndexName: "byYear", // 名前をつける
        KeySchema: [
          {
            AttributeName: "Year", // Year をパーティションキーに設定
            KeyType: "HASH",
          },
          {
            AttributeName: "ID", // ID をソートキーに設定
            KeyType: "RANGE",
          },
        ],
        Projection: {
          ProjectionType: "ALL", // すべての属性に対して適用
        },
        ProvisionedThroughput: {
          ReadCapacityUnits: 1,
          WriteCapacityUnits: 1,
        },
      },
    },
  ],
};
dynamodb.updateTable(params, function (err, data) {
  if (err) ppJson(err);
  // an error occurred
  else ppJson(data); // successful response
});
```

`byYear` という名前でGSIを作成しました。

（DescribeTable で確認してみましょう。）

### GSIを使用してQueryを実行する

以下のように `IndexName` を指定し、特定の `Year` を持つ項目を取得できます。

```jsx
var params = {
  TableName: "athlete",
  IndexName: "byYear", // 先ほど追加した GSI
  KeyConditionExpression: "#y = :year", // Year は予約後のため、 #y に置き換える
  ExpressionAttributeNames: {
    "#y": "Year", // #y を Year にマッピング
  },
  ExpressionAttributeValues: {
    ":year": 1994,
  },
  Limit: 5, // 5件
  ScanIndexForward: true, // ID の昇順に取得
  ReturnConsumedCapacity: "INDEXES", // セカンダリインデックスの小計も取得
};
docClient.query(params, function (err, data) {
  if (err) ppJson(err);
  // an error occurred
  else ppJson(data); // successful response
});
```

結果が5件返ってきていることを確認しておきましょう。

### [Q] Scan で filter を使えばよいのでは？

以下のようなリクエストで試してみましょう。

```jsx
var params = {
  TableName: "athlete",
  Limit: 5, // 5 件取得したい
  FilterExpression: "#y = :year",
  ExpressionAttributeNames: {
    "#y": "Year",
  },
  ExpressionAttributeValues: {
    ":year": { N: "1994" },
  },
  ReturnConsumedCapacity: "INDEXES",
};
dynamodb.scan(params, function (err, data) {
  if (err) ppJson(err);
  // an error occurred
  else ppJson(data); // successful response
});
```

思った件数の結果が得られません。

実は、**FilterExpression はクエリの結果を取得してから**絞り込みを行うため、今回のケースだと**5件分Scanしてから**条件に合致した項目のみを返しています。

このため、結果が失われないようにするためには Limit を指定せずに Scan を実行する必要があり、この場合大量のキャパシティユニットを消費することになります。