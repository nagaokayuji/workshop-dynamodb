# テーブルのデータを取得(Query)

[Query - Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_Query.html)

`Query` ではパーティションキーを指定し、パーティションキーが一致したすべての項目を取得します。

`KeyConditionExpression` を指定してパーティションキーを指定する必要があります。

※ 同様にソートキーの条件も指定することができます。

ここでは、 `ID` が `5` のデータを取得してみます。

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

## 実行結果

結果は以下のようになります。
```json
{
  "Items": [
    {
      "NOC": "NED",
      "Sex": "F",
      "index": 4,
      "City": "Calgary",
      "Weight": 82,
      "Name": "Christine Jacoba Aaftink",
      "Sport": "Speed Skating",
      "Year": 1988,
      "Games": "1988 Winter",
      "Event": "Speed Skating Women's 500 metres",
      "Height": 185,
      "Team": "Netherlands",
      "ID": 5,
      "Medal": null,
      "Season": "Winter",
      "Age": 21
    },
    {
      "NOC": "NED",
      "Sex": "F",
      "index": 5,
      "City": "Calgary",
      "Weight": 82,
      "Name": "Christine Jacoba Aaftink",
      "Sport": "Speed Skating",
      "Year": 1988,
      "Games": "1988 Winter",
      "Event": "Speed Skating Women's 1,000 metres",
      "Height": 185,
      "Team": "Netherlands",
      "ID": 5,
      "Medal": null,
      "Season": "Winter",
      "Age": 21
    },
    {
      "NOC": "NED",
      "Sex": "F",
      "index": 6,
      "City": "Albertville",
      "Weight": 82,
      "Name": "Christine Jacoba Aaftink",
      "Sport": "Speed Skating",
      "Year": 1992,
      "Games": "1992 Winter",
      "Event": "Speed Skating Women's 500 metres",
      "Height": 185,
      "Team": "Netherlands",
      "ID": 5,
      "Medal": null,
      "Season": "Winter",
      "Age": 25
    },
    {
      "NOC": "NED",
      "Sex": "F",
      "index": 7,
      "City": "Albertville",
      "Weight": 82,
      "Name": "Christine Jacoba Aaftink",
      "Sport": "Speed Skating",
      "Year": 1992,
      "Games": "1992 Winter",
      "Event": "Speed Skating Women's 1,000 metres",
      "Height": 185,
      "Team": "Netherlands",
      "ID": 5,
      "Medal": null,
      "Season": "Winter",
      "Age": 25
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
    }
  ],
  "Count": 6,
  "ScannedCount": 6
}
```

結果の項目は Scan と同様になっています。


## 注意
`athlete` テーブルでは `ID` がパーティションキーであることに注意してください。

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

## Query のオプション
いくつかご紹介します。

- KeyConditionExpression
  - Query 特有のオプションです。
  - パーティションキーやソートキーの条件を指定します。
  - パーティションキー
    - `partitionKeyName = :partitionkeyval` の形式にする必要があります。
  - ソートキー
    - `=` 以外の演算子も使用できます。
      - `<`, `>`, `<=`, `>=`, `BETWEEN`, `begins_with`
- ExpressionAttributeNames
  - 置換に使用する名前(Attribute)を指定します。
  - 
- ExpressionAttributeValues
  - 指定したexpressionのプレースホルダに値を設定します。
  - `:`を使用して
  - 
- ScanIndexForward
  - ソートキーの順序を指定できます。
  - true
    - 昇順になります。
    - 指定しない場合はtrueとして扱われます。
  - false
    - 降順になります。


