---

title: "CodeClimate のカバレッジは SimpleCovの設定がしてあるところだとうまくいかない"
date: 2014-01-20T14:42:00+09:00
comments: true
categories: ["ruby"]
---

[CodeClimate](https://codeclimate.com) で、課金して利用しているとカバレッジを取得する機能がある。

* [設定方法はここに書かれてる](https://codeclimate.com/docs#test-coverage)

中身は SimpleCov らしく SimpleCov の設定より先に書いていたらうまくうごかなかった。
仕方ないので削除してみたら、うまくうごきました。

SimpleCov のフォーマッタを差し替えているらしく、両方使いたい場合は、MultiFormatter を使えばよいらしい。

```
SimpleCov.formatter = SimpleCov::Formatter::MultiFormatter[
  SimpleCov::Formatter::HTMLFormatter,
  CodeClimate::TestReporter::Formatter
]
```

しかし、CodeClimate ではカバー率を表示するだけっぽいのであまり使いどころがない感じでした。
