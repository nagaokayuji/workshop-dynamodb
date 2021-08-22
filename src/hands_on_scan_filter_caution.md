# Scan, Filter の注意点

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