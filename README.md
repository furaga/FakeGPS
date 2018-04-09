# FakeGPS

AndroidのGPSの位置を任意の座標に変更する。
エミュレータ上でなんとなく動作していることを確認した。

# About Implementation

GoogleApiClientを使う。
NOTE: Androidのチュートリアル http://android.xsoftlab.net/training/location/location-testing.html は情報が古い。最新のAndroid StudioだとLocationClientがimportできない。

### AndroidでGoogleMapを表示する
プロジェクトの作成・認証については以下を参考にした
https://qiita.com/rkonno/items/8bec3d5a45235fc88a08

### GPS座標の取得方法
LocationManager, LocationListenerの使い方
http://ria10.hatenablog.com/entry/20121109/1352470160
http://www.adakoda.com/android/000125.html
https://qiita.com/yasumodev/items/5f0f030f0ebfcecdff11

### GPS座標の設定方法
LocationManagerを使ったGPSの位置情報の改ざん
https://www.codota.com/android/methods/android.location.LocationManager/setTestProviderLocation

ACCESS_MOCK_LOCATIONをパーミッションに追加した上で以下の関数を呼べばよい。
- addTestProvider(), 
- setTestProviderLocation(), 
- setTestProviderStatus(), 
- setTestProviderEnabled(), 

setTestProviderLocationに与えるLocationは、緯度・経度・時刻を適切に設定しないとエラーが出る。

```java
Location location = new Location();
location.setLongitude(longitude);
location.setLatitude(latitude);
location.setTime(System.currentTimeMillis());
```

# Trouble shooting

Android開発しててはまったところ

### Android Studioでプロジェクト開きなおしたら"Please select android sdk" というエラーが出るようになった
build.gradleと同期すればよい。
https://stackoverflow.com/questions/34353220/android-studio-please-select-android-sdk

### LocationListener.onLocationChanged()が呼ばれなくなった
前回の実行時に正しく終了処理しなかったのが原因。VM・端末を再起動するとひとまず直るはず。
根本的には、終了処理をきちんと呼ぶようにコードを修正する（例：clearLM()）

