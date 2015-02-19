---

title: "設定画面で表示する値をアプリ側で設定する - iOS Second Stage Advent Calendar 2013"
date: 2013-12-19T00:00:00+09:00
comments: true
categories: ["ios"]
---

この記事は[iOS Second Stage Advent Calendar 2013](http://qiita.com/advent-calendar/2013/ios-2)の19日目の記事です。
Advent Calendar に参加したのはギャングである[aguuu](http://qiita.com/aguuu) の中の人に脅されたわけではありません。

iOS のアプリケーションを iOS自体の設定画面で設定を行えるアプリケーション
があります。その画面内に稀にバージョン番号が表示されているようなものがあると思います。
これを行うには方法として紹介されているのはビルドする際に値を差し替える方法があります。
ググるとこればっかりでてきます。

* [参考: アプリのバージョン、ビルドを「設定」の初期値として設定する](http://d.hatena.ne.jp/MasatoKONDO/20120410/1334045339)

今回はそれをせずに、アプリ内で設定できたので紹介しておこうと思います。
動的設定とでも言えばいいのでしょうか。


この設定画面を用意する方法は Settings.bundle を追加し Root.plist を作成することで作ることができます。


今回のゴール

![](/images/ios-second-stage-advent-calendar/setting.png)

バージョンってタイトルには書いてますが、最終起動時刻のほうがわかりやすいので、最終起動時刻に焦点を当てます。
、設定画面に値を表示するには、Setting Bundle を追加します。

![](/images/ios-second-stage-advent-calendar/SettingsBundle.png)

root.plist に type を table のアイテムを追加し、DefaultValue を設定します。

**DefaultValue を設定します。**

大事なことなので強調しておきます。

![](/images/ios-second-stage-advent-calendar/root-plist.png)

あとは `[NSUserDefaults standardUserDefaults]` に値を書き込めばよいです。

```obj-c
NSUserDefaults* userDefaults = [NSUserDefaults standardUserDefaults];
NSDate* now = [[NSDate alloc] initWithTimeIntervalSinceNow:0];
[userDefaults setObject:[now descriptionWithLocale:[NSLocale currentLocale]]
                 forKey:@"lastLaunched"];
```

とっても簡単ですね。

[プロジェクトファイルはこちらにおいておきます](https://github.com/eiel/iOSAdventCal2013)

たまたま最初に思いつく方法を試したらうまくいきました。(最初は
Documents や Cache ディレクトリの中身を確認して plist を直接書き換えたなんて言えない…。しかも、何か勘違いをして UserDefaults を経由せずに書き換えたとか言えない…。)


ざっくりと公式のドキュメントをみましたが、記述をみつけられなかったので、裏技的なテクニックなのかもしれません。

明日はみんなのハーレムである [makowis](http://qiita.com/mako_wis) さんです。楽しみですね。
