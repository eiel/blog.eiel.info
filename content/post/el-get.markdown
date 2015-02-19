---

title: "自分の el-get のワークフローについて整理する"
date: 2013-08-07T19:07:00+09:00
comments: true
tags: ["emacs","el-get"]
---

この記事は [すごい広島 #12](http://great-h.github.io/events/event-012.html) で書きました。

Emacs のパッケージ管理に `el-get` を利用しています。
他にもいろいろありますが、どれもしっかりと使いこなせないまま `el-get` におちついています。
ぶっちゃけ、`el-get` 使いこなせていません。
使いこなせてないので不安な箇所があったので確認してまとめました。

`el-get` を使いこなせてないのに、これに落ち着いている理由は`Distributed Setup` という機能を利用しているからです。
これを使うと インストールしているパッケージがない場合自動でインストールしてくれます。

例えば、`helm` というパッケージを利用していて、emacsの設定ファイルを git などで共有しているとします。普段利用していない別のマシンで設定ファイルをコピーしてきます。
これで Emacs を起動すると、起動時にエラーが出てしまいます。
もし el-get の `Distributed Setup` の機能を利用していればインストールしていないパッケージをダウンロードしてくれます。
ドロップボックスで共有している場合は不要なのですが、そういった使い方をしていない場合は有効です。

この方法を使っている場合の設定方法とパッケージの更新方法について書いておきます。

### 設定方法

その設定方法は

```common-lisp
(el-get 'sync 'helm)
```

と、書いておきます。
基本的にはこれでOKです。

一緒に yasnippet をインストールしたいのであれば リストにすればよいです。

```common-lisp
(el-get 'sync '(helm yasnippet))
```

たぶんリストにしなくても大丈夫。

```common-lisp
(el-get 'sync 'helm 'yasnippet)
```

別々にやっても大丈夫。

```common-lisp
(el-get 'sync 'yasnippet)
(el-get 'sync 'helm)
```

すでにインストールしてしまっていて、動作を確認したい場合は `el-get-remove`コマンドで削除して、再評価することで動作確認できます。
`rm` コマンドなどで削除してしまった場合は el-get から見ると削除したことになっていないので注意してください。
その場合でも `el-get-remove` を呼べば大丈夫です。

`el-get` 関数はダウンロードしていないパッケージの場合はダウンロードするという動作をします。
第1引数の 'sync というを付けておくと同期実行になります。
ファイルのダウンロードを待つことになります。
nil にすることで非同期にも実行できます。
設定を el-getの hook に書いたりする必要がでてくると考えられますが試してません。同期実行するほうが設定しやすいと思います。

第2引数以降にはパッケージ名を指定することになります。
パッケージ名`el-get-list-package`コマンドなどで確認できます。
パッケージ名を省略した場合は インストール済のパッケージのリストが入るようです。
`el-get` 関数は他にも load-path を通したりもするようです。

あとは `el-get` の自動でインストールするコードを加えて

```common-lisp
(add-to-list 'load-path "~/.emacs.d/el-get/el-get")
(unless (require 'el-get nil 'noerror)
  (with-current-buffer
      (url-retrieve-synchronously
       "https://raw.github.com/dimitri/el-get/master/el-get-install.el")
    (goto-char (point-max))
    (eval-print-last-sexp)))
(el-get 'sync 'helm)
;; helm の設定
(el-get 'sync 'yasnippet)
;; yasnippet の設定
```

という感じにしておくと設定ファイルをコピーするだけで設定が完了します。
<small>とはいえ、だいたいなにかがトラブりますが。</small>

この方法を使っている場合は `el-get-install` はとりあえず試してみたいパッケージをインストールするという感じで使えます。
「他の環境でも使いたいなぁ」とか、設定いじりたいなぁ。となれば追記すれば良いのではないかと思います。
パッケージを更新をした場合は hook により設定が上書きされるんじゃないかと気になっていたりもしますが、未確認です。

### パッケージの更新

パッケージの更新するには `el-get-update` でパッケージごとに更新するか、
`el-get-update-all` でまとめて更新できます。
適当な頻度で実行すればよいと思います。

### まとめ

よくわかってないけど、自分が最低限必要なことはできているという使い方をしています。
`(el-get 'sync)` まわりの部分について確認したので、記事にしました。
もうちょっと使いこなしたい。

もっと詳しいことは info などを読むのが良いと思います。

* [github elget/elget.info](https://github.com/dimitri/el-get/blob/4.stable/el-get.info)
