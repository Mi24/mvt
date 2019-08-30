MVTファイル作成要領（その1）

- 作成環境 WSL（UBUNTU 18.04)

## shp  => geojson

    $ ogr2ogr  -f geoJSON xxx.geojson xxx.shp -oo ENCODING=CP932
※国土数値情報の文字エンコードはShiftJIS  
※このままだとShiftJIS(CP932)のままなのでvscodeでエンコードをUTF8に変換

win10:C:\Program Files\QGIS 3.4\bin\ogr2ogr.exe  
UBUNTU:sudo apt-get install gdal-binでインストール

## geojson => PBF or MVT

    $ tippecanoe -l layername -rg -zg -Z6 -o xxx.mbtiles xxx.geojson --force 

-z  最大ズームレベル(z18だと馬鹿みたいにファイルができるので程々に)  
-Z  最小ズームレベル  
-rg レート指定（後述）  
-l  レイヤー名  
-f 上書き  

[tippecanoe document](https://github.com/mapbox/tippecanoe)

    $ mb-util --image_format=pbf xxx.mbtiles tile
    $ cd tile  
    $ gzip -d -v -r -S .pbf *
    $ find . -type f -exec mv -v '{}' '{}'.mvt \;

## github でhost

/docs以下が配信される。

ogr2ogr  -f geoJSON xxx.geojson xxx.shp -oo ENCODING=CP932 -lco COORDINATE_PRECISION=5 WRITE_BBOX=YES

tippecanoe -l gs_map -rg -zg -Z6 -o /mnt/c/project/tochi_riyou/L03-b16_513x.mbtiles  /mnt/c/project/tochi_riyou/L03-b-16_5133.geojson  /mnt/c/project/tochi_riyou/L03-b-16_5134.geojson /mnt/c/project/tochi_riyou/L03-b-16_5233.geojson /mnt/c/project/tochi_riyou/L03-b-16_5234.geojson --force  --drop-fraction-as-needed

mb-util --image_format=pbf /mnt/c/project/tochi_riyou/L03-b16_513x.mbtiles /mnt/c/project/tochi_riyou/tile