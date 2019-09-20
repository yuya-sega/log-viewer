## 各コンテナのログをelasticsearchで収集し、kibanaで可視化する。

### 構成
fluentd 1.6.2-1.0
elasticsearch 7.3.2
kibana 7.3.2

### 起動方法

```
docker-compose build && docker-compose up
```

### 確認方法

http://localhost:8010/
にアクセスして、apacheのaccessログを出力させる

```
docker-compose exec postgres psql -U postgres -d test
```
コンソールから、上記のコマンドを実行して、postgresのログを出力させる。

### kibanaで確認

http://127.0.0.1:5601
上記にアクセスして、kibana画面を表示させる。
サイドメニューの「Manegement」を選択して、Index Patternsの定義を行う。

#### apacheログ
1. 「Create Index Pattern」ボタンを押下する
2. 「Index Pattern」に「apache.access-*」と入力して、「Next Step」ボタンを押下する
※ 「Nest Step」ボタンが押下できない場合は、elasticsearchのログ収集タイミングの問題なので、30秒程度待つ
3. 「Time Filter field name」のプルダウンボックスから「@timestamp」を選択し、「Create Index Pattern」ボタンを押下する
4. 1~3までの作業が完了することで、Discoverからログを確認できるようになる。

#### postgresログ
apacheログのIndex Pattern登録作業と一緒だが、2で「apache.access-*」と入力したところを「postgres.query-*」に読み替える

