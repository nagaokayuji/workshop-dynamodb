# テーブルの詳細を取得(DescribeTable)

[DescribeTable - Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_DescribeTable.html)

`DescribeTable` を実行して`athlete`テーブルの詳細を取得してみましょう。

以下のようにテーブル名(`TableName`)を指定してDescribeTableを実行します。

```jsx
var params = {
    TableName: 'athlete',
};
dynamodb.describeTable(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});
```

実行すると以下のようなレスポンスが返ってきます。

```JSON
{
  "Table": {
    "AttributeDefinitions": [
      { "AttributeName": "ID", "AttributeType": "N" },
      { "AttributeName": "index", "AttributeType": "N" }
    ],
    "TableName": "athlete",
    "KeySchema": [
      { "AttributeName": "ID", "KeyType": "HASH" },
      { "AttributeName": "index", "KeyType": "RANGE" }
    ],
    "TableStatus": "ACTIVE",
    "CreationDateTime": "2021-08-09T04:33:11.161Z",
    "ProvisionedThroughput": {
      "LastIncreaseDateTime": "1970-01-01T00:00:00.000Z",
      "LastDecreaseDateTime": "1970-01-01T00:00:00.000Z",
      "NumberOfDecreasesToday": 0,
      "ReadCapacityUnits": 1,
      "WriteCapacityUnits": 1
    },
    "TableSizeBytes": 182074,
    "ItemCount": 999,
    "TableArn": "arn:aws:dynamodb:ddblocal:000000000000:table/athlete"
  }
}
```

## レスポンスの中身について

TableDescription の定義が返ってきます。

詳しくは以下に記載があります。

[TableDescription - Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_TableDescription.html)
### AttributeDefinitions

- AttributeName
  - 属性名です。
- AttributeType
  - 属性のデータ型の定義です。
    - N = Number
    - S = String
    - B = Binary

※ DynamoDB の型について

[サポートされているデータの種類](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/DynamoDBMapper.DataTypes.html)
### KeySchema
パーティションキー・ソートキー情報です。

- AttributeName
  - 属性名です。
- KeyType
  - その名の通りキーのタイプです。
  - `HASH`　= パーティションキー
  - `RANGE` = ソートキー


