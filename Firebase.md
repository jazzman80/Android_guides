# Firebase
## Подключение
Подключение Firebase подробно описано на сайте
## Crashlytics
Отключение crashlytics для дебажных версий    
В OnCreate Application
```
FirebaseCrashlytics.getInstance().setCrashlyticsCollectionEnabled(!BuildConfig.DEBUG)
```
Дополнительное логирование
```
FirebaseCrashlytics.getInstance().log("My Log")
```
Пойманные исключения - в блок catch добавляем
```
FirebaseCrashlytics.getInstance().recordException(e)
```
## Notification Messages
Отправляются автоматически, если приложение не запущено или не в фокусе     
В Gradle уровня Module добавляем
```
implementation 'com.google.firebase:firebase-messaging-ktx'
```
В манифест, внутри тега application добавляем
```
        <meta-data
            android:name="com.google.firebase.messaging.default_notification_icon"
            android:resource="@drawable/ic_notification"/>
```
## Data Messages
Пишем дополнительный класс сервиса уведомлений
```
import com.google.firebase.messaging.FirebaseMessagingService
import com.google.firebase.messaging.RemoteMessage

class FcmService: FirebaseMessagingService() {
    override fun onMessageReceived(message: RemoteMessage) {
        // Пишем Notification, который может принимать данные из message
        val notification = NotificationCompat.Builder(this, App.NOTIFICATION_CHANNEL_ID)
            .setSmallIcon(R.drawable.ic_notification)
            .setContentTitle(message.data["title"])
            .setContentText(message.data["text"])
            .setAutoCancel(true)
            .build()

        NotificationManagerCompat.from(this).notify(Random.nextInt(), notification)

        super.onMessageReceived(message)
    }

    override fun onNewToken(token: String) {
        // Действия при обновлении fcm токена - обычно сохранение его на сервер

        super.onNewToken(token)
    }
}
```
В манифест, внутри тега application добавляем
```
        <service android:name=".FcmService"
            android:exported="false">
            <intent-filter>
                <action android:name="com.google.firebase.MESSAGING_EVENT"/>
            </intent-filter>
        </service>
```
