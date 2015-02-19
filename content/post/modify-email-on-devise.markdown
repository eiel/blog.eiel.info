---

title: "Devise で email 変更する。"
date: 2012-12-30T14:41:00+09:00
comments: true
categories: ["rails","devise","ruby"]
---

Railsの plugin で 認証を行なう [devise](https://github.com/plataformatec/devise) という gem があります。
このユーザ認証で `実際にユーザにメールを送信して、登録を完了する`という機能を提供するのに  confirmable という機能があります。

このConfirmableという機能を使用していると管理者が ユーザのメールアドレスを変更してあげる必要がある場合、代えるときもメールがユーザに送信されます。これが便利なときもあったりテスト時にこまったりすることがあります。

`devise :confirmable` した モデルには `skip_confirmation!` `skip_reconfirmation!` というメソッドが追加されてるので、これらを呼び出すことで回避することができます。

ちなみに、これらのメソッドの中身をみると
```ruby
def skip_confirmation!
  self.confirmed_at = Time.now.utc
end

def skip_reconfirmation!
  @bypass_postpone = true
end
```
となってます。

confirmed_at に値がはいっていれば有効で、@bypass_postpone が true で メールの送信が回避できそうですね。このあたりの実装はversionによって変更される恐れがあるので直接利用するには注意が必要です。
