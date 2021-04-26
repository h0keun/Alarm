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
  
### AlarmManager

+ Real Time : 실제시간으로 실행
+ Elapsed Time : 기기가 부팅되거 얼마나 지났는지로 실행

#### 기능

+ 지정된 시간에 알람이 울림
+ 지정된 시간 이후에는 매일 같은 시간에 반복된 알람설정
