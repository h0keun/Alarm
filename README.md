```๐ก FastCampus ๊ฐ์ ์๊ฐ ๋ฐ ์ ๋ฆฌ```

### Alarm
+ AlarmManager
+ Notification
+ Broadcast Receiver
+ SharedPreferences

### Background
+ Immediate tasks (์ฆ์ ์คํํด์ผํ๋ ์์)
  - Thread
  - Handler
  - Kotlin coroutines  
+ Deferred tasks (์ง์ฐ๋ ์์)
  - WorkManager  
+ Exact tasks (์ ์์ ์คํํด์ผํ๋ ์์)
  - AlarmManager
  ```KOTLIN
  ๋ณธ ์ฝ๋์์๋ ๋น์ ํํ setInexacRepeating API๋ฅผ ์ฌ์ฉํจ

  alarmManager.setInexacRepeating(
      AlarmManager.RTC_WAKEUP,
      calendar.timeInMillis,
      AlarmManager.INTERVAL_DAY,
      pendingIntent
  )

  ์ด๋ฅผ ์ ํํ ์๊ฐ์ ํ๊ธฐ์ํด์  setExact ๋ฅผ ์ฌ์ฉํด์ผํ๋ค.

  ๋ํ ์ต๊ทผ ์๋๋ก์ด๋ ํฐ์์๋ Doz๋ชจ๋(์๋ฉด์ํ)๊ฐ ์ ์ฉ๋๊ธฐ ๋๋ฌธ์
  ์ด๊ฒฝ์ฐ์๋ ์๋์ ํ์ฉ์ํค๊ธฐ ์ํด์๋ 
  alarmManager.setAndAllowWhileIdle() ์ด๋ 
  alarmManager.setAndAllowIdle() ๋ฅผ ์ฌ์ฉํด์ผํ๋ค.
  ```
### AlarmManager
+ Real Time : ์ค์ ์๊ฐ์ผ๋ก ์คํ
+ Elapsed Time : ๊ธฐ๊ธฐ๊ฐ ๋ถํ๋๊ฑฐ ์ผ๋ง๋ ์ง๋ฌ๋์ง๋ก ์คํ

#### ๊ธฐ๋ฅ
+ ์ง์ ๋ ์๊ฐ์ ์๋์ด ์ธ๋ฆผ
+ ์ง์ ๋ ์๊ฐ ์ดํ์๋ ๋งค์ผ ๊ฐ์ ์๊ฐ์ ๋ฐ๋ณต๋ ์๋์ค์ 


<img src="https://user-images.githubusercontent.com/63087903/119833597-33066e80-bf3a-11eb-94e2-16dd5039135a.jpg" width="200" height="430"> <img src="https://user-images.githubusercontent.com/63087903/119833612-36015f00-bf3a-11eb-9a36-6b7f5382aa89.jpg" width="200" height="430"> <img src="https://user-images.githubusercontent.com/63087903/119833607-34d03200-bf3a-11eb-8137-d20f9ac98627.jpg" width="200" height="430">

### [2021-07-07]
#### xml
+ Constraint๋ฅผ ์ด์ฉํ์ฌ ๊ฐ ๋ทฐ๋ค์ ๋ฐฐ์น์์ผฐ์ผ๋ฉฐ ํนํ ๊ฐ์ด๋ฐ์ ๋ํ๋๋ 00:00(์๊ฐ) ๊ณผ AM(PM) ํ์๋ฅผ  
  ๋ค๋ฅธ ์์ญ๊ณผ ๊ตฌ๋ถ์ง๊ธฐ์ํด background๊ฐ ์์ ํํ์ธ drawablelayout์ ๊ตฌํํ๊ณ  ์ด๋ฅผ  
  xml์ ์์ฑํ View์ background์ ์ง์  ๋ฐ DimensionRatio๋ฅผ 1:1๋ก ํ์ฌ ํ๋ฉด ์ ์ค์์ ์์นํ๋๋ก ํ์๋ค.
  ```KOTLIN
  drawable๋ ์ด์์์ ์์ฑ์ผ๋ก๋ 
  shape = "oval" // ์์ ํํ
  solid color = "@color/transparent" // ์์์ ํฌ๋ชํ๊ฒํ์ฌ ๋ค๊ฐ ๋น์น๋๋ก
  stroke width, color = 1dp, white // 1dp์ฌ์ด์ฆ์ ํฐ์ ํ๋๋ฆฌ
  ```
  ๋ฑํ xml๋จ์์๋ ์ด๋ ค์ด ๋ถ๋ถ์ด ์๋ค
#### Manifest
+ BroadcastReceiver() ๋ฅผ ์์๋ฐ๋ AlarmReceiver๋ฅผ ๋ฐ๋ก ๋ฉ๋ํ์คํธ์ ์ถ๊ฐ
  ```KOTLIN
  <receiver android:name=".AlarmReceiver"
       android:exported="false"/>
  ```
  android:exported ๋ activity ๋๋ receiver ๋ฑ์ ์ค์ ํ ์ ์์ผ๋ฉฐ ๋ค๋ฅธ ์ ํ๋ฆฌ์ผ์ด์์ ๊ตฌ์ฑ์์๋ก activity๋ฅผ ์์ํ  ์ ์๋์ง๋ฅผ ์ค์ ํ๋ค.  
  (๋ค๋ฅธ ์ ํ๋ฆฌ์ผ์ด์์ activity๋ฅผ ๋ธ์ถ์ํค๋ ๋ง๋๋)  
  ๋ค๋ฅธ ์ฑ์์ activity๋ฅผ ์์ํ  ์ ์์ผ๋ฉด "true" ๋ก ์ค์ ํ๊ณ , ๋ค๋ฅธ ์ฑ์์ ์์ํ  ์ ์์ผ๋ฉด "false"๋ก ์ค์ ํ๋ค.  
  ์ฆ ์์ ๊ฒฝ์ฐ์์๋ false๋ก ๋์๊ธฐ ๋๋ฌธ์ ํด๋น activity๋ ๊ฐ์ ์ฑ ๋๋ ์ฌ์ฉ์ ID๊ฐ ๊ฐ์ ์ฑ์์๋ง ์์ ํ  ์ ์๋ค.[๐](https://m.blog.naver.com/websearch/221668354461)
  
#### Kotlin class
+ alarm ํจํค์ง ๋ด์ MainActivity, AlarmReceiver, AlarmDisplayModel ์ 3๊ฐ์ ์ฝํ๋ฆฐ ํด๋์ค๋ค์ ์์ฑํ์๋ค.  
  - MainActivity  
    1.์๋์ ON/OFF ํ๋ ๊ธฐ๋ฅ์ ๊ตฌํ  
    2.์๋์๊ฐ ์ค์  ๋ฐ ๋ณ๊ฒฝ์ ์ํ ๋ค์ด์ผ๋ก๊ทธ๋ฅผ ๋์ฐ๊ณ  sharedPreferences๋ฅผ ํตํด ์ ์ฅ  
    3.์ ์ฅ๋ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์์ PM, AM, ์, ๋ถ ์ ๊ตฌ๋ถํ์ฌ ๋ทฐ์ ํ์  
    ```KOTLIN
    * onCreate์์๋ ๊ฐ๊ฐ์ ๋ฒํผ์ ๋ด๋นํ๋ ํจ์๋ฅผ ํธ์ถํ๊ณ , 
    * SharedPreferences๋ก ์ ์ฅํ ์๋์๊ฐ์ ๊ดํ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์ค๊ณ  ๋ทฐ์ ๋ ๋๋งํด์ค๋ค.
    
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        initOnOffButton() // ์๋ ON/OFF
        initChangeAlarmTimeButton() // ์๋์๊ฐ ์ค์ 

        val model = fetchDataFromSharedPreferences() //step1 ๋ฐ์ดํฐ ๊ฐ์ ธ์ค๊ธฐ
        renderView(model) //step2 ๋ทฐ์ ๋ฐ์ดํฐ ๊ทธ๋ ค์ฃผ๊ธฐ(๋ ๋๋ง)
    }
    
    * initOnOffButton()๊ณผ initChangeAlaramTimeButton()์ ๊ฐ๊ฐ ๋ฒํผํด๋ฆญ ๋ฆฌ์ค๋๋ฅผ ํตํด  
      ํจ์๊ฐ ๋์ํ๊ธฐ ๋๋ฌธ์ ์ด๊ธฐ์ ๋ทฐ๋ฅผ ๊ทธ๋ฆฌ๋๋ฐ ์ํฅ์ ์ฃผ์ง ์๋๋ค.
      
    * ๋๋ฌธ์ ๋ฐ์ดํฐ๋ฅผ ๊ฐ์ ธ์ค๋ ํจ์๊ฐ ์คํ๋๋ ๋ถ๋ถ์์ sharedPreferences๋ฅผ ์ ์ธํ๊ณ ,  
      ์๊ฐ๊ฐ "00:00" ์ ๊ธฐ๋ณธ๊ฐ์ผ๋ก, onoff์ฌ๋ถ๋ false๋ฅผ ๊ธฐ๋ณธ๊ฐ์ผ๋ก ๋์ด ๋ชจ๋ธํด๋์ค์ ๊ฐ์ ํ ๋นํ๋ค.
      ์ด๋ฅผ ํตํด ๋ชจ๋ธ๊ฐ์ ๋ ๋๋งํ์ฌ ๋ทฐ๋ฅผ ๊ทธ๋ฆด๋์ ๊ฐ์ด ์กด์ฌํ์ง ์์ ์๋ฌ๊ฐ ๋๋ ์ํฉ์ ๋ฐฉ์งํ์๋ค.
      
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
    ๋ชจ๋  ์ฝ๋์ ๋ด์ฉ์ ๋ณธ๋ฌธ์ ๋จ์ ์ ์๊ธฐ ๋๋ฌธ์ ๊ฐ๋จํ๊ฒ ๊ตฌ๋๋ก ์์ฑํ๋ฉฐ ์ถํ ๋ณด๊ฒ ๋๋ค๋ฉด, ํด๋น์ฝ๋๋ฅผ ๋์๋๊ณ  ๋ณด๋๊ฒ ์ข์ ๊ฒ ๊ฐ๋ค.  
    
    โผ ์๋ ON/OFF
    ```KOTLIN
    private fun initOnOfButton(){
    ...
    }    
    * ์๋ก์ด model์ ์ ์ํ์ฌ ๋ชจ๋ธํด๋ ์ค์ ์ ๋ณด๋ฅผ ํ ๋นํ๋ค. 
      fetchDataFromSharedPreferences() ์์ ๋ชจ๋ธํด๋์ค์ ๊ฐ์ ํ ๋นํด ์ฃผ์์ง๋ง,  
      ํน์๋ชจ๋ฅผ null์๋ฌ๋ฅผ ๋ฐฉ์งํ๊ธฐ์ํด ?: return@setOnClickListener ๋ฅผ ํตํด ๋์ํด ์ค๋ค.
        
    * ๋ชจ๋ธํด๋์ค์ onoff ์ฌ๋ถ๋ฅผ if~else ๋ฌธ์ ํตํด ์ฝ์ด๋ค์ฌ์ ์ฒ๋ฆฌํ๊ฒ ๋๋ค.  
      ์์์ ์ ์ธํ model ๊ฐ์ด ๋ชจ๋ธํด๋์ค๋ก๋ถํฐ null์ด ์๋ ๊ฐ์ผ๋ก ์ ๋์ด์๋ค๋ฉด  
      newModel์ด๋ผ๋ ์๋ก์ด ๊ฐ์ฒด๋ฅผ ์์ฑํ์ฌ saveAlarmModel()ํจ์์ ์ ๋ณด๋ฅผ ๋ด์ ์คํ์ํค๊ณ ,  
      ๋ํ renderView(model)๋ฅผ ํตํด saveAlarmModel์์ ๋ฆฌํด์ผ๋ก ๋ฐ์์จ ๋ชจ๋ธ๊ฐ์ ํ ๋นํด์ฃผ๊ณ  ํ๋ฉด์ ๊ฐฑ์ ํ๋ค.
      
    private fun saveAlarmModel(hour: Int, minute: Int, onOff: Boolean) : AlarmDisplayModel {
    ...
    }
    * sharedpreferences๋ฅผ ํตํด ์๊ฐ๊ฐ๊ณผ onOff๊ฐ์ ์ ์ฅํด์ฃผ๊ณ  ๋ชจ๋ธํด๋์ค ๊ฐ๋ค์ ๋ฆฌํดํด์ค๋ค.(๊ธฐ๋ณธ๊ฐ์ ์ด๋ฏธ ์ ์ธ๋์ด์์!) 
      
    if (newModel.onOff) {
    ...
    } else {
        cancelAlarm()
    }
    * fetchDataFromSharedPreferences() ์์ sharedPreferences๋ฅผ ์ ์ธํ๊ณ  ์๊ฐ๊ฐ์ "00:00" ์ ๊ธฐ๋ณธ๊ฐ์,  
      onoff์ ๋ํด์  false์ ๊ธฐ๋ณธ๊ฐ์ ์ง์ ํด์ฃผ๊ณ  ๋ชจ๋ธํด๋์ค์ ๋ด์์ฃผ์๋ค. ๋๋ฌธ์ initOnOffButton()์์ ํด๋ฆญ์ด๋ฒคํธ ๋ฐ์์์   
      val newModel = saveAlarmModel(model.hour, model.minute, model.onOff.not()) // not() ์ด๊ธฐ ๋๋ฌธ์ newModel.onOff๋ true๊ฐ ๋จ!
      ๋๋ฌธ์ ๋ฒํผ ํด๋ฆญํ  ๋๋ง๋ค if๋ฌธ๊ณผ else๋ฌธ์ด ๋ฒ๊ฐ์๊ฐ๋ฉฐ ์คํ๋๋ค. 
    
    * if๋ฌธ => ON(true) ์ผ ๋๋ ์๋๋ฉ๋์ ธ๋ฅผ ์คํ์ํค๊ณ ,
      val alarmManager = getSystemService(Context.ALARM_SERVICE) as AlarmManager
      val intent = Intent(this, AlarmReceiver::class.java)
      val pendingIntent = PendingIntent.getBroadcast(this, ALARM_REQUEST_CODE,
              intent, PendingIntent.FLAG_UPDATE_CURRENT) // ํ์ฌ๊ฐ์ง๊ณ  ์๋ ์ ๋ณด๋ก ์๋๋งค๋์  ์๋ฐ์ดํธ

      alarmManager.setInexactRepeating(
          AlarmManager.RTC_WAKEUP,
          calendar.timeInMillis,
          AlarmManager.INTERVAL_DAY,
          pendingIntent
      )
    
    * false ์ผ ๋๋ ๋ฐ๋ก ์ง์ ํ ํจ์ cancelAlarm() ์ ์คํ ์์ผ์ ์๋๋ฉ๋์ ธ๊ฐ ์คํ๋๋๊ฒ์ ๋ง๋๋ค.
    private fun cancelAlarm() {
        val pendingIntent = PendingIntent.getBroadcast(
            this,
            ALARM_REQUEST_CODE,
            Intent(this, AlarmReceiver::class.java),
            PendingIntent.FLAG_NO_CREATE // ์ ์ฅ๋์ด์๋ ์๋๋งค๋์  ์ ๋ณด ์ญ์ 
        )
        pendingIntent?.cancel()
    }    
    ```
    โผ ์๋์๊ฐ ์ค์  ๋ฐ ๋ณ๊ฒฝ
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
    
    * ๋ฒํผํด๋ฆญ์ด๋ฒคํธ ๋ฐ์์ ์บ๋ฆฐ๋๋ฅผ ์ฝ์ด๋ค์ฌ์ ์๊ฐ๊ฐ์ ์ ์ฉํ ๋ค์ด์ผ๋ก๊ทธ๋ฅผ ๋์์ค๋ค.
      ๋ํ ๊ฐ๊ฐ์ ์ ๋ณด๋ค์ ๋ทฐ๋ฅผ ๊ฐฑ์ ์ํค๊ณ (rederView) ๊ธฐ์กด์ ์กด์ฌํ๋ ์๋์ ๋ณด๋ฅผ ์ญ์ (cancelAlarm)์์ผ์ฃผ๋ฉฐ,
      saveAlarmModel์ ํตํด ๊ฐฑ์ ๋ ์๊ฐ์ ๋ณด๋ฅผ ์ ์ฅํ๊ฒ๋๋ค.
      
    * ์ดํ์ ๊ณผ์ ์ ์์ ๋์ผํ ์ํ๊ฐ ๋จ!
    ```
    โผ ์๋๊ด๋ จ ๋ฐ์ดํฐ ๊ฐ์ ธ์ค๊ธฐ ๋ฐ ํ๋ฉด์ ๋ฟ๋ฆฌ๊ธฐ
    ```KOTLIN
    * ์์ ๊ณผ์ ์์ ์ ์ฅ๋๋ ๋ฐ์ดํฐ์ ๋ณด๋ฅผ ๋ฐ์ ๋ชจ๋ธํด๋์ค์ ํ ๋นํ๊ณ  ๋ชจ๋ธ๋ฐ์ดํฐ๊ฐ์ ๋ฆฌํด์์ผ์ 
      onCreate์ renderView()๋ฅผ ํตํด ํ๋ฉด์ ๊ทธ๋ฆฌ๊ฒ ๋๋ค.
      
    * ์ด๊ณณ์์ ์ํํ๋ ์์ธ์ฒ๋ฆฌ๋ ๋ค์๊ณผ๊ฐ๋ค.
    val pendingIntent = PendingIntent.getBroadcast(
            this,
            ALARM_REQUEST_CODE,
            Intent(this, AlarmReceiver::class.java),
            PendingIntent.FLAG_NO_CREATE // ์ ์ฅ๋์ด์๋ ์๋๋งค๋์  ์ ๋ณด ์ญ์  ์ค์  ๋ด์ฉ์ ์๋์ ๊ฐ์!
        )                                
     // Flag indicating that if the described PendingIntent already exists,
     // the current one should be canceled before generating a new one. 
     
        if ((pendingIntent == null) and alarmModel.onOff) {
            //์๋์ ๊บผ์ ธ์๋๋ฐ, ๋ฐ์ดํฐ๋ ์ผ์ ธ์๋ ๊ฒฝ์ฐ
            alarmModel.onOff = false
        } else if ((pendingIntent != null) and alarmModel.onOff.not()) {
            //์๋์ ์ผ์ ธ์๋๋ฐ, ๋ฐ์ดํฐ๋ ๊บผ์ ธ์๋ ๊ฒฝ์ฐ
            //์๋์ ์ทจ์ํจ
            pendingIntent.cancel()
        }
    ```
    
    
  - AlarmDisplayModel : ์๋ ๊ด๋ จ ๋ฐ์ดํฐ๋ฅผ ๋ณด๊ดํ๋ ๋ชจ๋ธํด๋์ค์ด๋ค.  
    ```KOTLIN
    * ์ง๊ด์ ์ผ๋ก ์ดํด๊ฐ ๋๋ ๋ชจ๋ธํด๋ ์ค์ด๊ธฐ ๋๋ฌธ์ ์์ธํ ์ค๋ช์ ์๋ตํ๋ฉฐ, 
    * ์ฝํ๋ฆฐ์์ get(), set()๋ถ๋ถ์ ๋ํด์ ์ถ๊ฐ์ ์ธ ๋ด์ฉ์๋ํด์๋ ์๋์ ์ ๋ฆฌํด ๋์๋ค.
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
                return if(onOff) "์๋ ๋๊ธฐ" else "์๋ ์ผ๊ธฐ"
            }

        fun makeDataForDB(): String {
            return "$hour:$minute"
        }
    }
    ```
    
  - AlarmReceiver : BroadcastReceiver()๋ฅผ ์์๋ฐ์ ์๋์๊ฐ์ ๋ง์ถ์ด notification์ ๋์์ค๋ค.  
    ๋ฑํ ์ด๋ ค์ด ๋ด์ฉ์ด์์ด์ ์ฝ๋๋ก ๋์ฒดํจ!
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
                    "๊ธฐ์ ์๋ฆผ",
                    NotificationManager.IMPORTANCE_HIGH
                )

                NotificationManagerCompat.from(context).createNotificationChannel(notificationChannel)
            }
        }

        private fun notifyNotification(context: Context){
            with(NotificationManagerCompat.from(context)){
                val build = NotificationCompat.Builder(context, NOTIFICATION_CHANNEL_ID)
                    .setContentTitle("์๋ฆผ")
                    .setContentText("์ผ์ด๋  ์๊ฐ์๋๋ค.")
                    .setSmallIcon(R.drawable.ic_launcher_foreground)
                    .setPriority(NotificationCompat.PRIORITY_HIGH)

                notify(NOTIFICATION_ID, build.build())
            }
        }
    }
    ``` 
  

๐ก PendingIntent ๋??[๐](https://www.charlezz.com/?p=861)  
+ ๊ฐ๋ต์ค๋ช  
  PendingIntent๋ Intent๋ฅผ ๊ฐ์ง๊ณ  ์๋ ํด๋์ค๋ก, ๊ธฐ๋ณธ ๋ชฉ์ ์ ๋ค๋ฅธ ์ ํ๋ฆฌ์ผ์ด์(๋ค๋ฅธ ํ๋ก์ธ์ค)์ ๊ถํ์ ํ์ฉํ์ฌ  
  ๊ฐ์ง๊ณ  ์๋ Intent๋ฅผ ๋ง์น ๋ณธ์ธ ์ฑ์ ํ๋ก์ธ์ค์์ ์คํํ๋ ๊ฒ ์ฒ๋ผ ์ฌ์ฉํ๊ฒ ํ๋ ๊ฒ์ด๋ค.  
+ ์ฉ๋  
  Notification์ผ๋ก ์์์ ์ํํ  ๋ ์ธํํธ๊ฐ ์คํ๋๋๋ก ํ๋ค.  
  Notification์ ์๋๋ก์ด๋ ์์คํ์ NotificationManager๊ฐ Intent๋ฅผ ์คํ์ํจ๋ค.  
  ์ฆ, ๋ค๋ฅธ ํ๋ก์ธ์ค์์ ์ํํ๊ธฐ ๋๋ฌธ์ Notification์ผ๋ก Intent ์ํ์ PendingIntent๋ฅผ ์ฌ์ฉํ๋๊ฒ์ ํ์์ ์ธ ๋ถ๋ถ์ด๋ค.  
  ```KOTLIN
  1. ๋ฐ์ฒ ๋ฐํํ๋ฉด์ ์์ ฏ์ผ๋ก Intent ์์์ ์ํํ  ๋  
  2. AlarmManager๋ฅผ ํตํด ์ง์ ๋ ์๊ฐ์ Intent ์์์ด ์์๋๋๋ก ํ  ๋
  ```
  + ๋ฐฉ๋ฒ
  PendingIntent๋ ์ปดํฌ๋ํธ์ ์ ํ์ ๋ฐ๋ผ ์์ฑ์ ๋ฉ์๋๋ฅผ ํธ์ถํ๋ ๋ฐฉ๋ฒ์ด ๋ค๋ฅด๋ค.  
  ```KOTLIN
  1. Activity๋ฅผ ์์ํ๋ Intent์ ๊ฒฝ์ฐ PendingIntent.getActivity()
  2. Service๋ฅผ ์์ํ๋ Intent์ ๊ฒฝ์ฐ PendingIntent.getService()
  3. BroadcastReceiver๋ฅผ ์์ํ๋ Intent์ ๊ฒฝ์ฐ PendingIntent.getBroadcast()
  ```
  
๐ก ์ฝํ๋ฆฐ get,set [๐](https://bbaktaeho-95.tistory.com/27), [๐](https://ddolcat.tistory.com/577), [๐](https://hongku.tistory.com/347)  
```KOTLIN
์๋ฐ์ ๊ฒฝ์ฐ getter์ setter๋ฅผ ์ง์  ๊ตฌํํด์ผ ํ๋ค. ๋คํ์ด๋ ์๋๋ก์ด๋ ์คํ๋์ค์์๋ ์๋ ์์ฑ์ ์ง์ํ๋ค.  
์ฝํ๋ฆฐ์ ๊ฒฝ์ฐ์๋ ์์ ๋ฒ๊ฑฐ๋ก์ด ์์์ ํ์ง ์์๋ ๋๋ค. ๋ณ์๋ฅผ ๋ง๋ค๋ฉด ์๋์ผ๋ก getter์ setter๋ฅผ ๋ด๋ถ์ ์ผ๋ก ์์ฑํด์ค๋ค.(๋์ ๋ณด์ด์ง ์์ ๋ฟ!!)  
์ฐธ๊ณ ๋ก var๋ก ์ ์ธํ ๋ณ์์ ๊ฒฝ์ฐ getter์ setter๋ฅผ ๋ชจ๋ ์์ฑํด ์ฃผ๋ฉฐ, val๋ก ์์ฑํ ๊ฒฝ์ฐ์๋ ๊ฐ์ ๋ณ๊ฒฝํ  ์ ์๊ธฐ ๋๋ฌธ์ getter๋ง ๋ด๋ถ์ ์ผ๋ก ์๋ ์์ฑ๋๋ค.

์ด ํ๋ก์ ํธ์ ๋ชจ๋ธ ํด๋ ์ค์์ get()์ ๋ฐ๋ก ์ฌ์ฉํ ์ด์ ๋ ๋ฐ์ดํฐ๋ฅผ ์ปค์คํํ์ฌ ๊ฐ์ ธ์ค๊ธฐ ์ํจ์ด๋ค. (์ปค์คํ ์ ๊ทผ์) ํด๋น๋ถ๋ถ๋ค์ ๋ํด์๋ ์์ ๋งํฌ์ ์์ธํ ์ค๋ช์ด ๋ด๊ฒจ์๋ค!
```
๐ก tag  
```KOTLIN
Tags can also be used to store data within a view without resorting to another data structure
```
๋ฐ์ดํฐ ์ ์ฅํ๋๋ฐ ํ์ฉํ๋ ๊ฒ ๊ฐ์๋ฐ, ์ด ํ๋ก์ ํธ์์๋ ON/OFF ๋ฒํผ ๊ด๋ จํด์ ๋ชจ๋ธํด๋์ค๋ฅผ ํตํด ๋ทฐ๋ฅผ ๋ ๋๋ง ํ  ๋ ์ฌ์ฉํ์๋ค. ์ถ๊ฐ์ ์ผ๋ก ์ ๋ณดํ์์ด ํ์ํจ! 

๐ก ์ฝํ๋ฆฐ ํ์์บ์คํ ์์๋ณด๊ธฐ
