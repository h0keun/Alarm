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
+ alarm 패키지 내에 MainActivity, AlarmReceiver, AlarmDisplayModel 의 3개의 코틀린 클래스들을 생성하였다.  
  - MainActivity  
    1.알람을 ON/OFF 하는 기능을 구현  
    2.알람시간 설정 및 변경을 위한 다이얼로그를 띄우고 sharedPreferences를 통해 저장  
    3.저장된 데이터를 가져와서 PM, AM, 시, 분 을 구분하여 뷰에 표시  
    ```KOTLIN
    * onCreate에서는 각각의 버튼을 담당하는 함수를 호출하고, 
    * SharedPreferences로 저장한 알람시간에 관한 데이터를 가져오고 뷰에 렌더링해준다.
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        initOnOffButton() // 알람 ON/OFF
        initChangeAlarmTimeButton() // 알람시간 설정

        val model = fetchDataFromSharedPreferences() //step1 데이터 가져오기
        renderView(model) //step2 뷰에 데이터 그려주기(렌더링)
    }
    
    * initOnOffButton()과 initChangeAlaramTimeButton()은 각각 버튼클릭 리스너를 통해  
      함수가 동작하기 때문에 초기에 뷰를 그리는데 영향을 주지 않는다.
      
    * 때문에 데이터를 가져오는 함수가 실행되는 부분에서 sharedPreferences를 선언하고,  
      시간값 "00:00" 을 기본값으로, onoff여부는 false를 기본값으로 두어 모델클래스에 값을 할당한다.
      이를 통해 모델값을 렌더링하여 뷰를 그릴때에 값이 존재하지 않아 에러가 나는 상황을 방지하였다.
      
    private fun renderView(model: AlarmDisplayModel) {
        findViewById<TextView>(R.id.ampmTextView).apply {
            text = model.ampmText
        }
        findViewById<TextView>(R.id.timeTextView).apply {
            text = model.timeText
        }
        findViewById<Button>(R.id.onOffButton).apply {
            text = model.onOffText
            tag = model
        }
    }
    ```
    모든 코드의 내용을 본문에 남을 수 없기 때문에 간단하게 구두로 작성하며 추후 보게 된다면, 해당코드를 띄워놓고 보는게 좋을 것 같다.  
    
    ◼ 알람 ON/OFF
    ```KOTLIN
    private fun initOnOfButton(){
    ...
    }    
    * 새로운 model을 정의하여 모델클레스의 정보를 할당한다. 
      fetchDataFromSharedPreferences() 에서 모델클래스에 값을 할당해 주었지만,  
      혹시모를 null에러를 방지하기위해 ?: return@setOnClickListener 를 통해 대응해 준다.
        
    * 모델클래스의 onoff 여부를 if~else 문을 통해 읽어들여서 처리하게 된다.  
      위에서 선언한 model 값이 모델클래스로부터 null이 아닌 값으로 잘 넘어왔다면  
      newModel이라는 새로운 객체를 생성하여 saveAlarmModel()함수에 정보를 담아 실행시키고,  
      또한 renderView(model)를 통해 saveAlarmModel에서 리턴으로 받아온 모델값을 할당해주고 화면을 갱신한다.
      
    private fun saveAlarmModel(hour: Int, minute: Int, onOff: Boolean) : AlarmDisplayModel {
    ...
    }
    * sharedpreferences를 통해 시간값과 onOff값을 저장해주고 모델클래스 값들을 리턴해준다.(기본값은 이미 선언되어있음!) 
      
    if (newModel.onOff) {
    ...
    } else {
        cancelAlarm()
    }
    * fetchDataFromSharedPreferences() 에서 sharedPreferences를 선언하고 시간값은 "00:00" 의 기본값을,  
      onoff에 대해선 false의 기본값을 지정해주고 모델클래스에 담아주었다. 때문에 initOnOffButton()에서 클릭이벤트 발생시에   
      val newModel = saveAlarmModel(model.hour, model.minute, model.onOff.not()) // not() 이기 때문에 newModel.onOff는 true가 됨!
      때문에 버튼 클릭할 때마다 if문과 else문이 번갈아가며 실행된다. 
    
    * if문 => ON(true) 일 때는 알람메니져를 실행시키고,
      val alarmManager = getSystemService(Context.ALARM_SERVICE) as AlarmManager
      val intent = Intent(this, AlarmReceiver::class.java)
      val pendingIntent = PendingIntent.getBroadcast(this, ALARM_REQUEST_CODE,
              intent, PendingIntent.FLAG_UPDATE_CURRENT) // 현재가지고 있는 정보로 알람매니저 업데이트

      alarmManager.setInexactRepeating(
          AlarmManager.RTC_WAKEUP,
          calendar.timeInMillis,
          AlarmManager.INTERVAL_DAY,
          pendingIntent
      )
    
    * false 일 때는 따로 지정한 함수 cancelAlarm() 을 실행 시켜서 알람메니져가 실행되는것을 막는다.
    private fun cancelAlarm() {
        val pendingIntent = PendingIntent.getBroadcast(
            this,
            ALARM_REQUEST_CODE,
            Intent(this, AlarmReceiver::class.java),
            PendingIntent.FLAG_NO_CREATE // 저장되어있던 알람매니저 정보 삭제
        )
        pendingIntent?.cancel()
    }    
    ```
    ◼ 알람시간 설정 및 변경
    ```KOTLIN
    private fun initChangeAlarmTimeButton() {
        val changeAlarmButton = findViewById<Button>(R.id.changeAlarmTimeButton)
        changeAlarmButton.setOnClickListener {

            val calendar = Calendar.getInstance()
            TimePickerDialog(this, { picker, hour, minute ->

                val model = saveAlarmModel(hour, minute, false)
                renderView(model)
                cancelAlarm()

            }, calendar.get(Calendar.HOUR_OF_DAY), calendar.get(Calendar.MINUTE), false)
                .show()
        }
    }
    
    * 버튼클릭이벤트 발생시 캘린더를 읽어들여서 시간값을 적용한 다이얼로그를 띄워준다.
      또한 각각의 정보들은 뷰를 갱신시키고(rederView) 기존에 존재하는 알람정보를 삭제(cancelAlarm)시켜주며,
      saveAlarmModel을 통해 갱신된 시간정보를 저장하게된다.
      
    * 이후의 과정은 위와 동일한 상태가 됨!
    ```
    ◼ 알람관련 데이터 가져오기 및 화면에 뿌리기
    ```KOTLIN
    * 위의 과정속에 저장되는 데이터정보를 받아 모델클래스에 할당하고 모델데이터값을 리턴시켜서 
      onCreate의 renderView()를 통해 화면을 그리게 된다.
      
    * 이곳에서 수행하는 예외처리는 다음과같다.
    val pendingIntent = PendingIntent.getBroadcast(
            this,
            ALARM_REQUEST_CODE,
            Intent(this, AlarmReceiver::class.java),
            PendingIntent.FLAG_NO_CREATE // 저장되어있던 알람매니저 정보 삭제 실제 내용은 아래와 같음!
        )                                
     // Flag indicating that if the described PendingIntent already exists,
     // the current one should be canceled before generating a new one. 
     
        if ((pendingIntent == null) and alarmModel.onOff) {
            //알람은 꺼져있는데, 데이터는 켜져있는 경우
            alarmModel.onOff = false
        } else if ((pendingIntent != null) and alarmModel.onOff.not()) {
            //알람은 켜져있는데, 데이터는 꺼져있는 경우
            //알람을 취소함
            pendingIntent.cancel()
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
    딱히 어려운 내용이없어서 코드로 대체함!
    ```KOTLIN
    class AlarmReceiver: BroadcastReceiver() {

        companion object {
            const val NOTIFICATION_ID = 100
            const val NOTIFICATION_CHANNEL_ID = "1000"
        }

        override fun onReceive(context: Context, intent: Intent) {

            createNotificationChannel(context)
            notifyNotification(context)
        }

        private fun createNotificationChannel(context: Context) {

            if(Build.VERSION.SDK_INT >= Build.VERSION_CODES.O){
                val notificationChannel = NotificationChannel(
                    NOTIFICATION_CHANNEL_ID,
                    "기상 알림",
                    NotificationManager.IMPORTANCE_HIGH
                )

                NotificationManagerCompat.from(context).createNotificationChannel(notificationChannel)
            }
        }

        private fun notifyNotification(context: Context){
            with(NotificationManagerCompat.from(context)){
                val build = NotificationCompat.Builder(context, NOTIFICATION_CHANNEL_ID)
                    .setContentTitle("알림")
                    .setContentText("일어날 시간입니다.")
                    .setSmallIcon(R.drawable.ic_launcher_foreground)
                    .setPriority(NotificationCompat.PRIORITY_HIGH)

                notify(NOTIFICATION_ID, build.build())
            }
        }
    }
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

💡 코틀린 타입캐스팅 알아보기
