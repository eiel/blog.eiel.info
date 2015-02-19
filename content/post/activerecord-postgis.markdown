---

title: "PostGIS関連でRailsの本番環境でエラー"
date: 2014-12-13T05:51:00+09:00
comments: true
categories: ["activerecord","postgis"]
---

PostGIS
開発環境ではおきないのだけど、pointを作ろうとしてエラーが起きる。

```
NoMethodError (undefined method `point' for #<Proc:0x000000038dde90>)
```

見事に下記の通りだった。

https://github.com/rgeo/activerecord-postgis-adapter/issues/63

```
self.lonlat = Pin.rgeo_factory_for_column(:latlon).point(self.longitude, self.latitude)
```

だったのを

```
self.lonlat = Pin.rgeo_factory_for_column(:latlon, {}).point(self.longitude, self.latitude)
```
に書き換え。`rgeo_factory_for_column`の第2引数に `{}`を加えた。
