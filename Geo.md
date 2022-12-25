# GeoLocation
## Gradle
В Gradle уровня Module добавляем
```
implementation 'com.google.android.gms:play-services-location:21.0.1'
```
## Manifest
Добавляем разрешения
```
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
```
