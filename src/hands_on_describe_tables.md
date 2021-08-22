# テーブルの詳細を取得(DescribeTable)

このテーブルの詳細を見てみます。

```jsx
var params = {
    TableName: 'athlete',
};
dynamodb.describeTable(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});
```

![https://res.cloudinary.com/ddaz9etkx/image/upload/v1628485639/ot/describe-table_fudpz9.png](https://res.cloudinary.com/ddaz9etkx/image/upload/v1628485639/ot/describe-table_fudpz9.png)

※ DynamoDB の型について

[サポートされているデータの種類](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/DynamoDBMapper.DataTypes.html)
