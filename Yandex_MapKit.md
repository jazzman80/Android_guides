# Yandex MapKit
## Gradle
В Gradle уровня Module добавляем
```
    // Yandex MapKit
    implementation 'com.yandex.android:maps.mobile:4.2.2-full'
```
## Permissions
Для работы необходимы разрешения интернета и определения местоположения    
В манифест добавляем
```
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
    <uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/>
    <uses-permission android:name="android.permission.INTERNET"/>
```
Запрашиваем разрешения во фрагменте с картой   
```
    // Permissions
    private val launcher = registerForActivityResult(
        ActivityResultContracts.RequestMultiplePermissions()
    ) { map ->
        if (!map.values.all { it }) {
            Toast.makeText(
                requireContext(),
                "We need this permissions",
                Toast.LENGTH_SHORT
            ).show()
        }
    }

    private fun checkPermissions() {
        val isAllGranted = REQUEST_PERMISSIONS.all { permission ->
            ContextCompat.checkSelfPermission(
                requireContext(),
                permission
            ) == PackageManager.PERMISSION_GRANTED
        }
        if (!isAllGranted
        ) {
            launcher.launch(REQUEST_PERMISSIONS)
        }
    }

    companion object {
        private val REQUEST_PERMISSIONS: Array<String> = arrayOf(
            Manifest.permission.ACCESS_COARSE_LOCATION,
            Manifest.permission.ACCESS_FINE_LOCATION
        )
    }
```
## Навигация
Камера следит за пользователем и плавно прилетает к нему   
В onCreate добавляем
```
        mapView = binding.mapView
        mapView.map.isRotateGesturesEnabled = false


        locationManager =
            MapKitFactory.getInstance().createLocationManager(LocationActivityType.AUTO_DETECT)
        locationListener = object : LocationListener {
            override fun onLocationUpdated(location: Location) {
                mapView.map.move(
                    CameraPosition(location.position, 15.0f, 0.0f, 0.0f),
                    Animation(Animation.Type.SMOOTH, 2.0f),
                    null
                )
            }

            override fun onLocationStatusUpdated(p0: LocationStatus) {

            }
        }
        
        locationManager.subscribeForLocationUpdates(
            100.0,
            5000,
            100.0,
            true,
            FilteringMode.ON,
            locationListener
        )

        userLocationLayer =
            MapKitFactory.getInstance().createUserLocationLayer(mapView.mapWindow)
        userLocationLayer.isVisible = true
        userLocationLayer.isHeadingEnabled = true


        super.onViewCreated(view, savedInstanceState)
    }

    override fun onStart() {
        mapView.onStart()
        MapKitFactory.getInstance().onStart()
        super.onStart()
    }

    override fun onStop() {
        mapView.onStop()
        MapKitFactory.getInstance().onStop()
        super.onStop()
    }


    override fun onDestroy() {
        super.onDestroy()
        _binding = null
    }
```
## Слой поиска
```
        // Поиск
        SearchFactory.initialize(requireContext())
        searchLayer = SearchFactory.getInstance().createSearchLayer(mapView.mapWindow)
        searchLayer.isVisible = true
        searchLayer.enableMapMoveOnSearchResponse(false)
        searchLayer.submitQuery("Достопримечательности", SearchOptions())
```
