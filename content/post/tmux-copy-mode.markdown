---

title: "tmux で copy-mode でのキーバインドについて"
date: 2013-02-17T12:45:00+09:00
comments: true
tags: ["tumx","emacs","vim"]
---

tmuxのコピーモードでのキーバインドはデフォルトで emacs like になっています。

なぜか、一部のマシンでのみ vi like になってたので、`man tmux` をみてみた。
>  the default is emacs, unless VISUAL or EDITOR contains ‘vi’.

デフォルトは emacs だけど環境変数 VISUAL か EDITOR に 'vi' を含んでいれば vi like になるそうです。

環境変数に影響されずに固定したい場合は

```
set-window-option -g mode-keys emacs
```

とか

```
set-window-option -g mode-keys vi
```

とか、書いておきましょう。
