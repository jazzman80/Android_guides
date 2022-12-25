# Permissions
## Manifest
Добавляем в манифест разрешения
```
uses-permission
```
## Проверка и запрос разрешений
На примере разрешений для камеры внутри фрагмента
```
    // Permissions
    private val launcher: ActivityResultLauncher<String> = registerForActivityResult(
        ActivityResultContracts.RequestPermission()
    ) {isGranted ->
        if (!isGranted) {
            Toast.makeText(requireContext(), getString(R.string.permission_request), Toast.LENGTH_SHORT).show()
        }
    }

    private fun checkPermissions() {
        if (ContextCompat.checkSelfPermission(
                requireContext(),
                CAMERA
            ) == PackageManager.PERMISSION_GRANTED
        ) {

        } else {
            launcher.launch(CAMERA)
        }
    }
```
## Запрос нескольких разрешений
```
    // Permissions
    private val launcher = registerForActivityResult(
        ActivityResultContracts.RequestMultiplePermissions()
    ) { map ->
        if (!map.values.all { it }) {
            Toast.makeText(
                requireContext(),
                getString(R.string.permission_request),
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
        private val REQUEST_PERMISSIONS: Array<String> = buildList {
            add(Manifest.permission.CAMERA)
            if (Build.VERSION.SDK_INT <= Build.VERSION_CODES.P) {
                add(Manifest.permission.WRITE_EXTERNAL_STORAGE)
            }
        }.toTypedArray()
    }
```
