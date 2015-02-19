---

title: "流れるようにプログラミングしたい - 合同勉強会 in 大都会岡山 2013 Winter"
date: 2013-12-15T01:38:00+09:00
comments: true
categories: ["勉強会","ruby","haskell"]
---

[合同勉強会 in 大都会岡山 -2013 Winter-](http://gbdaitokai.doorkeeper.jp/events/5725) でライトニングトークをしました。

合同勉強会という名前からわかるように各勉強会からいろんなスピーカーがやってきて、セッションをします。
私は[Hiroshima.rb](http://hiroshimarb.github.io/)・[すごい広島](http://great-h.github.io/)・[WEB TOUCH MEETING](http://webtouchmeeting.com/) からやってきたという形でライトニングトークをしました。

<script async class="speakerdeck-embed" data-id="82c04820470b0131b3441e6594d9299f" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

「流れるようにプログラミングしたい」というタイトルです。
Ruby で読み書きしやすいプログラミングをしたときの問題点を紹介しつつ、Haskell の良いところを紹介するといった内容です。
内容も多かったのでかなりの早口で喋りました。

* [コードはこちらら](https://gist.github.com/eiel/7956834)

Ruby で関数型の思考でプログラミングをすると自然と流れるようなコードになりますが、問題があり、データをすべて読み終えないとプログラムが実行されないという状態になります。

その例が flow.rb のプログラムです。

```ruby
puts ARGF.each_line
  .map(&:to_i)            # 数値に
  .map { |n| n * 5}       # 5倍する
  .select { |n| n > 10 }  # 10より大きいものだけに
  .first(5)               # 最初の5つ
```

Haskell を使用した場合は、そのまま書いてもこの問題はおきず、現時点で処理できる時点まで処理してくれています。

```haskell
main = do
  getContents >>= (
    return .
    take 5 .        -- 最初の5個
    filter (>10) .  -- 10より大きいものだけに
    map (*5) .      -- 5倍する
    map read .      -- 数値に
    lines
    ) >>= mapM_ print
```

しかし、Haskell の場合はコードの順番と実行の順番が逆転してしまいます。

Haskell をつかわなくても ruby 2.0 以降標準添付された Enumerator::Lazy を使うとこの問題は解決できます。

```ruby
ARGF.each_line
  .lazy
  .map(&:to_i)             # 数値に
  .map { |n| n * 5 }       # 5倍する
  .select { |n| n > 10 }   # 10より大きいものだけに
  .map { |n| puts n }      # 出力
  .first(5)                # 最初の5個
```

この lazy はなかなか一筋縄にはいかなくて、出力も map でやらなければならなく、純粋さを追求すると気持ち悪いです。

Haskell の問題である順番が逆転してしまう問題は Control.Arrow を利用すると解決できます。

```haskell
main = getContents >>= (
  lines
  >>> map read       -- 数値に変換
  >>> map (*5)       -- 5倍する
  >>> filter (>10)   -- 10より大きいものだけに
  >>> take 5         -- 最初の5個
  >>> return
  ) >>= mapM_ print
```

これでちゃんと、順番どおりによむことができます。

### まとめ

Arrow の部分はおまけにすぎないですが、感動した部分であったり、関数型の思考で Ruby を書いた場合は効率という点で気になる部分もあり Haskell 面白いと思った部分です。

その部分を無理矢理5分で話したのでかなり早口だったり、私自身の息が上がったりして少し迷惑をおかけしました。

笑いもとれていたようなので、その点はよかったです。

ByteString や Text をつかった場合、どうなるのか確認しておきたいなぁ。
