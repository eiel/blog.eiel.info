---

title: "UIImage#initWithCGImage:scale:orientation で回転させる話"
date: 2014-01-22T02:11:00+09:00
comments: true
categories: ["ios"]
---

iOS な作業をしていた。

UImage な画像をてっとり早く回転する方法として、

```objc
image = [[UIImage alloc] initWithCGImage:image.CGImage scale:image.scale orientation:UIImageOrientationRight];
```

というのがある。

みたいなのがあるのですが、2度回転させようとして、下記のように2度呼んで回転しないなぁ、というハマり方をした。

```objc
image = [[UIImage alloc] initWithCGImage:image.CGImage scale:image.scale orientation:UIImageOrientationRight];
image = [[UIImage alloc] initWithCGImage:image.CGImage scale:image.scale orientation:UIImageOrientationRight];
```

実際に並んでいれば気づくのだけど、違う場所で呼んでる場合は注意。

CIImage の使う向きを変えてCGImageは使いまわしてるだけなので、右に回転させたものを使うだけになる。「右に方向にして使う」という機能なので当たり前なのだけど。

どう対処したかというと、1回目の回転はちゃんと新しい画像をつくって対処した。

```objc
CGContextRef context = UIGraphicsGetCurrentContext();
CGContextRotateCTM(context,M_PI);
CGContextTranslateCTM(context, -image.size.width, -image.size.height);
[image.layer renderInContext:context];
image = UIGraphicsGetImageFromCurrentImageContext();
```

ちょっと無理矢理感。
移動させてるのは、(0,0) を中心に回転するので、画像が実際にかきこまれる位置が負の領域にいってしまってるから。
