# MeCab Webサービス

docker-composeコマンドだけで起動できるMeCabサービス

* flask-mecab
  * MeCabを利用できるRESTfulなflaskサーバー
  * 新語辞書mecab-ipadic-neologd

## How to Deploy

```bash
gcloud run deploy mecab-service --source . --region us-west1 --allow-unauthenticated --ingress=internal
```

## 実行方法
HTTPリクエスト

```text
POST /mecab/v1/parse-neologd
```

リクエストヘッダ

```text
Content-Type: application/json
```

リクエストボディ

```json
{
  "sentence": 文字列
}
```

## 実行例 mecab-ipadic-neologd

mecab-ipadic-neologdは固有名詞に強い辞書です。

```shell-session
$ curl -X POST http://localhost:5000/mecab/v1/parse-neologd \
       -H "Content-type: application/json" \
       -d '{"sentence": "関数型プログラミング"}'  | jq .
```

```json
{
  "dict": "neologd",
  "message": "Success",
  "results": [
    {
      "原型": "関数型プログラミング",
      "品詞": "名詞",
      "品詞細分類1": "固有名詞",
      "品詞細分類2": "一般",
      "品詞細分類3": "*",
      "活用型": "*",
      "活用形": "*",
      "発音": "カンスーガタプログラミング",
      "表層形": "関数型プログラミング",
      "読み": "カンスウガタプログラミング"
    }
  ],
  "status": 200
}
```
