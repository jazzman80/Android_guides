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
