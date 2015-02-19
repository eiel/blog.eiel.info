---

title: "PostGISを試してみる"
date: 2014-12-11T13:54:00+09:00
comments: true
categories: ["postgres"]
---

緯度経度って奴をDBにいれる。
Postgresなら[PostGIS](http://postgis.net/)って奴があるらしい。

とりあえずローカルで試す。

```
brew install postgresql
brew install postgis
```

データベースを作る

```
createdb postgis_sample
psql -d postgis_sample -c "CREATE EXTENSION postgis;"
```

`CREATE EXTENSION postgis;` すれば postgis が使えるらしい。

八丁堀から510ビルまでの距離を求めてみる。

```
$ psql -d postgis_sample -c "SELECT ST_Distance(ST_GeographyFromText('Point(132.463495 34.393817)'), ST_GeographyFromText('Point(132.468527 34.393366)'));"
  st_distance
---------------
 465.421862299
```

ST_Distanceで距離を算出できる。

テーブルをつくってみる。

```
CREATE table places ( name VARCHAR(255),geog GEOGRAPHY(Point));
INSERT INTO places  VALUES ('ShakeHands', 'POINT(132.458767 34.394010)');
INSERT INTO places  VALUES ('MOVIN''ON', 'POINT(132.465314 34.393052)');
```

シャレオ中央からの距離を求めてみる。

```
> SELECT name, ST_Distance(geog, ST_GeographyFromText('POINT(132.457589 34.395300)')) FROM places;

ShakeHands | 179.475100761
 MOVIN'ON   | 752.858070591
```

500m 以内の絞り込みをしてみる。

```
SELECT name FROM places WHERE ST_Distance(geog, ST_GeographyFromText('POINT(132.457589 34.395300)')) < 500;

ShakeHands
```

3000件つっこんで検索してみる

```
$ for i in `seq 3000`
> do
>   psql -d postgis_sample -c "INSERT INTO places  VALUES ('$i', 'POINT(132.$i 34.$i)');"
> done

$ psql -d postgis_sample

> /timing

> SELECT name FROM places WHERE ST_Distance(geog, ST_GeographyFromText('POINT(132.457589 34.395300)')) < 5000

Time: 12.816 ms;
```

12msだった。ジオグラフィ型だと計算量があれこれらしいの狭い範囲ならジオメトリでやるほうがよいようだけど、どうしたらよいかわからないけど、ジオグラフィのままでも場合によっては良さそう。

### 参考文献

* [Boundless : Introduction to PostGIS (Japanese) : 第17章: ジオグラフィー型](http://workshops.boundlessgeo.com/postgis-intro-jp/geography.html)
* [Googleマップで緯度・経度を求める （拡張版）](http://user.numazu-ct.ac.jp/~tsato/tsato/geoweb/googlemaps/coordinates/advanced.html)
* [PostGIS クエリーの基本 — GeoPacific.org](http://www.geopacific.org/opensourcegis/postgis/postgis-30af30a830ea30fc306e57fa672c)
