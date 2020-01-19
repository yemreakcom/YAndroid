# 🔰 Giriş \| Notification

## 📦 Gerekli Paketleri Dahil Etme

* 👮‍♂️ Bildirimler için alttaki paketin `build.gradle` içerisine dahil edilmesi gerekir

```groovy
implementation "com.android.support:support-compat:28.0.0"
```

## 👀 Bildirimlere Bakış

* 👮‍♂️ Android 8.0 sonrasında `Notification Channel` zorunluluğu gelmiştir
* 🆔 Channel ID verilmesi zorunlu hale getirilmiştir \(eskilerde zorunlu değil\)

### ⭐ Notification Channel

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
@RequiresApi(Build.VERSION_CODES.O)
private fun createNotificationChannel() {
	// Bildirim kanalını özelliklerini oluşturma
	val name = getString(R.string.notification_name)
	val description = getString(R.string.notification_description)
	val importance = NotificationManager.IMPORTANCE_HIGH
	
	// Bildirim kanalını oluşturma
	val channel = NotificationChannel(CHANNEL_ID, name, importance).apply {
		this.description = description
		lightColor = Color.BLUE
		lockscreenVisibility = Notification.VISIBILITY_PRIVATE
	}

	// Bildirimi sisteme kaydetme
	(getSystemService(Context.NOTIFICATION_SERVICE) as NotificationManager)
			.apply {
				createNotificationChannel(channel)
			}
}
```
{% endtab %}

{% tab title="Java" %}
```java
@RequiresApi(Build.VERSION_CODES.O)
private String createNotificationChannel() {
    String channelId = "telemetry";
    NotificationChannel channel = 
        new NotificationChannel(
            channelId, "Telemetry Service", 
            NotificationManager.IMPORTANCE_DEFAULT
    );
    
    channel.setLightColor(Color.BLUE);
    channel.setLockscreenVisibility(Notification.VISIBILITY_PRIVATE);
    
    NotificationManager notificationManager = 
    (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
    
    if (notificationManager != null) {
        notificationManager.createNotificationChannel(channel);
    }

    return channelId;
}
```
{% endtab %}
{% endtabs %}

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
var builder = NotificationCompat.Builder(this, CHANNEL_ID)
        .setSmallIcon(R.drawable.notification_icon)
        .setContentTitle(textTitle)
        .setContentText(textContent)
        .setPriority(NotificationCompat.PRIORITY_DEFAULT)
```
{% endtab %}

{% tab title="Java" %}
```java
NotificationCompat.Builder builder = 
        new NotificationCompat.Builder(this, CHANNEL_ID)
        .setSmallIcon(R.drawable.notification_icon)
        .setContentTitle(textTitle)
        .setContentText(textContent)
        .setPriority(NotificationCompat.PRIORITY_DEFAULT);
```
{% endtab %}
{% endtabs %}

![](../../.gitbook/assets/noti_content.png)



