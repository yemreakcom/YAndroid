---
description: Android üzerinde bildirim verme işlemleri ve yönetimi
---

# 🔔 Notification

## 🎈 Notification Nedir

![](../.gitbook/assets/notification_hand.png)

![](../.gitbook/assets/notification_channel.png)

## 👨‍💼 Bildirimleri Yönetme

![](../.gitbook/assets/notification_manager.png)

## 🕊️ Pending Intent

![](../.gitbook/assets/pending_intent.png)

## 🎳 Geniş Bildirimler

![](../.gitbook/assets/expandable_notification.png)

## 💎 Bildirim Özellikleri

![](../.gitbook/assets/notification_types.png)

## 👨‍💻 Kod Örneği

```java
@RequiresApi(Build.VERSION_CODES.O)
private String createNotificationChannel() {
    String channelId = "telemetry";
    NotificationChannel channel = new NotificationChannel(channelId, "Telemetry Service", NotificationManager.IMPORTANCE_DEFAULT);
    channel.setLightColor(Color.BLUE);
    channel.setLockscreenVisibility(Notification.VISIBILITY_PRIVATE);
    NotificationManager notificationManager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
    if (notificationManager != null) {
        notificationManager.createNotificationChannel(channel);
    }

    return channelId;
}
```

