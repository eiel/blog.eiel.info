---

title: "Devise で登録時にConfirmation Token が不正な値というエラー"
date: 2014-02-20T14:00:00+09:00
comments: true
categories: ["rails","devise"]
---

Devise で確認メールで確認してから有効にする機能 Confirmable の機能をつかってたのだけど、トークンが不正というエラーがおきた。

この問題は devise 3.1.0 より前で、確認のために送信されるメールの内容をカスタマイズしていれば起きてるんじゃないかと思う。

DBに格納される Token の値が HMAC されるようになったらしい。

* [Use HMAC on tokens stored in the DB ·  143794d · plataformatec/devise · GitHub](https://github.com/plataformatec/devise/commit/143794d701bcd7b8c900c5bb8a216026c3c68afc)

そのため、画面をカスタマイズしている場合、token の取得方法が変えないといけないっぽい。

```diff
-<p><%= link_to 'Confirm my account', confirmation_url(@resource, :confirmation_token => @resource.confirmation_token) %></p>
+<p><%= link_to 'Confirm my account', confirmation_url(@resource, :confirmation_token => @token) %></p>
```

確認メールだけでなく `app/views/devise/mailer` 内のファイル全部変えないといけない気がする。

* confirmation_instructions.html.erb
* reset_password_instructions.html.erb
* unlock_instructions.html.erb

あたりですね。

### 蛇足

v3.1.0 ってリリースされたの9月なんだが…さっき気づいたということは…。とおもって Gemfie.lock を git log --patch Gemfile.lock して devise 検索したら、デプロイされたのは最近だった。

### 参考

直してから探したやつだけど。

* [ruby on rails - Devise "Confirmation token is invalid" when user signs up - Stack Overflow](http://stackoverflow.com/questions/18626544/devise-confirmation-token-is-invalid-when-user-signs-up)

### 関連

* [Devise で email 変更する。 - そんなこと覚えてない](http://blog.eiel.info/blog/2012/12/30/modify-email-on-devise/)
