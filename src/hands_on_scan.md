# テーブルのデータを取得(Scan)

Scanでは簡単にテーブルの内容を取得することができます。

後述する Query と比べると大量の RCU を消費するため注意です。

3件取得してみます。

```jsx
var params = {
    TableName: 'athlete',
    Limit: 3, // 3件取得
    ReturnConsumedCapacity: 'TOTAL', // 消費したキャパシティユニットも返す
};
dynamodb.scan(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});
```
