# テーブルのデータを取得(Scan)

Scanでは簡単にテーブルの内容を（複数件）取得することができます。

後述する Query と比べると大量の RCU を消費するため注意が必要です。

様々なオプションが指定できますが、今回は`Limit` を指定し、3件取得してみます。

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

## 実行結果

これを実行すると
以下のような結果が得られると思います。

```json
{
  "Items": [
    {
      "NOC": { "S": "NOR" },
      "Sex": { "S": "M" },
      "index": { "N": "118" },
      "City": { "S": "Barcelona" },
      "Weight": { "N": "75" },
      "Name": { "S": "Morten Gjerdrum Aasen" },
      "Sport": { "S": "Equestrianism" },
      "Year": { "N": "1992" },
      "Games": { "S": "1992 Summer" },
      "Event": { "S": "Equestrianism Mixed Jumping, Individual" },
      "Height": { "N": "185" },
      "Team": { "S": "Norway" },
      "ID": { "N": "43" },
      "Medal": { "NULL": true },
      "Season": { "S": "Summer" },
      "Age": { "N": "34" }
    },
    {
      "NOC": { "S": "UZB" },
      "Sex": { "S": "M" },
      "index": { "N": "610" },
      "City": { "S": "Athina" },
      "Weight": { "N": "73" },
      "Name": { "S": "Sherzod Abdurahmonov" },
      "Sport": { "S": "Boxing" },
      "Year": { "N": "2004" },
      "Games": { "S": "2004 Summer" },
      "Event": { "S": "Boxing Men's Middleweight" },
      "Height": { "N": "178" },
      "Team": { "S": "Uzbekistan" },
      "ID": { "N": "352" },
      "Medal": { "NULL": true },
      "Season": { "S": "Summer" },
      "Age": { "N": "22" }
    },
    {
      "NOC": { "S": "CRC" },
      "Sex": { "S": "M" },
      "index": { "N": "907" },
      "City": { "S": "Los Angeles" },
      "Weight": { "N": "72" },
      "Name": { "S": "Glen Abrahams Martnez" },
      "Sport": { "S": "Athletics" },
      "Year": { "N": "1984" },
      "Games": { "S": "1984 Summer" },
      "Event": { "S": "Athletics Men's 100 metres" },
      "Height": { "N": "179" },
      "Team": { "S": "Costa Rica" },
      "ID": { "N": "517" },
      "Medal": { "NULL": true },
      "Season": { "S": "Summer" },
      "Age": { "N": "22" }
    }
  ],
  "Count": 3,
  "ScannedCount": 3,
  "LastEvaluatedKey": { "index": { "N": "907" }, "ID": { "N": "517" } },
  "ConsumedCapacity": { "TableName": "athlete", "CapacityUnits": 0.5 }
}
```

### 結果の見方
- Items
  - ここに取得したデータが入っています。
- Count
  - 結果の件数です。
- ScannedCount
  - スキャンされた件数です。
  - FilterExpression 等で絞り込まれる前の件数を指します。
- LastEvaluatedKey
  - オペレーションが終了したときのプライマリキーです。
  - すべての要素がScanされた場合は空になります。

## Scanのオプション
- FilterExpression
  - 結果を絞り込むことができます。
  - 表示される結果を絞り込むため、指定しても消費する RCU は変わりません。
- IndexName
  - セカンダリインデックスを使用する場合に指定します。
- Select
  - 結果の属性を指定できます。
  - 指定できる内容は以下です。
    - ALL_ATTRIBUTES
      - すべての属性を返します。
      - 何も指定しない場合はこれが指定されます。
    - ALL_PROJECTED_ATTRIBUTES
      - インデックスで設定されたすべての属性を返します。
      - インデックス使用の場合に指定できます。
    - SPECIFIC_ATTRIBUTES
      - `AttributesToGet` に指定した属性のみを返します。
    - COUNT
      - マッチした件数のみを返します。