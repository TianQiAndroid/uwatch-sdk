# UWatch BLE SDK 使用说明文档
概述
UWatchBleManager 是一个智能手表蓝牙通信管理类，基于 BLE（蓝牙低功耗）技术实现与手表的双向通信。本 SDK 提供了设备控制、数据同步、消息推送等完整功能。
快速开始
# 1. 初始化与连接状态监听
kotlin
// 继承 AbstractBleManager 实现设备连接逻辑
class MyBleManager : AbstractBleManager() {
    // 实现连接、断开等基础方法
}
// 初始化
UWatchBleManager.init
// 注册蓝牙连接状态
UWatchBleManager.registerOnConnectionStateChangeLintener
// 扫描设备
use UWatchBLEScanner to scan UWatch device

# 2. 指令响应监听器
kotlin
// 注册监听器
UWatchBleManager.registerOnDeviceResponseListener
功能分类
# 一、设备属性类指令
# 1.1 时间同步
kotlin
// 同步手机时间到手表
UWatchBleManager.syncTime()
# 1.2 电量查询
kotlin
// 查询手表电量
UWatchBleManager.getBattery()
// 响应数据：byte[0] 为电量百分比 (0-100)
# 1.3 屏幕设置
kotlin
// 查询屏幕亮度
UWatchBleManager.getScreenBrightness()

// 设置屏幕亮度 (0-100)
UWatchBleManager.setScreenBrightness(80)
# 1.4 语言设置
kotlin
// 查询当前语言
UWatchBleManager.getLanguage()

// 设置语言（自动使用系统语言）
UWatchBleManager.setUserLanguage()

// 获取支持的语言列表
UWatchBleManager.getDeviceSupportLanguage()
# 1.5 单位设置
kotlin
// 设备单位（公制/英制）
UWatchBleManager.getDeviceUnit()
UWatchBleManager.setDeviceUnit(DeviceUnitData(DeviceUnitData.DeviceUnit.METRIC))

// 天气单位（摄氏度/华氏度）
UWatchBleManager.getWeatherUnit()
UWatchBleManager.setWeatherUnit(WeatherUnitData(WeatherUnitData.WeatherUnit.CELSIUS))

// 时间格式（12小时/24小时）
UWatchBleManager.getTimeUnit()
UWatchBleManager.setTimeUnit(TimeUnitData(TimeUnitData.TimeUnit.HOUR_24))
# 1.6 勿扰模式
kotlin
// 查询勿扰设置
UWatchBleManager.getDoNotDisturb()

// 设置勿扰模式
val doNotDisturbData = DoNotDisturbData(
    switch = true,
    startHour = 22,
    startMinute = 0,
    endHour = 7,
    endMinute = 0
)
UWatchBleManager.setDoNotDisturb(doNotDisturbData)
# 1.7 设备信息
kotlin
// 获取手表类型
UWatchBleManager.getDeviceWatchType()

// 获取设备序列号
UWatchBleManager.getDeviceSerialNo()

// 获取固件版本
UWatchBleManager.getDeviceFirmwareVersion()

// 上报手机类型（安卓）
UWatchBleManager.reportPhoneType()
1.8 其他功能
kotlin
// 寻找设备（手表会震动或响铃）
UWatchBleManager.startFindDevice()
UWatchBleManager.stopFindDevice()

// 恢复出厂设置
UWatchBleManager.restoreFactorySettings()

// 设置亮屏时长（秒）
UWatchBleManager.setScreenTime(30)
# 二、个人信息管理
# 2.1 个人信息
kotlin
// 获取个人信息
UWatchBleManager.getPersonalInfo()

// 设置个人信息
val personalInfoData = PersonalInfoData(
    sex = PersonalInfoData.Sex.MALE,
    age = 30,
    height = 175,
    weight = 70
)
UWatchBleManager.setPersonalInfo(personalInfoData)
# 三、状态控制类
# 3.1 设备开关设置
kotlin
// 获取设备开关状态
UWatchBleManager.getDeviceSwitchSetting()

// 设置设备开关
val switchData = DeviceSwitchData(
    goalAchievementSwitch = true,           // 目标达成提醒
    regularSportsDataUploadSwitch = true,   // 规律运动数据上传
    messageReminderSwitch = true,           // 消息提醒
    sleepMonitorSwitch = true,              // 睡眠监测
    autoSyncSwitch = true,                  // 自动同步
    raiseHandBrightenScreenSwitch = true,   // 抬腕亮屏
    antiLostSwitch = true,                  // 防丢失
    vibrationSwitch = true,                 // 震动开关
    soundSwitch = true                      // 声音开关
)
UWatchBleManager.setDeviceSwitchSetting(switchData)
# 3.2 设备绑定
kotlin
// 开始绑定设备
UWatchBleManager.startBindDevice()

// 绑定完成
UWatchBleManager.bindDeviceComplete()

// 解除绑定
UWatchBleManager.unBindDevice()
# 四、闹钟管理
# 4.1 闹钟设置
kotlin
// 获取最大闹钟数量
UWatchBleManager.getMaxAlarmCount()

// 获取闹钟列表
UWatchBleManager.getAlarmData()

// 获取第一个未使用的闹钟索引
UWatchBleManager.getFirstUnusedAlarmIndex()

// 添加闹钟
val alarmData = AlarmData(
    alarmIndex = 0,                     // 闹钟索引
    switch = true,                      // 开关
    alarmCycle = booleanArrayOf(        // 重复周期 [周一,周二,...,周日]
        false, true, true, true, true, true, false
    ),
    alarmHour = 7,                      // 小时
    alarmMinute = 30,                   // 分钟
    vibrationMode = 1,                  // 震动模式
    reminderLater = 0                   // 稍后提醒
)
UWatchBleManager.addAlarm(alarmData)

// 编辑闹钟
UWatchBleManager.editAlarm(alarmData)

// 删除单个闹钟
UWatchBleManager.deleteAlarm(0)

// 删除所有闹钟
UWatchBleManager.deleteAllAlarms()
# 4.2 周期间隔提醒
kotlin
// 获取周期间隔设置
UWatchBleManager.getIntervalReminder(IntervalReminderData.EventType.DRINK)

// 设置周期间隔提醒
val intervalData = IntervalReminderData(
    eventType = IntervalReminderData.EventType.DRINK,
    switch = true,
    weeks = booleanArrayOf(true, true, true, true, true, false, false),
    startHour = 8,
    startMinute = 0,
    endHour = 22,
    endMinute = 0,
    period = 2  // 间隔小时数
)
UWatchBleManager.setIntervalReminder(intervalData)
# 五、消息推送类
# 5.1 普通消息推送
kotlin
// 推送消息到手表
val messageData = MessageData(
    messageType = MessageData.MessageType.WECHAT,
    message = "您有一条新消息"
)
UWatchBleManager.pushMessage(messageData)

// 支持的消息类型：
// MessageData.MessageType.SMS         短信
// MessageData.MessageType.WECHAT      微信
// MessageData.MessageType.QQ          QQ
// MessageData.MessageType.FACEBOOK    Facebook
// MessageData.MessageType.TWITTER     Twitter
// MessageData.MessageType.WHATSAPP    WhatsApp
// MessageData.MessageType.OTHER       其他
# 5.2 天气推送
kotlin
// 推送单日天气
val weatherData = WeatherPushData(
    weatherTimeType = WeatherPushData.WeatherTimeType.TODAY,
    weatherType = WeatherPushData.WeatherType.SUNNY,
    temp = 25,
    lowestTemp = 18,
    highestTemp = 28
)
UWatchBleManager.pushWeather(weatherData, false)

// 推送多日天气
val weatherList = mutableListOf<WeatherPushData>()
weatherList.add(todayWeather)
weatherList.add(tomorrowWeather)
weatherList.add(afterTomorrowWeather)
UWatchBleManager.pushWeather(weatherList, true)  // true表示同步天气
# 六、联系人管理
# 6.1 联系人操作
kotlin
// 获取联系人数量
UWatchBleManager.getContactCount()

// 获取可添加的联系人索引
UWatchBleManager.getAddContactIndex()

// 获取联系人列表
UWatchBleManager.getContactList()

// 添加联系人
val contactData = ContactPushData(
    contactIndex = 0,
    contactName = "张三",
    contactPhone = "13800138000"
)
UWatchBleManager.addContact(contactData)

// 删除联系人
UWatchBleManager.deleteContact(contactData)

// 删除所有联系人
UWatchBleManager.deleteAllContact()
# 6.2 来电静音
kotlin
// 获取来电静音状态
UWatchBleManager.getInComingMute()

// 设置来电静音
UWatchBleManager.requestInComingMute(true)  // true:静音，false:正常
# 七、健康数据类
# 7.1 运动数据
kotlin
// 获取运动历史数据
UWatchBleManager.getSportsHistory()

// 删除运动历史数据
UWatchBleManager.deleteSportsHistory()

// 获取计步数据
UWatchBleManager.getToadyStepCaloriesAndDistanceMeasureData()
UWatchBleManager.getStepHistoryData()
# 7.2 睡眠数据
kotlin
// 获取今日睡眠数据
UWatchBleManager.getTodaySleepData()

// 获取睡眠历史数据
UWatchBleManager.getSleepHistoryData()

// 删除睡眠数据
UWatchBleManager.deleteSleepHistory()

// 设置自动睡眠监测时间
val sleepMonitorData = AutoSleepMonitorTimeData(
    startHour = 23,
    startMinute = 0,
    endHour = 7,
    endMinute = 0,
    allDayMonitor = false
)
UWatchBleManager.setAutoSleepMonitorTime(sleepMonitorData)
# 7.3 心率监测
kotlin
// 获取心率数据
UWatchBleManager.getHeartRateData()

// 删除心率数据
UWatchBleManager.deleteHeartRateData()

// 设置自动心率监测间隔
UWatchBleManager.setAutoHeartRateMonitorInterval(IntervalData(interval = 30))

// 设置心率报警范围
val heartRateAlarmData = HeartRateAlarmRangeData(
    switch = true,
    upperLimit = 120,
    lowestLimit = 50
)
UWatchBleManager.setHeartRateAlarmRange(heartRateAlarmData)
# 7.4 实时测量
kotlin
// 获取实时测量开关状态
UWatchBleManager.getRealtimeMeasureReportSwitch(MeasureType.HEART_RATE)

// 设置实时测量开关
val measureSwitchData = RealtimeMeasureReportSwitchData(
    measureType = MeasureType.HEART_RATE,
    open = true
)
UWatchBleManager.setRealtimeMeasureReportSwitch(measureSwitchData)

// 开始实时测量
UWatchBleManager.startRealtimeMeasure(MeasureType.HEART_RATE)

// 设置自动测量间隔
val autoMeasureData = AutoMeasureIntervalData(
    measureType = MeasureType.HEART_RATE,
    autoMeasureInterval = 60  // 分钟
)
UWatchBleManager.setAutoMeasureInterval(autoMeasureData)
7.5 久坐提醒
kotlin
// 获取久坐提醒设置
UWatchBleManager.getLongSitReminderSetting()

// 设置久坐提醒
val longSitData = LongSitReminderData(
    interval = 60,      // 提醒间隔（分钟）
    startHour = 9,
    startMinute = 0,
    endHour = 18,
    endMinute = 0
)
UWatchBleManager.setLongSitReminder(longSitData)
# 八、表盘管理
# 8.1 表盘推送
kotlin
// 推送表盘数据
UWatchDialPushManager.startDialPush

# 8.2 壁纸推送
kotlin
// 推送壁纸数据
UWatchWallpaperPushManager.startWallpaperPush

# 8.3 二维码设置
kotlin
// 设置支付二维码（微信/支付宝）
UWatchBleManager.setQrcodeText(
    isQrcodeTypeWechat = true,  // true:微信，false:支付宝
    qrcodeText = "支付二维码内容"
)
# 九、其他功能
# 9.1 远程拍照
kotlin
// 开启远程拍照（手表控制手机拍照）
UWatchBleManager.startRemotePhoto()

// 关闭远程拍照
UWatchBleManager.stopRemotePhoto()
# 9.2 计时运动数据
kotlin
// 获取计时运动数据总数
UWatchBleManager.getTimedSportsDataTotalCount()

// 获取所有计时运动数据
UWatchBleManager.getTimedSportsData()

// 删除计时运动数据
UWatchBleManager.deleteTimedSportsData()

# 九、其他
use HealthDataUtil to calculate distance or calories

how to get dial list
use POST to request https://u-watch.com.cn/api/app/ota/otaType?height=${screenHeight}&width=${screenWidth}
data class DialOtaData(
        val ota_type: MutableList<DialOtaTypeStyle>?,
        val ota_style: MutableList<DialOtaTypeStyle>?

)
data class DialOtaTypeStyle(
            val dictType: String?,
            val dictLabel: String?,
            val dictValue: String?
)

use POST to request https://u-watch.com.cn/api/app/ota/v3/list with form body:
pageSize = xxx
pageNum = xxx // start from 0
width = screenWidth
height = screenHeight
type = ota_type[i].dictValue
style = ota_style[i].dictValue
}
data class DialListResponse(
    val total: Int,
    val rows: List<DialItem>,
    val code: Int,
    val msg: String?
)
data class DialItem(
    val dialBinUrl: String?,
    val previewImageUrl: String?,
)
