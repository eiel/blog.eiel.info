---

title: "「状態管理用の変数をインスタンスに持たせるなこのタコって話」という記事のStateパターンを適用したときのリファクタリングのステップが気になって。"
date: 2013-02-11T01:52:00+09:00
comments: true
tags: ["リファクタリング","Stateパターン"]
---

内容の批判とかではなくて、自分が気になったことで、記事にするのもどうかなって思ってたのですが、サンプルコードを書いちゃったので、折角なので書きおきします。

[状態管理用の変数をインスタンスに持たせるなこのタコって話 - life.should be_happy # => 1 examples, ? failures](http://nekogata.hatenablog.com/entry/2013/02/09/233540) という記事がホットエントリに出ていて、すごく面白いです。

その中で 「Stateパターンを使おう」というところでリファクタリングのステップが大きいなーって感じて、30分ぐらい考えてしまったんです。
Stateパターンってのをよく知っていれば、書き換えることは難しくないのですが、丁寧にコミットすることを考えたとき、どういうリファクタリングのステップにすればいいのかな、と。

リファクタリングは極力小さなステップで行うほうが良いです。いきなりがっつりやるためにはテストコードがほぼ必須になります。
あったとしても小さいほうがうごかなくなって悩む時間が減ります。


ということで、**「リファクタリング飛躍」** に該当する部分は、[player_02.rb](https://gist.github.com/Shinpeim/4745444) から [player_03.rb](https://gist.github.com/Shinpeim/4745446) への変化する部分になります。

リファクタリングのステップが大きすぎることを**「リファクタリング飛躍」**と個人的に呼びたいです。


## 例 1 クラスごとに分かれる処理を case でまとめておく

[https://gist.github.com/eiel/4748209](https://gist.github.com/eiel/4748209)

ポイントだけ、抜粋します。

元のコードは:
```ruby
  def move(direction)
    case @moving_mode
    when MOVING_MODE::NORMAL
      x_speed = 10 # かにモードとの整合性のために
      y_speed = 10 # 無駄な変更が加えられている
    when MOVING_MODE::FAST
      x_speed = 20 # FASTモードにも！
      y_speed = 20
    when MOVING_MODE::KANI
      x_speed = 40
      y_speed = 5
    end
 
    case direction
    when :up
      @position[:y] -= y_speed #ここも書き換えないといけない
    when :down
      @position[:y] += y_speed # ここも
    when :left
      @position[:x] -= x_speed # こk
    when :right
      @position[:x] += x_speed # k
    end
  end
```

```ruby
  def move(direction)
    case @moving_mode
    when MOVING_MODE::NORMAL
      case direction
      when :up
        @position[:y] -= 10
      when :down
        @position[:y] += 10
      when :left
        @position[:x] -= 10
      when :right
        @position[:x] += 10
      end
    when MOVING_MODE::FAST
      case direction
      when :up
        @position[:y] -= 20
      when :down
        @position[:y] += 20
      when :left
        @position[:x] -= 20
      when :right
        @position[:x] += 20
      end
    when MOVING_MODE::KANI
      case direction
      when :up
        @position[:y] -= 40
      when :down
        @position[:y] += 40
      when :left
        @position[:x] -= 5
      when :right
        @position[:x] += 5
      end
    end
  end
```

コード量は増えます。

Player03.rb へとリファクタリングするには、それぞれのクラスと同じコードになってないとクラスへの分離は難しいです。
ポリモーフィズムってのは 自動でwhenが増える case と見なせる(と個人的に思っている)ので、ここからクラスへ分けるのは常套句になります。

外側のwhenブロックごとにクラスへと分離していくことになります。


case がネストしていて、少し異臭がするので、はじめからこういうコードを書けるのは State パターンを見据えてるか、かなりの熟練プログラマーな気がします。
実際には移動量はデータにできるので、もっとシンプルにかけるよーな気がしますが、本題からずれるので考えません。


## 例2 いきなりクラスに分離してみる。

[https://gist.github.com/Shinpeim/4745444](https://gist.github.com/Shinpeim/4745444)

実際にはひとつづやれば良いと思います。

ポイントだけ抜粋。

元コードも再び:
```ruby
  def move(direction)
    case @moving_mode
    when MOVING_MODE::NORMAL
      x_speed = 10 # かにモードとの整合性のために
      y_speed = 10 # 無駄な変更が加えられている
    when MOVING_MODE::FAST
      x_speed = 20 # FASTモードにも！
      y_speed = 20
    when MOVING_MODE::KANI
      x_speed = 40
      y_speed = 5
    end
 
    case direction
    when :up
      @position[:y] -= y_speed #ここも書き換えないといけない
    when :down
      @position[:y] += y_speed # ここも
    when :left
      @position[:x] -= x_speed # こk
    when :right
      @position[:x] += x_speed # k
    end
  end
```


```ruby
    def move(direction, position)
      x_speed = 10
      y_speed = 10
 
      case direction
      when :up
        position[:y] -= y_speed
      when :down
        position[:y] += y_speed
      when :left
        position[:x] -= x_speed
      when :right
        position[:x] += x_speed
      end
      return position
    end
```

MOVING_MODE::NORMAL のときの処理だけ抜粋してクラスにわけます。 `x_speed` と `y_speed`を展開すれば、Ruby03.rbのコードへと向います。

これはこれでこのまま放置しても良い気がします。
他と並べたときに整理したくなるかもしねません。

これも、Stateパターンが見えてないと難しいかもしれません。

## まとめ。

これらは単純に僕がコードを並べて検討したかっただけです。
どちらの例もゴールが見えてないと難しいと実感したのが結論です。

あと、リファクタリングにはテストコード大事だと感じました。

そんなわけで先人の知恵たるデザインパターンを学ぶにはとても価値あることです。
