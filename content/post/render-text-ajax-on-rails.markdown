---

title: "Rails で ajax をちょっと実験するときに `render text:` すると コールバックしてこない。 "
date: 2013-04-03T19:49:00+09:00
comments: true
categories: ["ruby","rails","ajax"]
---

サンプルコードがなくて申し訳ないです。

rails で ajax を使用とすると 一般的には [jquery-ujs](https://github.com/rails/jquery-ujs) を利用します。

あるリンクをクリックする時に非同期に読込みたい場合は

```ruby
<%= link_to 'hoge', @user, class: user-link %>
```

に対して、 remote: true を追加します。

```ruby
<%= link_to 'hoge', @user, remote: ture, class: user-link %>
```

といった感じになります。
あとはサーバからのレスポンスが返ってきた時の処理を書きましょう。


```javascript
$(function () {
   $(document).on('ajax:success', '.user-link', function (ujs, content, status, xhr) {
    $('#user-info').html(content);
  });
});
```


少し説明不足ですが、気にせず。

さて、ここで リンク先が未実装で手抜きして:

```ruby
class UsersController
  def show
    render text: 'hoge'
  end
end
```

と、さくっと実装しちゃうと さきほど実装した javascript の コールバックが呼ばれません。
めんどくさがらずに、 `app/views/users/show.htmle.rb` などを作成してあげましょう。

そんなこと滅多にないか…。
