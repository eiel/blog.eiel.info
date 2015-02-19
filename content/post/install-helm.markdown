---

title: "Helmをインストールしてみた"
date: 2012-04-04T11:25:00+09:00
comments: true
tags: [emacs,helm]
---
anythingのフォークである[helm](https://github.com/emacs-helm/helm)を試してみた。
試した環境は GNU Emacs 24.0.94.1 (x86_64-apple-darwin11.3.0, NS apple-appkit-1138.32) of 2012-03-25

* 具合がわるくなってしまう設定

```
(set-file-name-coding-system 'utf-8-hfs)
(setq local-coding-system 'utf-8-hfs)
```


utf-8-hfsを設定してるとhelmのロードがおわらなくなった。何かを収集中に無限ループに落ちいる模様。anythingにできて、helmにできないことはまだまだたくさんあるので、しばらくanythingと共存させてみます。
