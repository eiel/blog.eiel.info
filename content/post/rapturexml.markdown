---

title: "raptureXML でXMLのparse"
date: 2012-12-06T14:33:00+09:00
comments: true
tags: ["iOS","XML","Objective-c"]
---

XMLのパースしなきゃいけなくて、libxmlで処理するのめんどくさいなー。ということで、[CocoaPod](http://ios.eiel.info/CocoaPods)を探った結果、[RaptureXML](https://github.com/ZaBlanc/RaptureXML)を試してみることにしました。

URLから直接XMLを取得するイニシャライザがついていて、とても簡単に利用することができました。

CocoaPodsを使わない場合は libz と libxml2 をリンクしてやるようにして、RXMLElement.hとRXMLElement.mをプロジェクトに追加するだけで使えます。

だいたい以下のように利用してます。

```objc
NSURL* url = [NSURL URLWithString:@"http://eiel.info/hoge.xml"];
RXMLElement* root = [RXMLElement elementFromURL:url];
NSMutableArray* schedule = [NSMutableArray array];
[root iterate:@"item" usingBlock: ^(RXMLElement *item) {
    [schedule addObject:[[ALScheduleItem alloc] initWithRXMLElement:item]];
}];
i_schedule = [NSArray arrayWithArray:schedule];
```
RXMLElementオブジェクトを配列に格納しておいて利用しようとしたら、失敗したのでモデルオブジェクトを用意してやりました。

値を取り出すには

```objc
[element child:@"day"].text;
```
とやって取り出せます。DOMのインターフェイスになってますね。XPathも利用できるようです。
ソースコードも500行程度でコンパクトでした。
