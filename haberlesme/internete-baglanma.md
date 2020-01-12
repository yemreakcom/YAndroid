# 🌍 İnternete Bağlanma

## ✍ El Notlarım

![](../.gitbook/assets/connect_internet_hand.png)

## 👮‍♂️ Gerekli İzinlerin Alınması

* 📃 `AndroidManifest.xml` dosyası üzerinden internet izni alınmalıdır
* 🐣`<uses-permission android:name="android.permission.INTERNET" />` ile internet erişimi 
* 🔸 `<uses-permission   android:name="android.permission.ACCESS_NETWORK_STATE" />` ile internet bağlantısı durumu izni alınır

{% hint style="info" %}
‍🧙‍♂ Detaylı bilgi için  [Including permissions in the manifest](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-3-working-in-the-background/lesson-7-background-tasks/7-2-c-internet-connection/7-2-c-internet-connection.html#permissions) dokümanına bakabilirsin
{% endhint %}

## 👨‍💼 Bağlantı Durumunu Yönetme

* 🧰 Tüm sistem servislerine `getSystemService` metodu ile erişilir
* 📶 `ConnectivityManager` ve `NerworkInfo` sınıflarından bağlantı bilgileri yönetilir

```java
private static final String DEBUG_TAG = "NetworkStatusExample";

ConnectivityManager connMgr = 
           (ConnectivityManager) getSystemService(Context.CONNECTIVITY_SERVICE);
           
NetworkInfo networkInfo = connMgr.getNetworkInfo(ConnectivityManager.TYPE_WIFI);
boolean isWifiConn = networkInfo.isConnected();

networkInfo = connMgr.getNetworkInfo(ConnectivityManager.TYPE_MOBILE);
boolean isMobileConn = networkInfo.isConnected();

Log.d(DEBUG_TAG, "Wifi connected: " + isWifiConn);
Log.d(DEBUG_TAG, "Mobile connected: " + isMobileConn);
```

{% hint style="info" %}
‍🧙‍♂ Detaylı bilgi için  [Managing the network state](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-3-working-in-the-background/lesson-7-background-tasks/7-2-c-internet-connection/7-2-c-internet-connection.html#manage_state) dokümanına bakabilirsin.
{% endhint %}

{% hint style="warning" %}
📢 API 29 ve sonrası için eskimiştir, güncel haline [activeNetworkInfo.type is deprecated in API level 28](https://stackoverflow.com/questions/53532406/activenetworkinfo-type-is-deprecated-in-api-level-28) alanından bakabilirsin
{% endhint %}

## ❣️ Dikkat Etmen Gerekenler

* 🕐 Bağlantı işlemleri uzun sürebilir
* 🚫 UI Thread üzerinden yapılmamalıdır, aksi halde uygulamayı engelleyebilir
* 💫 Bağlantı işlemleri [Asenkron İşlemler](../arkaplan/asynctask-ve-asynctaskloader/) yazısına göre yapılmalıdır

## 👮‍♂️ Güvenlik Notları

* 🐣 Veri tabanına direkt erişim **olmamalı**, API üzerinden erişim olmalı
* 👨‍💻 Reverse Engineering ile bağlantıda kullandığın bilgileri elde edebilirler

## 🔗 Faydalı Kaynaklar

{% embed url="https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-3-working-in-the-background/lesson-7-background-tasks/7-2-c-internet-connection/7-2-c-internet-connection.html" %}

