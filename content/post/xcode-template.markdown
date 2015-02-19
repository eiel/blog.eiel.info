---

title: "Xcodeのテンプレート"
date: 2012-06-18T18:03:00+09:00
comments: true
tags: [xcode,ios]
---
Xcodeが genarate する templateですが、
いつもかきかえる部分があるのでなんとかしたいなーっておもっててしらべたら

http://akisute.com/2009/06/xcode.html

にかかれているんですが、
```
/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/
```
というディレクトリはすでにありません。

現在は
```
/Applications/Xcode.app/Contents/Developer/Platforms/iPhoneOS.platform/Developer/Library/Xcode/
```
あたりにあるようです。
