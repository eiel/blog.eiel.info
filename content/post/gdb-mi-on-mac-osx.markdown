---

title: "Macで M-x gdbするとうまく動かない…"
date: 2012-12-27T00:59:00+09:00
comments: true
categories: ["emacs","mac"]
---

まず要点から。

新しめの emacs でMac上で `M-x gdb` がうまくうごきません。
`M-x gud-gdb` は動くことがわかりました。
`gud-gdb`を使う場合は`gdb --fullname command`で動かすと良いです。

原因ですが、Macの gdb が古い模様。最近の Emacsに添付されてる gdb は gdb/mi インターフェイス(よくわかってない)でやりとりするようで、一応うごくけどわけのわからぬ動作をするみたいです。

[gdbmi](http://users.snap.net.nz/~nickrob/#GDBMI)

Gentoo-Prefix をつかって gdb いれてみたものの run ができませんでした。ツールチェーンまわりはよくわからないのでなんとも言えません。

gud-gdb ですが、これは gdb よりも機能が弱いものの模様です。古いeamcsがこれと同等のもので動いてるのかもしれませんが、調べていません。gdbに比べたら便利な機能は減るかもしれませんが、十分使えそうです。

LLDB/MI みたいなものがあれば良いのでしょうが、よくわかりません。

このあたりはMac使ってて不便なところですね。
