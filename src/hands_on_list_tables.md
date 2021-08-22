# テーブル一覧を取得(ListTables)

```jsx
var params = {
// 今回は指定なし
};
dynamodb.listTables(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});
```

`athlete` というテーブルが存在すると思います。

以下のサイトからダウンロードしたものから1000行を抽出したものです。

[120 Years of Olympic History](https://www.kaggle.com/mysarahmadbhat/120-years-of-olympic-history)
