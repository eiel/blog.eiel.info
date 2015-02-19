---

title: "iOS でmtimeを設定する"
date: 2014-02-10T19:05:00+09:00
comments: true
tags: ["ios"]
---

iOS で ファイルの mtime を設定したい事態が発生した。
utimes(2) を利用してもよいのだけど、なるべく Cocoa の領域でコードは書いておきたい。

書き込む前に取得。取得したいファイルパスはわかっているとします。

```objc
NSString* path = @"hoge.txt";
NSFileManager* filemngr = [NSFileManager defaultManager];

NSDictionary* attributes = [filemngr attributesOfItemAtPath:path error:nil];

if (attributes) {
    NSDate *date = [attributes fileModificationDate]
}
```

`NSFileManager-attributesOfItemAtPath:error` で 辞書型でファイルの情報を取得できます。
`NSDictionary-fileModificationDate` は NSFileManager.h で拡張されたメソッドです。これを使えば取得できます。

書き込みする際は拡張メソッドはないですが、取得した辞書型に値を設定してに書き込みすればできました。

```
NSString* path = @"hoge.txt";
NSFileManager* filemngr = [NSFileManager defaultManager];

NSDictionary* attributes = [filemngr attributesOfItemAtPath:path error:nil];

if (attributes) {
    NSMutableDictionary* mattributes = [NSMutableDictionary dictionaryWithDictionary:attributes];
    NSDate *date = [NSDate new];
    [mattributes setObject:mtime forKey:NSFileModificationDate];
    [fileManager setAttributes:mattributes ofItemAtPath:path error:nil];
}
```

`NSFileManager-setAttributes:mattributes ofItemAtPath:error:` に作成した attributes を使うだけでした。

### 参考文献

* [ios - Get document directory files date modified time in iphone - Stack Overflow](http://stackoverflow.com/questions/13854993/get-document-directory-files-date-modified-time-in-iphone)
