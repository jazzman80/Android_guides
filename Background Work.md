# Worker Manager
Производит работу в фоновом режиме
## Worker
Отдельный класс worker организует работу
```
class SunriseCalculationWorker(
    context: Context,
    workerParams: WorkerParameters
) : Worker(
    context,
    workerParams
) {
    lateinit var times: SunTimes
    var lat: Double = 0.0
    private var lng: Double = 0.0

    companion object {
        const val HOUR = "hour"
        const val MINUTE = "minute"
        const val LAT = "LAT"
        const val LNG = "LNG"

        fun createWorkRequest(lat: Double, lng: Double): OneTimeWorkRequest {
            return OneTimeWorkRequestBuilder<SunriseCalculationWorker>()
                .setInputData(workDataOf(LAT to lat))
                .setInputData(workDataOf(LNG to lng))
                .build()
        }
    }

    override fun doWork(): Result {
        var lat = inputData.getDouble(LAT, 0.0)
        var lng = inputData.getDouble(LNG, 0.0)

        times = SunTimes.compute()
            .now()
            .at(lat, lng)
            .execute()

        val hour = times.rise?.hour ?: 0
        val minute = times.rise?.minute ?: 0

        val result = workDataOf(HOUR to ((hour.times(MINUTES_PER_HOUR) + minute)))

        return Result.success(result)
    }
}
```
## Work Manager
Во фрагменте запускаем 
```
 myWorkRequest = SunriseCalculationWorker.createWorkRequest(
                binding.latitudeInput.text.toString().toDouble(),
                binding.longitudeInput.text.toString().toDouble()
            )

            WorkManager.getInstance(requireContext()).getWorkInfoByIdLiveData(myWorkRequest.id)
                .observe(viewLifecycleOwner) { work ->
                    val setTime = work.outputData.getDouble(HOUR, 0.0) + addedMinutes
                    setAlarm(setTime)
                }
            WorkManager.getInstance(requireContext()).enqueue(myWorkRequest)
```
# Alarm Manager 
Запускает приложение в определённое время
## Reciever
В манифесте необходимо зарегистрировать его
```
        <receiver
            android:name=".AlarmReceiver"
            android:exported="false">
        </receiver>
```
А также дать необходимые разрешения
```
    <uses-permission android:name="android.permission.SCHEDULE_EXACT_ALARM" />
```
Запускается по окончании работы
```
class AlarmReceiver : BroadcastReceiver() {
    override fun onReceive(context: Context?, intent: Intent?) {
        val notification = RingtoneManager.getDefaultUri(RingtoneManager.TYPE_NOTIFICATION)
        RingtoneManager.getRingtone(context, notification).play()
    }
}
```
