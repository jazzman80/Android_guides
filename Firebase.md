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
