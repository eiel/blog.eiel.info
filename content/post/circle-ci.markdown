---

title: "Circle CI を試した"
date: 2013-10-30T11:55:00+09:00
comments: true
categories: ["ci", "hwebsys"]
---

[2013年10月の広島Webシステム開発勉強会](https://twitter.com/hwebsys)内で[Cicle CI](https://circleci.com) を試していた。

最安値が19ドルで、この場合プライベートリポジトリがひとつ使える。

Rails プロジェクトで試しました。設定した内容はわずかで、`.ruby-version` の指定をしただけで、テストの実行することができました。
これはこのプロジェクトが ruby 2.0 以上である必要があるからです。
リポジトリの選択も Github から一覧が取得されているので、一覧から選択するだけでした。

* `config/database.yml` は自動で生成されました。
* デフォルトでは並列ビルドされず、20分ぐらい実行にかかりました
* Edit settings から 6つに分けて並列に実行できました。4分ぐらいで終了するようになりました
* GitHub に Pull Request があれば merge するときにビルド実行結果がわかります
現在、タイムゾーンの影響で失敗してテストがあるので修正方法を模索しているところです。

並列ビルドは rspec、 cucumber それぞれ、自動で6分割して実行されていました。
一部のcucumberがかなり素早く終了したのでファイル単位で分割しているのだと思います。
[その辺のカスタマイズ方法はここに書いてある](https://circleci.com/docs/parallel-manual-setup)

結局 `circle.yml` は、これだけしか書いていない状態。

```yaml
machine:
  ruby:
    version: 2.0.0-p247
  timezone: Asia/Tokyo
```

導入がとても簡単で維持費が EC2 のマイクロを立ち上げっぱなし程度です。
コンテナの数ではなくて、プライベートリポジトリの数での課金なのでアクティブなプロジェクトでのみ使用する使い方になりそうです。
そんな使い方が可能なのかはまだよくわからない。
