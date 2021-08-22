# テーブルのデータを取得(Query)

[Query - Amazon DynamoDB](https://docs.aws.amazon.com/amazondynamodb/latest/APIReference/API_Query.html)

`Query` ではパーティションキーを指定し、パーティションキーが一致したすべての項目を取得します。

`KeyConditionExpression` を指定するとソートキーの条件で絞ることもできます。



試しに `ID` が `5` のデータを取得してみます。

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