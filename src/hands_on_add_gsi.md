# GSIを追加する

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