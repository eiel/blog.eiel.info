---

title: "knife のログレベルの設定"
date: 2013-12-17T23:46:00+09:00
comments: true
categories: ["chef"]
---


knife solo でエラーがでるけど、どこで起きてるかわからねぇ。

というわけで、pry を差し込みして調べた。ぐぐってトップのほうにあるのは
「やり方がわからない!」って、なっていた。悲しい。

結論としては、knifeの設定ファイルである `.chef/knife.rb` や
`~/.chef/knife.rb` に以下を追記すればいい

```
verbosity       :debug
```


`:debug` としてるのは分かりやすくかいただけで、 `nil` や `0` 、`1` で
ない値であればいい。

参考:

[https://github.com/opscode/chef/blob/11.8.2/lib/chef/knife.rb#L373-L380](https://github.com/opscode/chef/blob/11.8.2/lib/chef/knife.rb#L373-L380)

```
case Chef::Config[:verbosity]
when 0, nil
  Chef::Config[:log_level] = :error
when 1
  Chef::Config[:log_level] = :info
else
  Chef::Config[:log_level] = :debug
end
```

これぐらいならドキュメントにかいてありそうだけどソースをみてしまった。

### 追記

`-V` でログレベルが変わる模様 `-VV` で、さらに変化する。
`-V` で出力がかわんなかったから油断した。(いいわけ)

### 関連

* [やっと Chef Solo はじめた](/blog/2013/11/13/abc-chef-solo/)
