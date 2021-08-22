# テーブル一覧を取得(ListTables)

ListTables を実行してみます。

特にパラメータは必要ありません。

```jsx
var params = {
// 今回は指定なし
};
dynamodb.listTables(params, function(err, data) {
    if (err) ppJson(err); // an error occurred
    else ppJson(data); // successful response
});
```

## 実行結果
実行してみると、

次のようなレスポンスが返ってきます。

```jsx
{"TableNames":["athlete"]}
```

`athlete` というテーブルが存在することがわかりました。


このテーブルには以下のサイトからダウンロードしたCSV(`athlete_events.csv`)から1000行を抽出したデータが格納されています。

[120 Years of Olympic History](https://www.kaggle.com/mysarahmadbhat/120-years-of-olympic-history)


```
About this file
ID Unique number for each athlete
Name Athlete's name
Sex Male (M) or Female (F)
Age Integer
Height In centimeters
Weight In kilograms
Team Team name
NOC National Olympic Committee 3-letter code
Games Year and season
Year Integer
Season summer or Winter
City Host city
Sport Sport
Event Event
Medal Gold, Silver, Bronze, or NA
```