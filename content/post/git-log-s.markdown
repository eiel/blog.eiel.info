---

title: "このコード書いた誰だよ! そんな時の git log -S でもしてみよう"
date: 2013-06-04T20:30:00+09:00
comments: true
categories: [git]
---

[広島Git勉強会](http://local.aguuu.com/events/15354) で [@pecosantoyobe](https://twitter.com/pecosantoyobe) が [git log の使えるオプションについて語る](https://github.com/furu/hiroshimadotgit)というナイスなセッションがありました。

その中で `git log -S` の紹介がありましたが、説明が難しそうなので、さらっと流れてしました。
折角なので実例を紹介します。

複数人でプログラムを書いてると、**「このコード書いたの誰だよwww」** 的なことが稀にあります。


例えばこんなコードがあるとします。

```ruby
class Sample
  def hoge
    hogehoge_gorogoro.to_sym.to_s
  end

  def hogehoge_gorogoro
    "hogehoge_gorogoro"
  end
end
```

**「`hogehoge_gorogoro.to_sym.to_s` ってなんだよ!! 意味あるのかよ!」** みたいなことがあると思います。


そんな時はすかさず git blame を利用します。

みやすさの都合上、emacs の magit-blame を利用します。

![magitt](/images/git-log-S-magit.png)

e92db224 で変更されていることがわかります。

ちょっとこの時のコミットを見てみましょう。

`git show e92db224`

![git show e92db224](/images/git-log-S.png)

**インデントの修正されているだけ**で、大した情報が得られません。
こんなときに `git log -S` を使います。


`git log -S 'hogehoge_gorogoro' --patch`

変更内容が見たいので、 `--patch` をつけました。

![git log -S](/images/git-log-S-log-S.png)

**「なんだよ。initial commit ではじめからそーなのかよ!!」** なんて展開でした。
ちょっと例が凝れてなくて便利さが伝わりにくいかもしれません。

diff の内容から更に git log -S で追ってみたりできます。
もし良いコミットログがあれば、コードの意図がわかったり チケットID などが記載されていれば、そちらを参照することになります。

**「なんだよ。書いたのオレじゃねーか!! orz」** なんてこともよくあります。が、気を落とさず綺麗なコードを書いていきたいですね。

お試しあれ。

### 関連

* [magit-brame-modeの表示がみやすかった件](/blog/2012/05/30/magit-blame-mode/)
