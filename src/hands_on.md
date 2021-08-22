# ハンズオン

ローカル環境でDocker, JavaScript SDK を使用して DynamoDB を触ってみましょう。

## 起動方法

```bash
git clone git@github.com:nagaokayuji/workshop-dynamodb.git
```

プロジェクト配下に移動し、以下を実行します。

```bash
docker-compose -f code/docker-compose.yml up
```

- [http://localhost:8000/shell/](http://localhost:8000/shell/) にアクセス
  - `DynamoDB JavaScript Shell` というタイトルの画面が出れば準備完了

## GUI アプリケーションの紹介

今回のハンズオンではインストールしなくても問題ありませんが、
DynamoDB 用のクライアントで NoSQL Workbench というものがあります。

以下のようにテーブルの項目を閲覧したり、インタラクティブにオペレーションを実行することができます。
![image](https://res.cloudinary.com/ddaz9etkx/image/upload/v1629657624/202108/ss_cgqn0l.png)

[NoSQL Workbench のダウンロード - Amazon DynamoDB](https://docs.aws.amazon.com/ja_jp/amazondynamodb/latest/developerguide/workbench.settingup.html)
