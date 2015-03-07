---
date: 2015-03-07T12:00:00+09:00
title: "OctopressからHugoに移行した マルチコアをもっと使いTai - LT駆動開発12"
---

[LT駆動開発12](https://github.com/LTDD/Sessions/wiki/LT%E9%A7%86%E5%8B%95%E9%96%8B%E7%99%BA12)の発表者資料です。テーマ 「祝一周年&ハーレム」です。

ブログをOctopressからHugoに移行したのでその再に感じたことを発表しました。

<script async class="speakerdeck-embed" data-id="37a5d766667749f8861476cae2daf0cf" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

Goはバランスがとれているなぁ、と最近感じていたりはします。

どちらにしろ、最近CPUは速くなってはいるだろうけど体感できないのは感じていたりしてました。
バッテリーの持ちはよくなってるけど。

前置きはさておき、普段ローカルで実行してるようなプログラムがマルチコアは生かせてないというのは非常に体感する出来事でした。
とはいえ、ウェブサービスをつくっていたりすると並列化なんてする必要はないだろうし、スマホアプリなら複雑な処理はAPIサーバにやらせればいいだろうし、
並列化が必要な場面は特定分野にはなるんだろうとは思います。

### Hugo移行の歳にしたこと

移行の際にしたことを記録しておきます。

日付のフォーマットの修正。

```
find . -type f -exec sed -i "" -e 's/date: \([0-9]\{4\}-[0-9]\{2\}-[0-9]\{2\}\) \([0-9]\{2\}:[0-9]\{2\}\).*$/date: \1T\2:00+09:00/g' {} \;
```

layoutパラメータは指定しないので削除。

```
find . -type f -exec sed -i "" -e 's/layout: .*//g' {} \;
```

カテゴリではなくてタグに変更

```
find . -type f -exec sed -i "" -e 's/categories/tags/g' {} \;
```

あとは、パーマリンクがずれないようにパーマリンクを指定しています。

```
permalinks:
  post: /blog/:year/:month/:day/:filename/
  ```

ただし、ファイル名にも日付を含んでたので、あきらめて削除しました。

[日付でソートできなくてつらい。](https://github.com/eiel/blog.eiel.info/tree/master/content/post)

あとは rss が atom.xml だったのが index.xmlになってしまったので、デプロイする前にcpして対処しました。

https://github.com/eiel/blog.eiel.info/commit/baeea3fca8ee6a4cf987c42e4ab29da6831cb6b1

Werckerをつかってデプロイしていますが、ステップがすでにあってとても楽チンでした。

https://github.com/eiel/blog.eiel.info/blob/baeea3fca8ee6a4cf987c42e4ab29da6831cb6b1/wercker.yml

```
box: wercker/default
build:
  steps:
    - arjen/hugo-build:
        version: 0.13
    - script:
        name: create atom.xml
        code: cp public/index.xml public/atom.xml
deploy:
  steps:
    - lukevivier/gh-pages@0.2.1:
        token: $GIT_TOKEN
        domain: blog.eiel.info
        basedir: public
```

おまけでHugoのテーマでそのまま使えたのは

* hyde
* purehugo
* redlounge
* simple-a
* tinyce

でした。手軽に移行したいなら参考にどうぞ。

記事が増えて、生成速度が問題にならないように今後JekyllやOctopressでも対処されていくことだと思いますので、無理して移行する必要はないと思います。

つらい場合は検討してみてはどうでしょうか。

個人的には下書きだけ生成してくれるオプション欲しいです。(自分でつくれ)

<iframe src="http://rcm-fe.amazon-adsystem.com/e/cm?lt1=_blank&bc1=000000&IS2=1&bg1=FFFFFF&fc1=000000&lc1=0000FF&t=eiel-22&o=9&p=8&l=as4&m=amazon&f=ifr&ref=ss_til&asins=4873116899" style="width:120px;height:240px;" scrolling="no" marginwidth="0" marginheight="0" frameborder="0"></iframe>


### 参考文献

* [OctopressからHugoへ移行した | SOTA](http://deeeet.com/writing/2014/12/25/hugo/)
