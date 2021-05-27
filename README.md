```💡 FastCampus 강의 수강 및 정리```

### Alarm
+ AlarmManager
+ Notification
+ Broadcast Receiver

### Background
+ Immediate tasks (즉시 실행해야하는 작업)
  - Thread
  - Handler
  - Kotlin coroutines  
+ Deferred tasks (지연된 작업)
  - WorkManager  
+ Exact tasks (정시에 실행해야하는 작업)
  - AlarmManager
  ```KOTLIN
  본 코드에서는 비정확한 setInexacRepeating API를 사용함

  alarmManager.setInexacRepeating(
      AlarmManager.RTC_WAKEUP,
      calendar.timeInMillis,
      AlarmManager.INTERVAL_DAY,
      pendingIntent
  )

  이를 정확한 시간에 하기위해선 setExact 를 사용해야한다.

  또한 최근 안드로이드 폰에서는 Doz모드(수면상태)가 적용되기 때문에
  이경우에도 알람을 허용시키기 위해서는 
  alarmManager.setAndAllowWhileIdle() 이나 
  alarmManager.setAndAllowIdle() 를 사용해야한다.
  ```
### AlarmManager
+ Real Time : 실제시간으로 실행
+ Elapsed Time : 기기가 부팅되거 얼마나 지났는지로 실행

#### 기능
+ 지정된 시간에 알람이 울림
+ 지정된 시간 이후에는 매일 같은 시간에 반복된 알람설정


<img src="https://user-images.githubusercontent.com/63087903/119833597-33066e80-bf3a-11eb-94e2-16dd5039135a.jpg" width="200" height="430"> <img src="https://user-images.githubusercontent.com/63087903/119833612-36015f00-bf3a-11eb-9a36-6b7f5382aa89.jpg" width="200" height="430"> <img src="https://user-images.githubusercontent.com/63087903/119833607-34d03200-bf3a-11eb-8137-d20f9ac98627.jpg" width="200" height="430">

💡 PendingIntent 란??
