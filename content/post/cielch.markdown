---

title: "エディタの文字色に悩む日々 - CIELCH"
date: 2013-03-13T11:38:00+09:00
comments: true
categories: ["エディタ","色"]
---

色について少し話しをしたいと思う。
プログラマたるものエディタの配色はこだわりたいものです。

目が疲れにくく、かつ、文字が読みやすい配色が欲しいのです。

特に私は視力がそんなによくないので、この点を気にしています。
既存のテーマのもので文字が読めなかったりして、色を変えたりしても上手いこと調和が取れない。
上手いこといかないので過去に何度か模索して、いまも模索しています。

そんなわけで今回は [CIELAB](http://ja.wikipedia.org/wiki/L*a*b*%E8%A1%A8%E8%89%B2%E7%B3%BB) について調べていたのですが、その変種な CIELCH について紹介したいと思います。

先にまとめておくと CIELCH を利用して配色を行います。
背景色と文字色で `L(明度)` がある程度の差をもつようにすると文字の読みやすさを確保することができます。
明度の差が大きいとチカチカするので差をつけすぎないのも大事です。
あとは C(彩度) や H(色相) のパラメータを調整しています。

## CIELAB

CIELAB は L a b というパラメータで色を指定します。CIEというのは 国際照明委員会だそうで、Lab とかいても通じそうです。
L は明度を示し 0だと黒で 100だと黒です。
a は正の値が大きいと マゼンダ(赤紫)っぽい色になり、負の値が大きいと緑っぽい色になります。
b は正の値が大きいと 黄色っぽい色になり、負の値が大きいと青っぽい色になります。

a と b がなんでそんな色なのかっていうと、色相を考えたとき、それぞれの正負が補色関係にあり、かつ a と b 直交するからでしょう。

この色空間の良いところは L です。
Lの明度は [HSV](http://ja.wikipedia.org/wiki/HSV%E8%89%B2%E7%A9%BA%E9%96%93)  の明度と違う明度で、人間の視覚に比較的近いのです。

HSV は 色相、彩度、明度を色で決める方法で、よくみかけると思います。この明度には人間の視覚においては非常にあてになりません。
例を出します。

黒が明るさ 0 なんだから 明るさ 100% の色を使えばうまくいくような勘違いを一度はしたことはないでしょうか。

rgb (0, 0, 255) の青を考えます。HSVでいうと 色相 240° 明度 100% 彩度 100% になります。 **明度 100%** です。

<div style="background-color: black; color: #0000FF">
明度 100% です。
</div>

大事なことなので3回言いました。3回目は読みにくいですね。
HSV の明度は人間の視覚に対応していません。

同じ明度の黄色でやってみましょう。

<div style="background-color: black; color: #FFFF00">
明度 100% です。
</div>

青のときに比べて文字がはっきりしますね。

というわけで、ほかの色空間が欲しくなります。

他に使えるものとしては[マンセルカラー](http://ja.wikipedia.org/wiki/%E3%83%9E%E3%83%B3%E3%82%BB%E3%83%AB%E3%83%BB%E3%82%AB%E3%83%A9%E3%83%BC%E3%83%BB%E3%82%B7%E3%82%B9%E3%83%86%E3%83%A0)とかありますが、RGB値が厳密には定義されてないようです。
色見本から測定して変換しているものはあるようです。

実世界の色とモニターの色を比較する場合、モニター自体の明るさを変更できてしまうし、一致させることができない。と、認識しました。

Haskell で CIELAB から RGB に変換する場合 ホワイトバランス というパラメータが必要でしたし。

いろんな文献をあたると RGB をマンセルカラーに変換する場合は XYZ という色空間を経由して、CIELAB にもっていくことがわかりました。
そこで CIELAB を利用するのですが、HSV のように 色相 明度 彩度で指定できると都合がよいので aとb を極座標にして 色相、彩度にしたのが CIELCH になります。

ようやく本題ですが、CIELCH の色立体が見たかったのです。
Y 方向に明度 X 方向に彩度 で 色相を18°づつずらしてみました。
また、RGB値に変換するときに どんな値でも変換できるのですが、負の値になるものやら,255を越えてしまうものやらでてしまいます。
若干丸めて、そのような色の場合は表示しないようにしました。

![異常っぽい値を省いた色見本](/images/cielch0.png)

[マンセルカラー](http://www.dic-color.com/knowledge/080926.html)と比べてみても似たような雰囲気はでてますね。
明い色になると青色がでてないのがわかります。


今度は彩度200 まで。
異常と判断した値も表示してみます。

![異常っぽい値を省かず色見本 彩度200](/images/cielch1.png)

彩度をあげると徐々に明るくなっちゃってますね。


もっと起きてることをわかりやすくするには彩度をもっと引っ張ります。
彩度500 とかいってみましょう。

![異常っぽい値を省かず色見本 彩度500](/images/cielch2.png)

ほんとは三次元に出したかった。

## 使用したコード

作成したコードも貼っておきます。Haskell のサンプルコードとしてお使いください。

```haskell
import Data.Colour.CIE
import Data.Colour.SRGB
import Data.Colour.CIE.Illuminant
import Data.Colour.SRGB.Linear

cieLCH :: (Ord a, Floating a) => Chromaticity a -> a -> a -> a -> Colour a
cieLCH white_ch l c h = cieLAB white_ch l a b
  where
    radius = pi * h / 180
    a = cos radius * c
    b = sin radius * c

validRGB :: (Fractional a, Ord a) => RGB a -> Bool
validRGB (RGB r g b) = if sum >= 0 && sum <= 3.1 &&
                          r > low && r < high &&
                          g > low && g < high &&
                          b > low && b < high
                       then True
                       else True
  where
    sum = r + g + b
    low = -0.1
    high = 1.2

toRGBColor :: (Fractional a, Ord a) => Colour a -> Maybe (Colour a)
toRGBColor colour = if validRGB rgb
                    then Just colour
                    else Nothing
  where
    rgb = Data.Colour.SRGB.Linear.toRGB colour

type ColorName = (Maybe (Colour Double), Double, Double)
type ColorTable = [[ColorName]]

colorTabletoHTML :: ColorTable -> String
colorTabletoHTML table = "<table style=\"display: inline;\">"
                         ++ (unlines $ map makeRow table) ++ "</table>"
  where
    makeRow :: [ColorName] -> String
    makeRow cols = "<tr>" ++ (unlines $ map makeCol cols) ++ "</tr>"
    makeCol :: ColorName -> String
    makeCol (color, l, c) = "<td style=\"background-color: " ++
                          rgbString color ++ "; \">&nbsp;</td>"
    rgbString Nothing = ""
    rgbString (Just color) = sRGB24show color

colorTable lightnesses chromas h =
  [[(makeColor l c h, l,c) | c <- chromas]
    | l <- lightnesses]
  where
    makeColor l c h = Main.toRGBColor $ cieLCH d65 l c h

hues = [0,18..342]
chromas = [200,160..0]
lightnesses = [100,80..0]

main = do
  putStrLn "<body style=\"background-color: #2c2c2c\">"
  mapM_ putStrLn $ map (colorTabletoHTML . colorTable lightnesses chromas) hues
  putStrLn "</body>"
```
[gist](https://gist.github.com/eiel/5148886)

実際、私達が普段目にするものは彩度が高いものはなくて、RGBの彩度100 の値は不自然です。
かといって彩度を下げすぎると味けなくなるのでバランスが大事です。
明度を統一しつつ彩度をどこまであげるか、という線型計画問題的な感じになります。これに色の調和の計算式を盛り込めば、機械的に配色を計算できるようになりそうですね。

## 実際に配色してみた

そんなわけでこれらを参考にして配色してみました。
明度 70 彩度 60 あたりで 色相は均等にはしていません。
彩度がでにくい色は少しだけ高い彩度が表現できる方向にずらしたりしています。

![配色例](/images/color.png)

抜粋したら以下の感じでした。
```
(gray "#c6c6c6")
(red "#ff91a7")
(orange "#ffa667")
(yellow "#f2e582")
(green "#87d46f")
(aqua "#6cd0dc")
(blue "#80c1ff")
(purple "#e9a7ff")
```

背景色は `#2c2c2c` を使用しています。こうしておくと黒が使えるし、黒は色として尖りがあるので、落ちついた雰囲気になります。

Haskell のコードについては別途解説したいと思います。

## 参考文献

* [Wikipedia: Lab色空間](http://ja.wikipedia.org/wiki/L*a*b*%E8%A1%A8%E8%89%B2%E7%B3%BB)
* [PDF カラーコミュニケーションガイド —色を正しく伝えるために](http://www.xrite.com/documents/literature/ja/L10-001_Understand_Color_ja.pdf)
* [色彩理論：表色系：オストワルト表色系](http://www.nanisama.com/color/system/Ostwalt/index.html)
* [HaskellDB colour](http://hackage.haskell.org/package/colour)
* [分子細胞生物学研究所・脳神経回路研究分野](http://jfly.iam.u-tokyo.ac.jp/lab/colorresearch.html)
