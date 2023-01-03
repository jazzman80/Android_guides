# Notifications
## Создание Channel
В Application создаём канал
```
    @RequiresApi(Build.VERSION_CODES.O)
    fun createNotificationChannel() {
        val name = "Test notification channel"
        val descriptionText = "Simple description text"
        val importance = NotificationManager.IMPORTANCE_DEFAULT

        val channel = NotificationChannel(NOTIFICATION_CHANNEL_ID, name, importance).apply {
            description = descriptionText
        }

        val notificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
        notificationManager.createNotificationChannel(channel)
    }

    companion object{
        const val NOTIFICATION_CHANNEL_ID = "test_channel_id"
    }
```
В OnCreate добавляем
```
        if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O)
            createNotificationChannel()
```
