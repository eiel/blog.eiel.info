---

title: "Haskell で Selenium"
date: 2014-08-25T16:20:00+09:00
comments: true
tags: ["haskell","selenium"]
---

たまには Haskell が書きたかったので、コマンドラインからあるウェブサービスに書き込みできるようにしたが失敗した。

失敗したというか画面を進めていくと止まってしまう。
なにやらアラートがでて処理ができていない感じなのだろうか。
Rubyでやっても停止するので、Haskellの問題ではない。

一応、最低限の使い方がわかったのでメモしとく。

利用したのは、[hs-webdriver](http://hackage.haskell.org/package/webdriver) と [phantomJS](http://phantomjs.org/)。

phantomJS は --webdriver オプションを使用することで、SeleniumのServerとして使えるようになる。
Haskellでは Selenium と対話するための webdriverというライブラリがあって制御することが可能。

Google にアクセスしてスクリーンショットを作成するプログラムをかいてみた。

```
{-# LANGUAGE OverloadedStrings #-}
import Test.WebDriver
import Control.Monad.IO.Class
import qualified Data.ByteString.Lazy.Char8 as B

main :: IO ()
main =
  runSession defaultConfig $ do
    openPage "http://google.co.jp/"
    screenshotWriteFile "google.png"

screenshotWriteFile::  FilePath -> WD ()
screenshotWriteFile name = do
  string <- screenshot
  liftIO . B.writeFile name  $ string
```

事前に

```
$ phantomjs --webdriver=4444
```

としてから実行する。

実行すると google.png というファイルが生成されている。

screenshot は ByteString を返すので保存しやすいように screenshotWriteFile という関数を定義して保存しやすくした。
これは webdriver が base64 形式で情報を返してくるのでエンコードした情報を返す模様。

ブラウザを操作するための関数は[Test.WebDriver.Commands](https://hackage.haskell.org/package/webdriver-0.6.0.1/docs/Test-WebDriver-Commands.html)を見ればかいてある。
