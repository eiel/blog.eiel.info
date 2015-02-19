---

title: "C++勉強会in広島でオープンセミナー2014@広島の告知を兼ねたLTしてきた"
date: 2014-01-11T23:54:00+09:00
comments: true
tags: ["C++"]
---

[C++勉強会in広島](http://partake.in/events/5ddde1fe-88b7-4541-9f37-02cf4fa0284c)に参加してLTしてきました。

<script async class="speakerdeck-embed" data-id="767eb2105c40013147cb72318cd7c772" data-ratio="1.33333333333333" src="//speakerdeck.com/assets/embed.js"></script>

テスト駆動開発が[オープンセミナー2014@広島](http://osh-2014.github.io/)のテーマのひとつなので、C++ のテスティングフレームワークである CppUTest を試しすことで告知するという手法をとりました。

C++は大学生のころ勉強していたようなそうでもないような、[More Effective C++ ](https://www.amazon.co.jp/dp/4621066064?tag=eiel-22&camp=1027&creative=7407&linkCode=as4&creativeASIN=4621066064&adid=1AD3439TTKC9VEHCJTFN&)ぐらいは読んだかなぁ。
そういえば丸善出版で再販されるそうですね。よかったよかった。

冷静に考えると C++ で書いたプログラムをビルドするのに Makefile を書いたのは、はじめてな気がしたり、clang++ をコマンドから使うのがはじめてだったり、LT をしようとすることでいろいろ勉強になるなぁ、と感じました。
勉強会があるので、そのために勉強するのも良い方法だと思います。

「まとめ」が「まとめ」じゃないって言われたので、今度から「いろいろ試した結果、最終的な結論」とかにしたいと思います。

[実際に動作確認するのに使用したソースコードは GitHub に置いています。](https://github.com/eiel/cpphiroshima-1)

* `src/test_runner.cpp` がテストを実行する部分です。
* `src/factorial_test.cpp` がテストコードです。
* `src/factorial.hpp` が階乗求めるプログラムの実装部分です。

make を実行するとビルドしてテストを実行するようにしています。

そういえば test というディレクトリを最初つくっていて、 make test した時に test ディレクトリがあるせいでうまく動かなかったりしました。
一般的にはどうするんだろうか。

### その他の発表

[南山まさかず氏](https://twitter.com/PG_nonen/) の Template Meta Programming なんかは全く知らない世界でした。
シンタックスを気にしなきゃ、純粋関数型プログラミングと見なせるようだったので、がんばれば使えそうな気がしてきております。
ちょっとぐらいサンプルコードを書いてみたいと思いますが、コンパイルエラーの解読がきっとつらいんだろうなぁ、と想像してます。

* [スライドはここにあるらしい](http://www.slideshare.net/masakazuminamiyama/cin-29888053)

[uchan_nos](https://twitter.com/uchan_nos)さんのC++でできる!OSの自作入門は、知ってる範囲のこともあったりそうでないところもあったりでおもしろかったです。
プログラム書いてたらOSの仕組みやらブートシーケンスはやっぱり知りたくなりますよね。

「フリースタンディング環境」という言葉は初めて知ったので、いろんな言語のそのあたりの状況もちょっと気になっております。

* [スライドはここにあるらしい](http://www.slideshare.net/uchan_nos/cppos)

その他、全体の雰囲気は[Togetterのまとめ](http://togetter.com/li/614849)をみるほうがわかりやすいかもしれない。
