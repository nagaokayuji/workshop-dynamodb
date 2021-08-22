# GSIを使用してQueryを実行する

以下のように `IndexName` を指定し、特定の `Year` を持つ項目を取得できます。

```jsx
var params = {
  TableName: "athlete",
  IndexName: "byYear", // 先ほど追加した GSI
  KeyConditionExpression: "#y = :year", // Year は予約語のため、 #y に置き換える
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