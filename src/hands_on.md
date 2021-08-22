# ハンズオン

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

## テーブル一覧を取得(ListTables)

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

## テーブルの詳細を取得(DescribeTable)

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

## テーブルのデータを取得(scan)

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

## テーブルのデータを取得(Query)

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

## GSIを追加する

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

## GSIを使用してQueryを実行する

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

## [Q] Scan で filter を使えばよいのでは？

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