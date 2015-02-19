---

title: "@souda1025 に PythonでFizzBuzzとかしてみた に対抗しろって煽られたので。"
date: 2013-01-26T14:33:00+09:00
comments: true
tags: ["python","ruby","haskell","FizzBuzz"]
---

[@soudai1025](http://twitter.com/soudai1025) が書いた[ブログ記事にPythonでFizzBuzzとかしてみた](http://soudai1025.blogspot.jp/2013/01/pythonfizzbuzz.html?spref=fb)というエントリーがあるのですが、Facebookでこういうコメントをみた。

> 多分、ひむひむが対抗してくるはず。

全力でお答えしましょう。

とりあえず、普通 FizzBuzz かくならこうかくだろう。

```python
def fizzbuzz(number):
    if number % 15 == 0:  # number % 5 == 0 and number % 3 == 0
        return "FizzBuzz"
    elif number % 5 == 0:
        return "Buzz"
    elif number % 3 == 0:
        return "Fizz"
    else:
        return str(number)

if __name__ == '__main__':
    number = int(raw_input("Please enter an integer: "))
    print fizzbuzz(number)
```

数値を入れると `数値の文字列` か "Fizz" か "Buzz" か "FizzBuzz" を返す関数を用意するほうが柔軟性があり、わかりやすいです。

さて、もとのコードを確認していきましょう。

```python
int = int(raw_input("Please enter an integer: "))

def do_fizz(int):
    if (int % 3) == 0:
        return 1
    return 0

def do_buzz(int):
    if (int % 5) == 0:
        return 2
    return 0

def do_answer(fizz, buzz):
    flag = fizz + buzz
    if flag == 0:
        print int #引数に居なくても外のintを参照出来る
    elif flag == 1:
        print "Fizz"
    elif flag == 2:
        print "Buzz"
    elif flag == 3:
        print "FizzBuzz"

do_answer(do_fizz(int), do_buzz(int))
```

さて、気になる点をあげていこう。

* `do_answer` 関数が外のスコープにアクセスしている。
* よくわからないフラグ処理がされている。
* `do_answer` の引数が意味不明。

## 関数が外のスコープにアクセスしている

関数が外のスコープにアクセスしてしまうとその関数だけみたときに他の部分を確認しないといけないのでよくない。

それぐらいなら引数を追加しましょう。

## よくわからないフラグ処理がされている

`do_fizz` と `do_buzz` が関数名から何をするのかさっぱりわからない。

* `do_fizz` は *3で割り切れる場合 1 を返し、それ以外の場合は 0 を返す関数である*
* `do_buzz` は *5で割り切れる場合 2 を返し、それ以外の場合は 0 を返す関数である*

ということはコードをよまなければわからない。ならば、関数の頭にコメントをかくか、そのような名前の関数にすべきだと思う。

`do_` という接頭辞が着いている以上何かする関数だと想像するので、ここで `print` されているのであれば、まだ良いと思うけど, iPhoneで閲覧していたらこの命名のせいで混乱しました。

## do_answer の引数が意味不明

fizz って何? buzz って何?

`do_answer(do_fizz(int), do_buzz(int))` これをみてわけがわかる人がいたら教えて欲しい。

`do_fizz` と `do_buzz` の実行結果を使うのであれば、関数内で使うべきだろう。`int`をdo_answer に渡さない設計にしているのに `do_fizz` と `do_buzz` に渡しているのに ここで int の文字がふたつ見える。 **わけがわからないよ**

`do_answer` は `print` するという点でまあ良いのじゃないかと思う。
ただ、doしないversionを用意しておけば

```python
for n in range(1,int):
   print answer(do_fizz(n), do_buzz(n))
```

とかきかえることができて、`int`までの FizzBuzz が表示できてナイスだと思います。

## まとめ

「Haskell と Ruby で書いたらどうなるかを書け」という煽りな気がしたけど無視してみた。

ついで、個人的感想。

> 「Pythonって三項演算子どうやるんだろ？」って思ったんで調べて使ってみた。

たぶん、`and` `or` で同様のことはできるけど、**「3項演算子は読みにくいから使うな。」** ってことだと思う。

> phpの

```
$flag[] = (int % 3 == 0) ? 1 : 0;
$flag[] = (int % 5 == 0) ? 2 : 0;

$flag['fizz'] = (int % 3 == 0) ? 1 :0;
$flag['buzz'] = (int % 5 == 0) ? 2 :0;
```

> みたいにいきなりList（配列）を作る書き方がPythonでも出来ると思うんだけど知識不足。
> 公式チュートリアルやったらどっかで出てくるかな？ｗ

初期化してない変数に無理矢理突っ込むということのほうがおかしい。
かくなら
```python
flag = []
flag.append(fizz)
flag.append(buzz)
```
となるのではないでしょうか。

このように中途半端なコードを書いて誰かを煽ると添削とかしてもらえるらしいです。非常に勉強する際にショートカットになりますし、煽られるほうも勉強になります。どんどん真似していきましょう。


まあ、せっかくなので Haskell でも書いておきました。

```
import Control.Monad

main = do
  putStr "please enter an integer:"
  number <- fmap read $ getLine
  putStrLn . fizzbuzz $ number
  -- forM_ [1..number] $ \n ->
  --       putStrLn . fizzbuzz $ n

fizzbuzz :: Int -> String
fizzbuzz n | n `mod` 15 == 0 = "FizzBuzz"
           | n `mod`  5 == 0 = "Buzz"
           | n `mod`  3 == 0 = "Fizz"
           | otherwise       = show n
```


再帰についても書きたいですが、話がずれてしまうので、また別の機会に。

[リポジトリはこちらに用意しておきました。](https://github.com/eiel/soudai-FizzBuzz)

トマホークおまちしています。
