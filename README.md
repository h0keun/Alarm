```💡 FastCampus 강의 수강 및 정리```

### Alarm
+ AlarmManager
+ Notification
+ Broadcast Receiver
+ SharedPreferences

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

### [2021-07-06]
#### xml
+ Constraint를 이용하여 각 뷰들을 배치시켰으며 특히 가운데에 나타나는 00:00(시간) 과 AM(PM) 표시를  
  다른 영역과 구분짓기위해 background가 원의 형태인 drawablelayout을 구현하고 이를  
  xml에 생성한 View의 background에 지정 및 DimensionRatio를 1:1로 하여 화면 정중앙에 위치하도록 하였다.
  ```KOTLIN
  drawable레이아웃의 속성으로는 
  shape = "oval" // 원의 형태
  solid color = "@color/transparent" // 색상은 투명하게하여 뒤가 비치도록
  stroke width, color = 1dp, white // 1dp사이즈의 흰색 테두리
  ```
  딱히 xml단에서는 어려운 부분이 없다
#### Manifest
+ BroadcastReceiver() 를 상속받는 AlarmReceiver를 따로 메니페스트에 추가
  ```KOTLIN
  <receiver android:name=".AlarmReceiver"
       android:exported="false"/>
  ```
  android:exported 는 activity 또는 receiver 등에 설정할수 있으며 다른 애플리케이션의 구성요소로 activity를 시작할 수 있는지를 설정한다.  
  (다른 애플리케이션에 activity를 노출시키냐 마느냐)  
  다른 앱에서 activity를 시작할 수 있으면 "true" 로 설정하고, 다른 앱에서 시작할 수 없으면 "false"로 설정한다.  
  즉 위의 경우에서는 false로 두었기 때문에 해당 activity는 같은 앱 또는 사용자 ID가 같은 앱에서만 시작 할 수 있다.[📌](https://m.blog.naver.com/websearch/221668354461)
  
#### Kotlin class
+ alarm 패키지 내에 MainActivity, AlarmReceiver, AlarmDisplayModel 의 3의 코틀린 클래스들을 생성하였고 각각의 역할은 다음과 같다.  
  - MainActivity :
  - AlarmDisplayModel : 알람 관련 데이터를 보관하는 모델클래스
  - AlarmReceiver :
  - 내가 너무 게을러서 7월 6일에 다시 작성하쟈ㅠ
  
  

💡 PendingIntent 란??
