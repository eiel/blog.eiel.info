---

title: "Travis-CI でコミットして GitHub にプッシュする - 公開鍵認証を利用してみる"
date: 2014-02-18T20:25:00+09:00
comments: true
tags: ["github"]
---

静的サイトジェネレータとGitHub Pagesを使っていると、Travis-CIでHTMLを生成してコミットを行い、masterを自動で更新して欲しいですね。

普通なら下記の記事の方法で充分でした。

* [Middleman で作った web サイトを Travis + GitHub pages でお手軽に運用する - tricknotesのぼうけんのしょ](http://tricknotes.hateblo.jp/entry/2013/06/17/020229)

しかし、 Organization のリポジトリに対してこの方法を使うとメンバーが私個人のリポジトリを操作ができる気がする。
仕方ないので別の方法を模索してみた。

GitHub には、リポジトリごとに公開鍵を追加する機能があったのでこれを使ってみました。

考えないといけないことは、秘密鍵をどうやってTarvisへもっていくかです。
秘密鍵を共通鍵で暗号化して、リポジトリに追加する方法を選んでみました。
共通鍵を `.travis.yml` の中に暗号化してに保存しておきます。
この共通鍵の復号は Travis 側で自動的にされます。この共通鍵を使い Travis 側でリポジトリに含まれる秘密鍵を復号します。

秘密鍵さえ手に入れば GitHub に push できます。


やることを整理します。

* Travis-CI の設定
* 秘密鍵と公開鍵の作成
* 秘密鍵を暗号化するための共通鍵の生成
* 秘密鍵の暗号化してリポジトリに追加
* GitHub のリポジトリに公開鍵を追加
* `.travis.yml` へ暗号化した共通鍵を設定
* `.travis.yml` にGitHubへ pushする処理などを記述

### Travis-CIの設定

割愛します。

ログインして、設定したいリポジトリをONにします。

## 秘密鍵と公開鍵の作成

ssh-gen コマンドを使います。
作る鍵を `deploy_key` として進めます。

```
$ ssh-keygen -f deploy_key
```

`deploy_key` と `deploy_key.pub` が生成されます。

### 秘密鍵を暗号化するための共通鍵の生成

適当につくります。shell変数 password に保存しておく例を書いておきます。

```
$ password=`cat /dev/urandom | head -c 10000 | openssl sha1`
```

### 秘密鍵の暗号化してリポジトリに追加

さっき作成した共通鍵で `deploy_key` を暗号化して `deploy_key.enc`を作成します。

```
$ openssl aes-256-cbc -k "$password" -in deploy_key -out deploy_key.enc -a
```

あとは適当にコミットします。 `deploy_key` をコミットしないように気をつけてください。

```
$ git add deploy_key.enc
$ git commit -m 'Add deploy key'
```

### GitHub のリポジトリに公開鍵を追加

`deploy_key.pub` をGitHubに登録します。

GitHubのリポジトリのページを表示して、`設定` > `Deploy keys` > `Add deploy key` で登録できます。区別がつくように名前は好きにつけましょう。

![](/images/2014-02-18-github-push.png)

### `.travis.yml` へ暗号化した共通鍵を設定

travis gem をインストールしていない場合はインストールしましょう。

```
$ gem install travis
```

`travis encrypt` コマンドを使用します。

```
$ travis encrypt -r [ユーザ名や組織名]/[リポジトリ名] "SERVER_KEY=$password" -a
```
.travis.yml の env.global へ情報が記録されます。

[すごい広島](https://github.com/great-h/great-h.github.io)を例にすると、こんな感じになります。

```
$ travis encrypt -r great-h/great-h.github.io "SERVER_KEY=$password" -a
```

### `.travis.yml` にGitHubへ pushする処理などを記述

`.tarvis.yml` の `after_success` にやりたいことを書きましょう。

鍵の設定の部分はこんな感じ。

```
after_success:
  - echo -e "Host github.com\n\tStrictHostKeyChecking no\nIdentityFile ~/.ssh/deploy.key\n" >> ~/.ssh/config
  - openssl aes-256-cbc -k "$SERVER_KEY" -in .travis/deploy_key.enc -d -a -out deploy.key
  - cp deploy.key ~/.ssh/
  - chmod 600 ~/.ssh/deploy.key
```

あとは煮るなり焼くなり。

すごい広島での例を上げておきます。
`_site` にファイルが生成されているので、それを master ブランチにコミットしてプッシュしています。

```
language: ruby
rvm:
  - 2.1.0
after_success:
  - echo -e "Host github.com\n\tStrictHostKeyChecking no\nIdentityFile ~/.ssh/deploy.key\n" >> ~/.ssh/config
  - openssl aes-256-cbc -k "$secret" -in .travis/deploy_key.enc -d -a -out deploy.key
  - cp deploy.key ~/.ssh/
  - chmod 600 ~/.ssh/deploy.key
  - git clone git@github.com:great-h/great-h.github.io.git -b master
  - cd great-h.github.io
  - cp -a ../_site/ .
  - git add --all
  - 'git commit -m "Generate Travis JOB $TRAVIS_JOB_NUMBER

https://travis-ci.org/great-h/great-h.github.io/builds/$TRAVIS_BUILD_ID"'
  - '[ "x$TRAVIS_BRANCH" == "xsource" ] && git push origin master'
branches:
  except:
    - master
env:
  global:
    - secure: "gIC6PLCnYmO29FiGqA1ZpVFsGBWbbdkZJGcBwYL2kyav3fPwdxRe6+RG3WEUfY2qwFnI52Br7pQ4ZClaBD76abObmYFW8Qkd13bgxgYMHFFzDh6ACMoY/JvRu4SXZcqiSi2QzeDTRk8Q825kGNY3QJXb4NiZ9gj8uAR9bNpnqnc="
```

### まとめ？

暗号の強度して十分なのか検証していない。復号できたとしても、できることは限られてるので、とりあえず、妥協している。

そういえば、master に作成したファイルをコミットしているのですが、master を push した際に travis が走るというバグに悩まされました。
`.travis.yml` には以下のように書いておけば master は無視されます。

```
branches:
  except:
    - master
```

実は master には `.travis.yml` を置いていないのが原因でした。

Travisの設定で `.travis.yml` が無ければ動かさないという設定もできますし、適当に master に .travis.yml を置いておくのも良いと思います。

他にも `.travis.yml` に秘密鍵を保存する手法をいくつかみかけました。
長さが足りないので分割して保存して、Travis側で結合して使うようです。

* [squeezing private SSH key into .travis.yml file](https://gist.github.com/lukewpatterson/4242707)


### 参考文献

* [Managing deploy keys · GitHub Help](https://help.github.com/articles/managing-deploy-keys)
* [Travis CI: Travis Pro](http://docs.travis-ci.com/user/travis-pro/#How-can-I-encrypt-files-that-include-sensitive-data%3F)
* [Getting "data too large for key size" for 128 character	length secret_key_base var · Issue #41 · travis-ci/travis · GitHub](https://github.com/travis-ci/travis/issues/41)
* [gitにsshで接続（ポートと秘密鍵を指定) - まじめにゆいがどくそん](http://nilfigo.hatenablog.com/entry/20130705/1373000104)

### 関連

* [Github Pages について整理しておきます - そんなこと覚えてない](/blog/2013/02/17/github-pages/)
* [Github で Jekyll を使う時に調べたこと - そんなこと覚えてない](/blog/2013/02/18/jekyll-on-github/)
* [そういえば彼氏募集というネタリポジトリがありましたね。真似するならこんな感じかなぁ。 - そんなこと覚えてない](/blog/2014/02/08/i-need-a-girlfriend/)
