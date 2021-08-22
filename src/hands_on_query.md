# テーブルのデータを取得(Query)

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
