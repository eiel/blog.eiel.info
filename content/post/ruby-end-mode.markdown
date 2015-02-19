---

title: "ruby-end マイナーモード - emacs"
date: 2013-10-22T14:41:00+09:00
comments: true
categories: ["emacs", "ruby"]
---

[ruby-end](https://github.com/rejeep/ruby-end) モードをいれました。

ruby で end を自動挿入してくれるマイナーモードです。

似たようなマイナーモードとしては ruby-elecric-mode があります。
この子は他にもいろいろ機能をもっています。end の補完も機能のひとつです。
個人的にはかなか挙動が使いづらく end の補完の処理だけ利用していました。

また、emacs24 では微妙な挙動をしたりするそうです。 - [参考: Emacs24 で ruby-electric的なruby-modeを実現するには - メモとか]

ruby-end は end が挿入されるタイミングが心地良いので試してみています。

### インストール

epel でインストールできるので `M-x package-install` で `ruby-end` で入力でインストールできます。

* [参考: package.el - EmacsJP](http://emacs-jp.github.io/packages/package-management/package-el.html)

普通は package.el で充分かと思いますが、私は el-get を利用してるので `(el-get 'sync 'ruby-end)` を設定ファイルに書いて評価しました。

### 関連

* [自分の el-get のワークフローについて整理する](http://blog.eiel.info/blog/2013/08/07/el-get/)
