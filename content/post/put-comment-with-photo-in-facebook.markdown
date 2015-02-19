---

title: "RubyでFacebookのコメントに写真を投稿する"
date: 2014-09-28T12:44:00+09:00
comments: true
tags: ["ruby","facebook","api"]
---

FacebookのGraph APIをさわった。コメントを自動でしたかったからだ。

RubyでFacebook Graph APIをたたくにはサードパーティなgemを使うのでいろいろい種類がある。今回は[koala](https://github.com/arsduo/koala)を使用した。

[コメントに関するAPIの仕様はここに書いてある](https://developers.facebook.com/docs/graph-api/reference/v2.1/object/comments)

```ruby
require 'kola'

object_id = "OBJECT_ID"
file_path = "FILE_PATH"
oauth_access_token = "ACCESS_TOKEN"

file = Koala::UploadableIO.new(file_path)
graph = Koala::Facebook::API.new(oauth_access_token)
graph.put_object(object_id,"comments", source: file)
```

で、投稿できる。`graph.put_comment` というメソッドがあるが、ファイルがわたせない。コメントしたいだけならこれで充分。


object_id が必要になる。
てっとり早い方法はブラウザでコメントしたいポストを開いたらURLに書いてある数字がobject_idである。

画像はsourceに指定するが Koala::UploadableIOで開いておく。

またoauth_tokenが必要になる。
[Graph API Explorer](https://developers.facebook.com/tools/explorer?method=GET&path=me%3Ffields%3Did%2Cname&version=v2.1)(アクセスにはログインしている必要がありそう)``から作成できる。たぶん1、2時間しかつかえないはず。(ちゃんと調べてない)

権限は

* user_photos
* publish_actions

が最低必要で、書き込みする場所によって対応したものが必要。

* user_status
* user_groups

などなど。必要に応じて追加する。

user_photosをつけてなくて3時間ぐらい悩んだのはただの愚痴。

ついでにRestClientで送りつける場合の方法

```ruby
require 'rest_client'
url = "https://graph.facebook.com/v2.1/#{object_id}/comments?oauth_token=" + oauth_access_token
RestClient.post(url, "source" => open("/Users/eiel/Desktop/hoge.png"))
```

### 参考文献

* [Ruby: How to post a file via HTTP as multipart/form-data? - Stack Overflow](http://stackoverflow.com/questions/184178/ruby-how-to-post-a-file-via-http-as-multipart-form-data)
