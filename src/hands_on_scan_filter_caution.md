# Scan, Filter の注意点

前項ではGSIを追加して特定の`Year`で検索しましたが、以下のような疑問が湧くのではないでしょうか。

-  Scan で filter を使えばよいのでは？

以下のようなScanオペレーションで試してみましょう。

`FilterExpression` に 前項の`KeyConditionExpression` と同一の条件を指定しています。

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

## 実行結果
以下のような結果が得られました。

```json
{
  "Items": [],
  "Count": 0,
  "ScannedCount": 5,
  "LastEvaluatedKey": { "index": { "N": "895" }, "ID": { "N": "512" } },
  "ConsumedCapacity": {
    "TableName": "athlete",
    "CapacityUnits": 0.5,
    "Table": { "CapacityUnits": 0.5 }
  }
}
```
思った件数の結果が得られません。

**FilterExpression はクエリの結果を取得してから**絞り込みを行うため、今回のケースだと**5件分Scanしてから**条件に合致した項目のみを返しています。

このため、結果が失われないようにするためには Limit を指定せずに Scan を実行する必要があり、この場合大量のキャパシティユニットを消費することになります。