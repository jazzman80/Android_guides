# Camera
## Gradle
В Gradle уровня Module прописать
```
    // Camera
    implementation 'androidx.camera:camera-core:1.2.0'
    implementation 'androidx.camera:camera-camera2:1.2.0'
    implementation 'androidx.camera:camera-lifecycle:1.2.0'
    implementation 'androidx.camera:camera-video:1.2.0'
    implementation 'androidx.camera:camera-view:1.2.0'
    implementation 'androidx.camera:camera-extensions:1.2.0'
```
## Preview
Во фрагмент прописываем параметры
```
    private var imageCapture: ImageCapture? = null
    private lateinit var executor: Executor
```
В OnCreateView инициализируем executor
```
    executor = ContextCompat.getMainExecutor(requireContext())
```
Создаём функцию старта камеры, которую нужно вызвать после получения или подтверждения разрешений
```
    // Camera
    private fun startCamera() {
        val cameraProviderFuture = ProcessCameraProvider.getInstance(requireContext())
        cameraProviderFuture.addListener({
            val cameraProvider = cameraProviderFuture.get()
            val preview = Preview.Builder().build()
            preview.setSurfaceProvider(binding.cameraPreview.surfaceProvider)
            imageCapture = ImageCapture.Builder().build()

            cameraProvider.unbindAll()
            cameraProvider.bindToLifecycle(
                this,
                CameraSelector.DEFAULT_BACK_CAMERA,
                preview,
                imageCapture
            )
        }, executor)
    }
```
