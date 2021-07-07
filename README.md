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

### [2021-07-07]
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
  - MainActivity  
    1.알람을 ON/OFF 하는 기능을 구현한 버튼  
    2.알람시간을 설정하기 위한 다이얼로그를 띄우는 버튼  
    3.설정한 알람시간을 sharedPreferences를 통해 저장하고 PM, AM, 시, 분 을 구분하여 뷰에 표시  
    ```KOTLIN
    * 알람 ON/OFF 여부에 따라서 저장한 알람시간이 되었을 때, 알람을 울릴지 말지를 구분지어야한다.
    * onCreate에서 각각의 버튼을 담당하는 함수를 호출하고, SharedPreferences로 저장한 알람시간에 관한 데이터를 가져요고 뷰에 렌더링해준다.
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        initOnOffButton() // 알람 ON/OFF
        initChangeAlarmTimeButton() // 알람시간 설정

        val model = fetchDataFromSharedPreferences() //step1 데이터 가져오기
        renderView(model) //step2 뷰에 데이터 그려주기(렌더링)
    }  
    ```
  - AlarmDisplayModel : 알람 관련 데이터를 보관하는 모델클래스이다.  
    ```KOTLIN
    * 직관적으로 이해가 되는 모델클레스이기 때문에 자세한 설명은 생략하며, 
    * 코틀린에서 get(), set()부분에 대해서 추가적인 내용에대해서는 아래에 정리해 두었다.
    data class AlarmDisplayModel (
        val hour: Int,
        val minute: Int,
        var onOff: Boolean
    ) {
        val timeText: String
            get() {
                val h = "%02d".format(if(hour < 12) hour else hour - 12)
                val m = "%02d".format(minute)

                return "$h:$m"
            }

        val ampmText: String
            get() {
                return if(hour < 12) "AM" else "PM"
            }

        val onOffText: String
            get() {
                return if(onOff) "알람 끄기" else "알람 켜기"
            }

        fun makeDataForDB(): String {
            return "$hour:$minute"
        }
    }
    ```
  - AlarmReceiver : BroadcastReceiver()를 상속받아 알람시간에 맞추어 notification을 띄워준다.
    ```KOTLIN
    
    ``` 
  

💡 PendingIntent 란??[📌](https://www.charlezz.com/?p=861)  
+ 간략설명  
  PendingIntent는 Intent를 가지고 있는 클래스로, 기본 목적은 다른 애플리케이션(다른 프로세스)의 권한을 허용하여  
  가지고 있는 Intent를 마치 본인 앱의 프로세스에서 실행하는 것 처럼 사용하게 하는 것이다.  
+ 용도  
  Notification으로 작업을 수행할 때 인텐트가 실행되도록 한다.  
  Notification은 안드로이드 시스템의 NotificationManager가 Intent를 실행시킨다.  
  즉, 다른 프로세스에서 수행하기 때문에 Notification으로 Intent 수행시 PendingIntent를 사용하는것은 필수적인 부분이다.  
  ```KOTLIN
  1. 런처 바탕화면의 위젯으로 Intent 작업을 수행할 때  
  2. AlarmManager를 통해 지정된 시간에 Intent 작업이 시작되도록 할 때
  ```
  + 방법
  PendingIntent는 컴포넌트의 유형에 따라 생성자 메서드를 호출하는 방법이 다르다.  
  ```KOTLIN
  1. Activity를 시작하는 Intent의 경우 PendingIntent.getActivity()
  2. Service를 시작하는 Intent의 경우 PendingIntent.getService()
  3. BroadcastReceiver를 시작하는 Intent의 경우 PendingIntent.getBroadcast()
  ```
  
  
💡 코틀린 캐스팅 is, as  
💡 코틀린 get,set [📌](https://bbaktaeho-95.tistory.com/27), [📌](https://ddolcat.tistory.com/577), [📌](https://hongku.tistory.com/347)  
```KOTLIN
자바의 경우 getter와 setter를 직접 구현해야 한다. 다행이도 안드로이드 스튜디오에서는 자동 생성을 지원한다.  
코틀린의 경우에는 위의 번거로운 작업을 하지 않아도 된다. 변수를 만들면 자동으로 getter와 setter를 내부적으로 생성해준다.(눈에 보이지 않을 뿐!!)  
참고로 var로 선언한 변수의 경우 getter와 setter를 모두 생성해 주며, val로 생성한 경우에는 값을 변경할 수 없기 때문에 getter만 내부적으로 자동 생성된다.

이 프로젝트의 모델 클레스에서 get()을 따로 사용한 이유는 데이터를 커스텀하여 가져오기 위함이다. (커스텀 접근자) 해당부분들에 대해서는 위의 링크에 자세한 설명이 담겨있다!
```
💡 tag  
```KOTLIN
Tags can also be used to store data within a view without resorting to another data structure
```
데이터 저장하는데 활용하는 것 같은데, 이 프로젝트에서는 ON/OFF 버튼 관련해서 모델클래스를 통해 뷰를 렌더링 할 때 사용하였다. 추가적으로 정보탐색이 필요함! 
