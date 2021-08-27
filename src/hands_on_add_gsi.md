# GSIを追加する

GSI を追加するには
`UpdateTable` オペレーションで `GlobalSecondaryIndexUpdates`を指定します。


ここでは `Year` をパーティションキーとしたGSIを追加してみます。

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

## 実行結果
`byYear` という名前でGSIを作成しました。

実行すると以下のようなレスポンスが得られます。

```json
{
  "TableDescription": {
    "AttributeDefinitions": [
      { "AttributeName": "Year", "AttributeType": "N" },
      { "AttributeName": "index", "AttributeType": "N" },
      { "AttributeName": "ID", "AttributeType": "N" }
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
    "TableArn": "arn:aws:dynamodb:ddblocal:000000000000:table/athlete",
    "GlobalSecondaryIndexes": [
      {
        "IndexName": "byYear",
        "KeySchema": [
          { "AttributeName": "Year", "KeyType": "HASH" },
          { "AttributeName": "ID", "KeyType": "RANGE" }
        ],
        "Projection": { "ProjectionType": "ALL" },
        "IndexStatus": "CREATING",
        "Backfilling": false,
        "ProvisionedThroughput": {
          "ReadCapacityUnits": 1,
          "WriteCapacityUnits": 1
        },
        "IndexArn": "arn:aws:dynamodb:ddblocal:000000000000:table/athlete/index/byYear"
      }
    ]
  }
}
```

なお、上記のレスポンスは DescribeTable を実行したときと同様の結果となっています。