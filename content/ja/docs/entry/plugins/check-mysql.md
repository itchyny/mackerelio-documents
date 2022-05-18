---
Title: チェックプラグイン - check-mysql
Date: 2022-04-26T11:53:19+09:00
URL: https://mackerel.io/ja/docs/entry/plugins/check-mysql
EditURL: https://blog.hatena.ne.jp/mackerelio/mackerelio-docs-ja.hatenablog.mackerel.io/atom/entry/13574176438086440922
CustomPath: plugins/check-mysql
---

`check-mysql` は MySQLの監視を行うチェックプラグインです。いくつかのサブコマンドに応じた条件で監視ができます。プラグインを使用するには [チェック監視に公式チェックプラグイン集を使う](https://mackerel.io/ja/docs/entry/howto/mackerel-check-plugins) をご覧の上、監視を実行するホストにインストールを行ってください。

[:contents]

### サブコマンド

`check-mysql` で使用可能なサブコマンドは以下の通りです。

|コマンド|監視内容|参照先|備考|
|---|---|---|---|
|uptime|MySQLサーバの稼働時間|`SHOW GLOBAL STATUS` の Uptime||
|readonly|MySQLサーバが読み取り専用になっているか|`SHOW GLOBAL VARIABLES` の read_only||
|replication|MySQLサーバのレプリケーションの状態|`SHOW REPLICA STATUS`<br>・IO_Running および SQL_Running がどちらも Yes の状態か<br>・Seconds_Behind の値が閾値を超えているか|・接続するユーザに<br>REPLICATION CLIENT 権限が必要<br>・MySQL 8.0.22 より前のバージョンは<br>`SHOW SLAVE STATUS` の結果を参照|
|connection|MySQLサーバのコネクション数|`SHOW GLOBAL STATUS` の Threads_connected||

### 実行例

プラグインは手動での実行が可能です。

```
check-mysql connection --host=127.0.0.1 --port=3306 --user=USER --password=PASSWORD --warning=250 --critical=280
```

```
check-mysql replication --host=127.0.0.1 --port=3306 --user=USER --password=PASSWORD --warning=5 --critical=10
```

## 詳細

### 共通オプション

以下のオプションは `check-mysql` のすべてのサブコマンドで利用可能です。

|オプション|省略形|説明|デフォルト値|
|---|---|---|---|
|--host=|-H|対象のホスト|localhost| 
|--port=|-p|接続ポート|3306|
|--socket=|-S|ソケットファイルの指定||
|--user=|-u|ユーザ名|root|
|--password=|-P|パスワード||
|--tls||TLS 接続を有効にする||
|--tls-root-cert=||TLS 接続時のルート証明書の指定||
|--tls-skip-verify||TLS 接続時に証明書を検証しない||
|-help|-h|コマンドのヘルプを表示<br>`check-mysql connection -h` のように実行するとサブコマンドごとのヘルプを表示||

### 閾値オプション

以下の閾値に関するオプションはサブコマンドによって条件が異なります。オプションの指定がない場合はデフォルト値が閾値になります。

|サブコマンド|オプション|省略形|説明|デフォルト値|
|---|---|---|---|---|
|uptime|--critical=<br>--warning=|-c<br>-w|uptime が指定秒数より少ない場合|0  (critical)<br>0 (warning)|
|replication|--critical=<br>--warning=|-c<br>-w|Seconds_Behind が指定秒数より少ない場合|250  (critical)<br>200 (warning)|
|connection|--critical=<br>--warning=|-c<br>-w|Threads_Connected が指定秒数より少ない場合|250  (critical)<br>200 (warning)|
|uptime|なし|||

### エージェントへの設定例

以下は `mackerel-agent.conf` への設定例です。

```
[plugin.checks.check-mysql-sample]
command = ["check-mysql", "connection", "--host", "127.0.0.1", "--port", "3306", "--user", "USER", "--password", "PASSWORD", "--warning", "250", "--critical", "280"]
```

## Tips

#### <u>MySQL の接続情報の記述方法</u>

`check-mysql` の実行に必要な接続情報は `--password` オプションに直接記述する以外の方法もあります。（command の内容を配列指定ではなく、文字列指定で記述する必要があります。）

* **コマンドを利用する**

文字列指定で記述した場合 `command` の内容はシェル経由の実行となりますので、パスワードを出力するコマンドの利用も出来ます。

`mackerel-agent.conf`
```
command = "check-mysql ・・・ --password="パスワードを出力するコマンド"
```

* **環境変数を利用する**

設定ファイルに環境変数を定義してオプションに変数を指定することが出来ます。

`mackerel-agent.conf`
```
command = "check-mysql ・・・ --password=$MYSQL_PASSWORD ・・・"
env = { "MYSQL_PASSWORD" = "xxxxx" }
```

* **別ファイルに記述した変数を利用する**

Mackerel エージェントは `/etc/sysconfig/mackerel-agent` に記述した変数を利用出来ます。

`mackerel-agent.conf`
```
command = "check-mysql ・・・ --password=$MYSQL_PASSWORD ・・・"
```

`/etc/sysconfig/mackerel-agent`
```
MYSQL_PASSWORD=xxxxx
```

## トラブルシューティング

- `Couldn't ...` というエラーメッセージが出力される
  - 接続情報が誤っているか、接続するユーザに MySQL の情報を閲覧する権限が与えられていない可能性があります。<br>`check-mysql` をホスト上で手動実行して情報の取得が可能かご確認ください。

## リポジトリ

https://github.com/mackerelio/go-check-plugins/tree/master/check-mysql
