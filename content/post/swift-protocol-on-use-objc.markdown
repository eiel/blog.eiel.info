---
date: 2016-01-1T10:00:00+09:00
title: "swiftで書いたProtocolをObj-Cで使うなら@objcをつけろ"
tags: ["swift","objective-c"]
---

Swiftで書いたProtocolをObjective-Cで使う予定があるなら、`@objc`をつけなければいけない。
つけなければ、Obj-C側で使えない。

```
@objc protocol Hoge {
  func get() -> Int
}
```

というHogeプロトコルがあったとき

```
class HogeService : NSObject {
  let hoge: Hoge
}
```

としたとき、HogeServiceのhogeをィールド利用しようとするとコンパイルエラーになる。
そもそもヘッダファイルに書き出されない。

単にちゃんとドキュメントを読んでいなくてハマっただけといえばそれだけである。

類似したもので、Swiftで書いたクラスはNSObjectを継承していないとObjective-Cで使えないなどある。

### 参考文献

* [SwiftとObjective-Cで相互に連携する - Qiita](http://qiita.com/edo_m18/items/861d090a5471f4f0eeae)
