# AWS DynamoDB ハンズオン

## DynamoDBとは？

AWSマネージドのNoSQLデータベースです。

### NoSQL (Not Only SQL)

RDBMS以外のDBMSを指します。

扱うデータによっては非常に高速になります。

NoSQLの中にもいろいろな種類があります。

- Key-Value store
    - Keyに対してValueを格納する構造
- Document store
    - 自由なデータ構造で「ドキュメント」として格納する
    - XML, YAML, JSON
    - key-value store に含まれる
- Graph
    - グラフ構造
- Column-oriented store （列指向）
    - 列方向にデータを扱う
- Object database
    - オブジェクト指向的にデータを扱う

**DynamoDB は Document store / Key-Value store** に該当します。