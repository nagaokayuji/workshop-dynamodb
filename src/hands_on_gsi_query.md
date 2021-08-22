# GSIを使用してQueryを実行する

追加したGSIを使用してQueryを発行してみましょう。

以下のように `IndexName` を指定し、特定の `Year` を持つ項目を取得できます。

ここでは`Year`が`1994`の項目を取得してみます。

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

ここで、`ReturnConsumedCapacity: "INDEXES"` を指定することでセカンダリインデックスのアクセスも表示することができます。


## 実行結果

結果が5件返ってきていることを確認しておきましょう。

```json
{
  "Items": [
    {
      "NOC": "NED",
      "Sex": "F",
      "index": 9,
      "City": "Lillehammer",
      "Weight": 82,
      "Name": "Christine Jacoba Aaftink",
      "Sport": "Speed Skating",
      "Year": 1994,
      "Games": "1994 Winter",
      "Event": "Speed Skating Women's 1,000 metres",
      "Height": 185,
      "Team": "Netherlands",
      "ID": 5,
      "Medal": null,
      "Season": "Winter",
      "Age": 27
    },
    {
      "NOC": "NED",
      "Sex": "F",
      "index": 8,
      "City": "Lillehammer",
      "Weight": 82,
      "Name": "Christine Jacoba Aaftink",
      "Sport": "Speed Skating",
      "Year": 1994,
      "Games": "1994 Winter",
      "Event": "Speed Skating Women's 500 metres",
      "Height": 185,
      "Team": "Netherlands",
      "ID": 5,
      "Medal": null,
      "Season": "Winter",
      "Age": 27
    },
    {
      "NOC": "USA",
      "Sex": "M",
      "index": 15,
      "City": "Lillehammer",
      "Weight": 75,
      "Name": "Per Knut Aaland",
      "Sport": "Cross Country Skiing",
      "Year": 1994,
      "Games": "1994 Winter",
      "Event": "Cross Country Skiing Men's 30 kilometres",
      "Height": 188,
      "Team": "United States",
      "ID": 6,
      "Medal": null,
      "Season": "Winter",
      "Age": 33
    },
    {
      "NOC": "USA",
      "Sex": "M",
      "index": 14,
      "City": "Lillehammer",
      "Weight": 75,
      "Name": "Per Knut Aaland",
      "Sport": "Cross Country Skiing",
      "Year": 1994,
      "Games": "1994 Winter",
      "Event": "Cross Country Skiing Men's 10 kilometres",
      "Height": 188,
      "Team": "United States",
      "ID": 6,
      "Medal": null,
      "Season": "Winter",
      "Age": 33
    },
    {
      "NOC": "USA",
      "Sex": "M",
      "index": 17,
      "City": "Lillehammer",
      "Weight": 75,
      "Name": "Per Knut Aaland",
      "Sport": "Cross Country Skiing",
      "Year": 1994,
      "Games": "1994 Winter",
      "Event": "Cross Country Skiing Men's 4 x 10 kilometres Relay",
      "Height": 188,
      "Team": "United States",
      "ID": 6,
      "Medal": null,
      "Season": "Winter",
      "Age": 33
    }
  ],
  "Count": 5,
  "ScannedCount": 5,
  "LastEvaluatedKey": { "Year": 1994, "index": 17, "ID": 6 },
  "ConsumedCapacity": {
    "TableName": "athlete",
    "CapacityUnits": 0.5,
    "Table": { "CapacityUnits": 0 },
    "GlobalSecondaryIndexes": { "byYear": { "CapacityUnits": 0.5 } }
  }
}
```

以上より、GSIを使用してQueryを発行することができました。
### ConsumedCapacity を見てみる
```json
"GlobalSecondaryIndexes": { "byYear": { "CapacityUnits": 0.5 } }
```
追加したGSIが使用されていることがわかります。