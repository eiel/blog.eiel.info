---

title: "そういえば彼氏募集というネタリポジトリがありましたね。真似するならこんな感じかなぁ。"
date: 2014-02-08T17:19:00+09:00
comments: true
tags: ["github","travis","haskell","ネタ"]
---

最近、C++界や、ひろし魔界で暴れているらしい[まさかず氏](https://twitter.com/PG_nonen)が[彼氏募集のリポジトリ](https://github.com/norinori2222/boyfriend_require)を真似して[彼女募集のリポジトリ](https://github.com/minamiyama1994/girlfriend_require)を作成してました。

条件さえ揃えば真似してもよかったのですが、条件が揃ってなかったので真似してませんでした。
気がついたら条件が揃ってたので真似してみることにしてみました。

* [﻿eiel/need_a_girlfriend - GitHub](https://github.com/eiel/need_a_girlfriend)

やったこと

* fork したけど 空のブランチつくって、fork元とはコード的には関係をなくした
* Haskell で DSL したかった。結局、Writer モナドの上に構築した。
* source ブランチを push すると travis で `README.md` を生成して master ブランチに自動で push する
* リポジトリの名前を変更した

### fork したけど 空のブランチつくって、fork元とはコード的には関係をなくした

fork したので、その上から上書きしてもよかったのですが、だいぶ違うし、ゼロからつくりたいけど fork したことは残したいよね。

ということで空のブランチをつくってから作りました。
`git checkout --orphan <branch名>` で空のブランチが作れます。

![ネットワーク](/images/2014-02-08-network.png)

### Haskell で DSL したかった。

README.md は手書きせずにプログラムから生成するようにしてみました。
[まさかず氏](https://twitter.com/PG_nonen)を真似ただけである。

Haskell で DSL 作るのにはどうしたらいいんだろうなぁ。たぶんモナド作ればいいんだろうと、コード書きはじめたけど、途中でよくわからなくなった。
それはそれで別に勉強すればいいやということから途中から Writer モナドでつくりました。

```haskell
background = do
  h1 '背景'
  p 'ほげほげごろごろ'
```

みたいに書きたかった。というか、このように書いてから h1 や p 関数を実装しました。

Writer モナド は tell 関数を呼びだしておくと、 runWriter した時に最終結果と tell した内容が引き出せるようです。

上記の例だと以下のような値が返るように作ってます。

```haskell
((),["# ","背景","\n","\n","ほげほげごろごろ","\n","\n"])
```

あとはリストの内容を標準出力に書きだしました。
リストを後ろにくっつけていくからパフォーマンスがなんか気になるけどどうなんだろう。

[あとはコードでも見てください。](https://github.com/eiel/need_a_girlfriend/blob/master/need_a_girlfriend.hs)

Writer モナド書き換える際、main関数は [`runWriter`](https://github.com/eiel/need_a_girlfriend/blob/master/need_a_girlfriend.hs#L3) の部分をちょっと書き換えたかなーぐらいなものでモナドの使いやすさを感じたような気がします。

### source ブランチを push すると travis で `REAME.md` を生成して master ブランチに自動で push する

基本的には

* [Middleman で作った web サイトを Travis + GitHub pages でお手軽に運用する - tricknotesのぼうけんのしょ](http://tricknotes.hateblo.jp/entry/2013/06/17/020229)

を参考にしました。他の方法も試したけどなかなか手強いので結局この方法にしました。

手順的には travis 上でリポジトリを選択して、処理を `.travis.yml`を記述します。

`.travis.yml` に書いた処理の最後で push しますが、 `-q` やら `2>/dev/null` がついてるせいでなんで失敗してるのか気づきにくいのがちょっと難点でした。
つけないと TORKEN が漏れてしまう。

参考程度に書いた yaml を貼っておきます。

```yaml
language: haskell
install: cabal install mtl
script: ghc need_a_girlfriend.hs
after_success:
  - git remote add deploy https://$GH_TOKEN@github.com/eiel/need_a_girlfriend.git
  - git fetch deploy master
  - git checkout master
  - git merge source --no-edit
  - ./need_a_girlfriend > README.md
  - git add README.md
  - 'git commit -m "Generate Travis JOB $TRAVIS_JOB_NUMBER

https://travis-ci.org/eiel/need_a_girlfriend/builds/$TRAVIS_BUILD_ID"'
  - '[ "x$TRAVIS_BRANCH" == "xsource" ] && git push -q deploy master 2>/dev/null'
branches:
  except:
    - master
env:
  global:
    - secure: "KJG63ZK8zdEboimt/+UOVDUu+cECmvSgsCyEUEQVjMnazxpEaNQbP+lEQv9TWki6eRtr71+vt3LU7H4H8Wm/jURV2WiYe31ZeE7wvRcjjaHRWHYfeTJ5OyBJhCJoauKBAwL/jIFSTDt3IEgGIW42WPwagGexHKm+Vh/0ETK1CNc="
    - GIT_COMMITTER_NAME="name"
    - GIT_COMMITTER_EMAIL="email@example.com"
    - GIT_AUTHOR_NAME="name"
    - GIT_AUTHOR_EMAIL="email@example.com"
```

after_success が script の実行に成功した場合に実行されます。

source ブランチの時にしか push しないような処理をいれてあります。
master ブランチの際は after_success がそもそも走らないようにしています。

コミットメッセージを作るのに環境変数から情報を得ています。
どんな環境変数があるかは

* [Travis CI: The Build Environment](http://docs.travis-ci.com/user/ci-environment/)

に書いてあります。
環境変数は travis の画面上では展開しない仕様になってるみたいです。

その他の参考文献

* [Travis CI: Building a Haskell Project](http://docs.travis-ci.com/user/languages/haskell/)
* [Travis CI: Custom Deployment](http://docs.travis-ci.com/user/deployment/custom/)

### リポジトリの名前を変更した

「require ってなんか違う気がするんだよなー」って感覚がしたので調べたら一般的には I need a boyfriend とか I need a girlfriend と書いてる例があったので、リポジトリ名は need_a_girlfriend にしました。

> eiel / need_a_girlfriend

となるので、なんか文法的にも良さげな気がします。

### まとめ

しかし、元のリポジトリの条件の多さにはびっくりする。
お互い悪いところは相性みながらちょっとづつ調整できないものなのでしょうか。
これだけは譲れないものはひとつかふたくぐらいあれば充分じゃないんでしょうか。
私にはよくわからないですけど。

日々、成長するのを放棄した人間にはなりたくないかなぁ。

そんなことはともかく[まさかず氏](https://twitter.com/PG_nonen)はすごくがんばっていると思うので素敵な彼女ができるように応援したいですね。

ではでは。
