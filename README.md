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
