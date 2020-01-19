---
description: Android üzerinde bildirim oluşturma
---

# 🏗️ Oluşturma \| Notification

## 👀 Bildirimlere Bakış

* 👮‍♂️ Android 8.0 sonrasında `Notification Channel` zorunluluğu gelmiştir
* 🆔 Channel ID verilmesi zorunlu hale getirilmiştir \(eskilerde zorunlu değil\)
* ☝ Bildirimlere tıklandığında uygulamanızı açması önemlidir

## 📦 Paketleri Dahil Etme

* 👮‍♂️ Bildirimler için alttaki paketin `build.gradle` içerisine dahil edilmesi gerekir

```groovy
implementation "com.android.support:support-compat:28.0.0"
```

## 📢 Notification Channel

* 👨‍💼 Bildirimlerin kategorilerle  yönetildiği yapıdır
* 👪 Kategorilere göre bildirim şekillerini düzenlemeye yardımcı olur
* 🚀 Her bildirimin farklı biz özelliği vardır ve ayrıca yönetilir
* 😏 Kullanıcı sadece kendisini rahatsız eden bildirimleri kapatır ve sizin diğer bildirimleriniz devam eder

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

## 🔔 Bildirim Oluşturma

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
private fun createNotification(): Notification {
   if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
      createNotificationChannel()
   }

   return NotificationCompat.Builder(this, CHANNEL_ID)
         .setSmallIcon(R.drawable.ic_notification)
         .setContentTitle(getString(R.string.notification_title))
         .setStyle(NotificationCompat.BigTextStyle()
               .bigText(getString(R.string.notification_text)))
         .setPriority(NotificationCompat.PRIORITY_MIN)
         .setCategory(NotificationCompat.CATEGORY_SERVICE)
         .setAutoCancel(true)
         .build()
}
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

## ✨ Uygulamayı Gösteren Bildirim

* ➕ `.setContentIntent(createShowAppPI())` olarak `NotificationCompat.Builder` üzerine eklenmelidir

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
private fun createShowAppPI(): PendingIntent {
   val intent = Intent(this, MainActivity::class.java).apply {
      flags = Intent.FLAG_ACTIVITY_NEW_TASK or Intent.FLAG_ACTIVITY_CLEAR_TASK
   }

   return PendingIntent.getActivity(
         this@TelemetryService,
         PI_SHOW_APP,
         intent,
         PendingIntent.FLAG_UPDATE_CURRENT)
}

private fun createNotification(): Notification {
		if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
			createNotificationChannel()
		}
		
		return NotificationCompat.Builder(this, CHANNEL_ID)
				.setSmallIcon(R.drawable.ic_notification)
				.setContentTitle(getString(R.string.notification_title))
				.setStyle(NotificationCompat.BigTextStyle()
						.bigText(getString(R.string.notification_text)))
				.setPriority(NotificationCompat.PRIORITY_MIN)
				.setCategory(NotificationCompat.CATEGORY_SERVICE)
				.setContentIntent(createShowAppPI()) // Yeni eklenen alan
				.setAutoCancel(true)
				.build()
}
```
{% endtab %}
{% endtabs %}

## 🚫 Bildirim Üzerinden İptal Etme

* ➕ `.addAction(R.drawable.ic_notification, "Kapat", createCloseServicePI())` olarak `NotificationCompat.Builder` üzerine eklenmelidir

```groovy
private const val REQUEST_STOP = 2

@SuppressLint("NewApi")
private fun createCloseServicePI(): PendingIntent {
	val stopSelfIntent = Intent(this, TelemetryService::class.java).apply {
		action = ACTION_STOP_SERVICE
	}

	return when (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
		true ->
			PendingIntent.getForegroundService(
					this,
					REQUEST_STOP,
					stopSelfIntent,
					PendingIntent.FLAG_CANCEL_CURRENT
			)
		false ->
			PendingIntent.getService(
					this,
					REQUEST_STOP,
					stopSelfIntent,
					PendingIntent.FLAG_CANCEL_CURRENT
			)
	}
}

private fun createNotification(): Notification {
		if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.O) {
			createNotificationChannel()
		}

		return NotificationCompat.Builder(this, CHANNEL_ID)
				.setSmallIcon(R.drawable.ic_notification)
				.setContentTitle(getString(R.string.notification_title))
				.setStyle(NotificationCompat.BigTextStyle()
						.bigText(getString(R.string.notification_text)))
				.setPriority(NotificationCompat.PRIORITY_MIN)
				.setCategory(NotificationCompat.CATEGORY_SERVICE)
				.setAutoCancel(true)
				// Yeni gelen alan 👇
				.addAction(R.drawable.ic_notification, "Kapat", createCloseServicePI())
				.build()
	}
```



