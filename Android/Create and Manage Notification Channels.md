# Create and Manage Notification Channels [Link](https://developer.android.com/training/notify-user/channels#CreateChannel)

## Developer side
- Android 8.0 Oreo(API 레벨 26)부터는 모든 알림을 채널에 할당해야 합니다. **(알림 채널을 하나 이상 구현해야 합니다.)**
- `targetSdkVersion`이 25이하로 설정된 경우에, 앱이 26 이상에서 실행될 때 25이하를 실행하는 기기들과 동일하게 작동합니다. 
- 채널마다 채널의 모든 알림에 적용되는 시각적/음향적 동작을 설정할 수 있습니다.
- 알림 채널을 만든 후에는 알림 동작을 변경할 수 없으며, 이 시점부터는 사용자가 모든 동작을 제어합니다. 
  **(단 채널의 이름 및 설명글은 변경 가능합니다.)**
- **주의**: 26이상을 타겟팅하는 경우 알림 채널을 지정하지 않고 알림을 게시하면 알림이 표시되지 않고 시스템에서 오류를 기록합니다.
  
 ## User side
- 사용자는 설정을 변경하고 앱에서 차단하거나 표시할 알림 채널을 결정할 수 있습니다.
- 사용자가 앱 설정에서 각 알림 채널들을 설정할 수 있습니다.
---

## 알림 채널 만들기
- 1. 고유한 채널 ID, 사용자가 볼 수 있는 이름, 중요도 수준을 사용해 `NotificationChannel` 객체를 생성합니다.
- 2. 선택적으로 `setDescription()`을 사용하여 앱 설정창에서 사용자에게 표시되는 설명을 지정합니다.
- 3. 생성한 알림 채널을 `createNotificationChannel()`에 전달하여 등록합니다.
- **주의**: `Notification Channel API`는 `Support Library`에서 사용할 수 없으므로 26이상에서만 실행되도록
  `SDK_INT` 버전에서 조건으로 코드를 보호해야 합니다.
```kotlin
if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
        // Create the NotificationChannel
        val name = getString(R.string.channel_name)
        val descriptionText = getString(R.string.channel_description)
        val importance = NotificationManager.IMPORTANCE_DEFAULT
        val mChannel = NotificationChannel(CHANNEL_ID, name, importance)
        mChannel.description = descriptionText
        // Register the channel with the system; you can't change the importance
        // or other notification behaviors after this
        val notificationManager = getSystemService(NOTIFICATION_SERVICE) as NotificationManager
        notificationManager.createNotificationChannel(mChannel)
    }
```
- 알림 채널은 1번만 만들면 되고, 앱을 시작할 때 해당코드를 호출하는 것이 안전합니다. **(ex: `Application Class`)**
- 채널의 알림 동작을 추가적으로 설정하려면 `NotificationChannel`인스턴스에 존재하는 다양한 속성 메소드를 사용합니다.
  **(`enableLights()`, `setLightColor()`, `setVibrationPattern()`)**
- 채널을 만든 후에는 따로 설정을 변경할 수 없으며, 사용자가 동작의 활성화 여부를 제어합니다.
- `CreateNotificationChannels()`를 이용하여 여러 알림 채널을 생성할 수 있습니다.
---

## 중요도 수준 설정
- 채널에 할당하는 중요도 수준은 채널에 게시하는 모든 알림 메시지에 적용됩니다.
- 중요도는 `NotificationChannel`생성자에서 지정해야 합니다.
- 25이하의 기기를 지원하려면 `NotificationCompat`클래스의 상수를 사용하여 `setPriority()`를 호출해야 합니다.

사용자가 볼 수 있는 중요도 수준 | 중요도(8.0이상) | 우선순위 상수(7.1이하)
-- | -- | --
긴급 (알림음이 울리며 헤드업 알림으로 표시됩니다.)| **IMPORTANCE_HIGH** | **PRIORITY_HIGH/PRIORITY_MAX**
높음 (알림음이 울립니다.)| **IMPORTANCE_DEFAULT** | **PRIORITY_DEFAULT**
중간 (알림음이 없습니다.)| **IMPORTANCE_LOW** | **PRIORITY_LOW**
낮음 (알림음이 없고 상태바에 표시되지 않습니다.)| **IMPORTANCE_MIN** | **PRIORITY_MIN**

- 중요도와 관계없이 모든 알림은 알림 창 및 사용자를 방해하지 않는 시스템 UI위치에 표시됩니다.
- 채널을 `NotificationManager`에 등록한 후에는 중요도 수준을 변경할 수 없습니다. 하지만 사용자는
  언제든지 앱 채널의 설정을 변경할 수 있습니다.
---

## 알림 채널 설정 읽기
- 사용자는 알림 채널의 설정을 수정할 수 있으며, 사용자가 알림 채널에 적용한 설정을 확인할 수 있습니다.
- 1. `getNotificationChannel()`또는 `getNotificationChannels()`를 호출하여 `NotificationChannel`객체를 가져옵니다.
- 2. `getVibrationPattern()`, `getSound()`, `getImportance()`와 같은 특정 채널 설정을 쿼리합니다.
- 앱의 의도된 동작을 금지한다고 생각하는 채널 설정을 감지한 경우 사용자에게 채널 설정을 변경할 것을 제안하고,
  채널 설정을 열기 위한 작업을 제공할 수 있습니다.
---

## 알림 채널 설정 열기
- 사용자가 알림 설정에 쉽게 액세스할 수 있게 알림 채널의 설정으로 리다이렉션 할 수 있습니다.
- `ACTION_CHANNEL_NOTIFICATION_SETTINGS` 작업을 사용하는 `Intent`로 알림 채널의 설정 화면을 열 수 있습니다.
```kotlin
val intent = Intent(Settings.ACTION_CHANNEL_NOTIFICATION_SETTINGS).apply {
    putExtra(Settings.EXTRA_APP_PACKAGE, packageName)
    putExtra(Settings.EXTRA_CHANNEL_ID, myNotificationChannel.getId())
}
startActivity(intent)
```
- `intent`에는 앱의 패키지 이름과 수정할 채널ID를 지정하는 두 가지 추가 기능이 필요합니다.
---

## 알림 채널 삭제
- `deleteNoficationChannel()`을 호출하여 알림 채널을 삭제할 수 있습니다.
```kotlin
val notificationManager = getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager
val id: String = "my_channel_01"
notificationManager.deleteNotificationChannel(id)
```
---

## 알림 채널 그룹 만들기
- 필요에 따라 채널을 각 그룹으로 구분해 사용자가 알림 채널을 쉽게 구별할 수 있습니다.
- 알림 채널 그룹마다 고유한 그룹 ID, 사용자가 볼 수 있는 이름이 필요합니다.
```kotlin
// The id of the group.
val groupId = "my_group_01"
// The user-visible name of the group.
val groupName = getString(R.string.group_name)
val notificationManager = getSystemService(Contenxt.NOTIFICATION_SERVICE) as NotificationManager
notificationManager.createNotificationChannelGroup(NotificationChannelGroup(groupId, groupName))
```
- 새 그룹을 만든 후 `setGroup()`을 호출하여 새 `NotificationChannel` 객체를 그룹에 연결할 수 있습니다.
- 채널을 등록한 후에는 알림 채널과 그룹 간의 연결을 변경할 수 없습니다.
