---
date: 2016-01-08T09:00:00+09:00
title: アプリケーションの起動ショートカットにBetter Touch ToolからKarabinerで行うようにした
tags: ["Karabiner"]
---

Better Touch Toolが有料化の運びになったため、会社PCでは申請しないと使えないだろうし、私にとってMUSTのアプリケーションではない。
しかし、別のツールでもできる機能なのだけど、私にとってMUSTな機能として、ショートカットキーでアプリケーションを起動する機能がある。
これまではQuickSilverやAlfledのpower packなどでやってきたことだ。

* 参考 [Macアプリをショートカットキーに登録して、一発起動させるアプリ「BetterTouchTool」 | iDEA CLOUD/dev](https://ideacloud.co.jp/dev/bettertouchtools_2.html)

今回、これを気に別の方法を模索した結果[Karabiner](https://pqrs.org/osx/karabiner/index.html.ja)で同様のことを行うことにした。

### メリット

* 右Cmdと左Cmdを区別したショートカット設定ができる
* テキストベースでの設定

左CmdはOSXのショートカットキーは右Cmdは独自ショートカットとかにできて便利。
テキストベースの設定なのでGit管理ができてよい。

### デメリット

* 設定するのがめんどくさい

xmlを書くことになるのでやっぱり設定は格段にめんどくさい。

### 設定例

Cmd+eでemacsを起動する例

```xml
<?xml version="1.0"?>
<root>
  <vkopenurldef>
    <name>KeyCode::VK_OPEN_URL_APP_Emacs</name>
    <url type="file">/Applications/Emacs.app</url>
  </vkopenurldef>

  <item>
    <name>Open Emacs</name>
    <identifier>private.command_e</identifier>
    <autogen>
      __KeyToKey__
      KeyCode::E, ModifierFlag::COMMAND_R,
      KeyCode::VK_OPEN_URL_APP_Emacs,
    </autogen>
  </item>
</root>
```

詳しくはKarabinerの[private.xml設定方法](https://pqrs.org/osx/karabiner/xml.html.ja)を。

### 関連

* [eiel/KarabinerConfig - GitHub](https://github.com/eiel/KarabinerConfig)
