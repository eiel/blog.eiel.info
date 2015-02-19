---

title: "Haskell のフィールドラベルをもつデータ型について"
date: 2014-09-06T09:26:00+09:00
comments: true
categories: ["haskell"]
---

Haskellのレコード構文というかフィールドラベルをもつデータ型についてなんだけど、苦手意識というか更新の方法を最近までよくしらなくてうまく使えてなかった。
わかったことを含めて書いておく。

フィールドラベルをもつデータ型はざっくりいえば、構造体のようなものである。名前と年齢をもつ「人」を表現する型をつくってみよう。

```haskell
data Person = Person { name :: String, age :: Int } deriving Show
```

上記はフィールラベルがないデータ型にいろいろおまけがついてくるだけなので

```haskell
data Perosn = Person String Int
```

とした場合と同じような使い方ができる。

```haskell
ghci> data Person = Person { name :: String, age :: Int } deriving Show
ghci> Person "eiel" 30
Person {name = "eiel", age = 30}
```

せっかくラベルがあるので、生かした使い方をしてみる。

```haskell
ghci> data Person = Person { name :: String, age :: Int } deriving Show
ghci> Person { name = "eiel", age = 30 }
Person {name = "eiel", age = 30}
```

冗長であるがわかりやすい。

ラベルをつけるとよいところは値を取り出すのが少し楽になる。
ラベルと同名の関数が生成される。

```haskell
ghci> data Person = Person { name :: String, age :: Int } deriving Show
ghci> let person = Person { name = "eiel", age = 30 }
ghci> name person
"eiel"
ghci> age person
30
```

特に意味はないが let を使わない場合。

```haskell
ghci> data Person = Person { name :: String, age :: Int } deriving Show
ghci> name Person { name = "eiel", age = 30 }
"eiel"
ghci> age Person { name = "eiel", age = 30 }
30
```

補足で、ラベルがない場合どうするか。

```haskell
ghci> data Person = Person { name :: String, age :: Int } deriving Show
ghci> let person = Person { name = "eiel", age = 30 }
ghci> case person of Person name _ = name
"eiel"
ghci> case person of Person _ age = age
30
```

値を更新する方法。
ここをちゃんと知らなくて使ってなかったけど、とても簡単。


```haskell
ghci> data Person = Person { name :: String, age :: Int } deriving Show
ghci> let person = Person { name = "eiel", age = 30 }
ghci> person { name = "eielh" }
Person {name = "eielh", age = 30}
ghci> person { name = "eielh", age = 20 }
Person {name = "eielh", age = 20}
```

`> 突然のハタチ <`

特に意味はないが lot を使わない場合。

```haskell
ghci> data Person = Person { name :: String, age :: Int } deriving Show
ghci> Person { name = "eiel", age = 30 } { name = "eielh" }
Person {name = "eielh", age = 30}
ghci> Person { name = "eiel", age = 30 } { name = "eielh", age = 20 }
Person {name = "eielh", age = 20}
```

ということはこういうこともできる。

```haskell
ghci> data Person = Person { name :: String, age :: Int } deriving Show
ghci> Person { name = "eiel", age = 30 } { name = "eielh" } { age = 20 }
Person {name = "eielh", age = 20}
ghci> Person { name = "eiel", age = 30 } { name = "eielh" } { name = "goro" }
Person {name = "goro", age = 30}
```

補足で、ラベルがない場合どうするか。

```haskell
ghci> data Person = Person { name :: String, age :: Int } deriving Show
ghci> let person = Person { name = "eiel", age = 30 }
ghci> case (case person of Person name age -> Person "eielh" age) of Person name age -> Person name 20
Person {name = "eielh", age = 30}
```

最適化できるけど、あえてしっかり書いてみる。これはつらい。
というか case 万能すぎ。

「構造体なんてただの組で、アクセスしやすいようにラベルがついてるだけなんだよ」と言われている気がした。

あれ?これ、十分[LT駆動開発](https://github.com/LTDD/Sessions/wiki)でネタにできた気がする…しまった。

### 参考文献

* [Haskell 2010 3 Expressions](http://www.haskell.org/onlinereport/haskell2010/haskellch3.html#x8-490003.15)
* [Haskell 98 Report: 式](http://www.sampou.org/haskell/report-revised-j/exps.html)

2番目のリンクは山下先生の日本語訳。2010はまだ訳されてない模様。
3.15節からあたり。
