---

title: "シンプルなプロンプトを提供する zsh pure を試してみた"
date: 2013-07-17T21:04:00+09:00
comments: true
categories: ["zsh"]
---

この記事は [すごい広島 #9](http://great-h.github.io/events/event-009.html) で行なったことです。

[Pure](https://github.com/sindresorhus/pure) という シンプルなプロンプトを提供する zshスクリプト を導入してみました。

機能としては

* バージョン管理システムのブランチを表示
* git の管理下で、ワークツリーに変更がある場合は `*` が表示される
* 実行の長いコマンドを実行した時に実行時間が表示される
* デフォルトのユーザと違う場合は `ユーザ名@ホスト名`

といった機能があります。

カレントパス と ブランチ名を表示している部分は print を使い出力していて、PROMPT は `>` だけの表示となってます。
そのため `Ctrl+l` を入力すると非常にすっきりした画面になります。

# 設定方法

私は `~/zsh.d/` に zsh のスクリプトを集めていますので、それを前提に書きます。

```
wget https://raw.github.com/sindresorhus/pure/master/prompt.zsh -O ~/.zsh.d/prompt.zsh
```

そして `~/.zshrc` を編集します。

```
# wget https://raw.github.com/sindresorhus/pure/master/prompt.zsh -O ~/.zsh.d/prompt.zsh
local username=''
DEFAULT_USERNAME='eiel'
[ $USER != $DEFAULT_USERNAME ] && local username='%n@%m '
```

ダウンロードした `prompt.zsh` をいじらなくて済むように、少しだけ細工しています。

DEFAULT_USERNAME には自分の ユーザ名を入れます。

導入するとこんな感じになりました。

![pure prompt](/images/pure-prompt.png)

# まとめ

バージョン管理の情報は PROMPT3 に表示していたのですが、コピペするとき邪魔になるので変えたかったので、試しました。

`git submodule` や `zsh のパッケージ管理システム`を使って管理したいですが、今回は wget を使う方法をとりました。
zsh のパッケージ管理システムってあるんだろうか。

特に プロンプトをいじっていない場合は、ここからカスタマイズしていくのも良いのではないでしょうか。
