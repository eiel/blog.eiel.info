---

title: "手軽にHaskell できる hawk が楽しい"
date: 2014-02-14T21:34:00+09:00
comments: true
tags: ["haskell"]
---

コマンドラインで Haskell のワンライナーっぽいものが使える [hawk](https://github.com/gelisam/hawk) ってのがあるらしくて、awk に似ているから hawk っていうらしい。

気軽に Haskell の練習できて楽しい。

インストールは

```
$ cabal install haskell-awk
```

らしい。

```
hoge:goro:mogu
goro:mogu:hoge
```

という入力があって 2列目だけ取り出したーいとかなら

```
$ echo "hoge:goro:mogu\ngoro:mogu:hoge" | hawk -d: -m '!!2'
mogu
hoge
```

という感じらしい。-m を使うと行ごとの処理がかけて -d を使うとデリミタを認識してあらかじめリストしておいてくれる。

それ意外にも情報源にできる。

奇数のリストを作って10個とりだしてみる。

```
$  hawk 'take 10 [1,3..]'
1
3
5
7
9
11
13
15
17
19
```

もちろん無限リストだって作れる。


```
$ hawk '[1,3..]' | head -n 10
1
3
5
7
9
11
13
15
17
19
```

`~/.hawk/prelude.hs` をいじれば、他のモジュールもインポートできる。

Data.Ix でも追加して遊んでみる。

```haskell
{-# LANGUAGE ExtendedDefaultRules, OverloadedStrings #-}
import Prelude
import qualified Data.ByteString.Lazy.Char8 as B
import qualified Data.List as L
import Data.Ix
```

```
$ hawk 'range ((0,0,0),(2,2,2))' | head -n 20
0 0 0
0 0 1
0 0 2
0 1 0
0 1 1
0 1 2
0 2 0
0 2 1
0 2 2
1 0 0
1 0 1
1 0 2
1 1 0
1 1 1
1 1 2
1 2 0
1 2 1
1 2 2
2 0 0
2 0 1
```

`ひっくくりかえして`、 `3つとる`とかを愚直にかくと、

```
$ seq 10 | hawk -a 'take 3 . reverse'
10
9
8
```

でやりたい順番と逆転してしまうので、 `~/.hawk/prelude.hs` に `import Control.Arrow` 追加しときました。

```
$ seq 10 | hawk -a 'reverse >>> take 3'
10
9
8
```

なんとなく楽しい。
