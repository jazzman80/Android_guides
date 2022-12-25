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
## Получение координат
```
    private lateinit var fusedClient: FusedLocationProviderClient
    
    ...
    
    override fun onStart() {
        super.onStart()
        checkPermissions()
    }

    override fun onStop() {
        super.onStop()
        fusedClient.removeLocationUpdates(locationCallback)
    }

    private val locationCallback = object :LocationCallback(){
        override fun onLocationResult(result: LocationResult) {
            binding.textView.text = result.lastLocation.toString()
        }
    }

    @SuppressLint("MissingPermission")
    private fun startLocation(){
        val request = LocationRequest.Builder(Priority.PRIORITY_HIGH_ACCURACY, 1_000).build()

        fusedClient.requestLocationUpdates(request, locationCallback, Looper.getMainLooper())
    }
```
