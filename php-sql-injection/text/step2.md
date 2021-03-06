## 攻撃方法

SQL文を組み立てる際、外部からのパラメータの処理が不適切だと、文字列リテラルの区切りを変更することが可能になり、結果として外部からSQL文のキーワードを注入することが可能になります。具体的にはシングルクオート(`'`)などの特別な意味をもった文字を外部から与えることにより、キーワードの解釈を変更するなどの操作をします。

一例として、データベースを使ったパスワード認証を行うための

```php
// $id と $pwd は外部から与えられるユーザIDとパスワード
$sql = "SELECT * FROM users WHERE id = '$id' AND PWD = '$pwd'"; // SQL文
```

というSQL文を生成するPHPコードで考えてみます。
このSQL文で、外部からのパスワード`$pwd`を`' OR 'a'='a`のように設定されてしまうと、`$sql`の内容は

```sql
SELECT * FROM users WHERE id = '$id' AND PWD = '' OR 'a'='a'
```

となってしまうため、SQL文のWHERE節のAND以降が常に真となり、パスワード認証の意味がなくなってしまいます。

SQLインジェクションの脆弱性を使うことで、以下の不正な操作が可能になります。

* データベースの検索条件を変更する （WHERE節の改ざんなど）
* データベースの内容を取得する (SELECT / UNION SELECT など)
* データベースの内容を更新する (UPDATE / INSERT / DELETE など)
* サーバー上でのOSコマンドの実行やファイルの読み書き
* HTTPリクエストの発行による他のサーバーの攻撃
