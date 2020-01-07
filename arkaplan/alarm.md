# ⏰ Alarm

## 👀 Alarm Yapısına Bakış

* ⏰ Belirli sürelerde tetiklenen `Intent` işlemleridir
* 🙇‍♂️ Uygulama kapalı olsa hatta telefon uykuda olsa bile çalışır
* 📈 Arka plan işlemlerinin tekrarlanma sıklığını azalttığından verimliliği artırır
* 👨‍💼 `AlarmManager` üzerinden, `Intent-Filter` yapısı gibi yönetilir

> 🙄 Telefondaki alarmdan bahsetmiyorum.

{% hint style="info" %}
‍🧙‍♂ Detaylı bilgi için [Introduction ~ 8.2 Alarm](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-3-working-in-the-background/lesson-8-alarms-and-schedulers/8-2-c-alarms/8-2-c-alarms.html#chapterstart) alanına bakabilirsin.
{% endhint %}

## 👮‍♂️ Kullanmaman Gereken Durumlar

* 🚩 Uygulaman üzerinde çalışacak olan eylemlerde kullanılmaz
* ✨ [`Handler`](https://developer.android.com/reference/android/os/Handler.html) , [`Timer`](https://developer.android.com/reference/java/util/Timer.html) veya[`Thread`](https://developer.android.com/reference/java/lang/Thread.html) yapısı tercih edilmelidir
* 🔄 Sunucu ile güncelleme işlemlerini bu yapı ile yapmayın
  * 🔥 [Firebase Cloud Messaging](https://firebase.google.com/docs/cloud-messaging/) üzerindeki [`SyncAdapter`](https://developer.android.com/training/sync-adapters/creating-sync-adapter.html) yapısı ile yapılır
* ⌚ Beklemeli işlemler için [`JobScheduler`](https://developer.android.com/reference/android/app/job/JobScheduler.html) yapısını tercih edin
  * 📶 Wi-Fi bağlandığında haberleri veya hava durumunu güncelleme gibi

## 🔸 Alarm Türleri

## 🧱 Temel İşlemler

### 🏗️ Alarm Kurma

* ✨ Dakikada çok fazla tekrarlanacak işlemler için  [`Handler`](https://developer.android.com/reference/android/os/Handler.html) yapısını tercih edin
* 🐣 [`getSystemService(ALARM_SERVICE)`](https://developer.android.com/reference/android/content/Context.html#ALARM_SERVICE) metodu ile [`AlarmManager`](https://developer.android.com/reference/android/app/AlarmManager.html) sınıfı alınır
* ✔️ Alarm türünü belirleyin ve `set...(<tip>, <süre>, <PendingIntent>)` metodu ile tanımlayın

```java
alarmMgr.set(AlarmManager.ELAPSED_REALTIME, 
             SystemClock.elapsedRealtime() + 1000*300,
             alarmIntent);
```

{% hint style="info" %}
‍🧙‍♂ Detaylı bilgi için  [Scheduling an alarm](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-3-working-in-the-background/lesson-8-alarms-and-schedulers/8-2-c-alarms/8-2-c-alarms.html#scheduling) alanına veya [GitHub Koduna](https://github.com/google-developer-training/android-fundamentals-apps-v2/blob/master/StandUp/app/src/main/java/com/android/fundamentals/standup/MainActivity.java#L91) bakabilirsin.
{% endhint %}

### ⏲ Tekrarlı Alarmlar

```java
alarmMgr.setInexactRepeating(AlarmManager.RTC_WAKEUP,
               calendar.getTimeInMillis(),
               AlarmManager.INTERVAL_FIFTEEN_MINUTES,
               alarmIntent);
```

### 👨‍💼 Olan Alarmı Kontrol Etme

```java
boolean alarmExists = 
 (PendingIntent.getBroadcast(this, 0, 
       alarmIntent,
       PendingIntent.FLAG_NO_CREATE) != null);
```

### ☠️ Alarmı Öldürme

```java
alarmManager.cancel(alarmIntent);
```

## 🙇‍♂️ WakeUp \(Uyandırma\)

## 👁️ Görülebilir Alarmlar

* ⏰ Kullanıcıya gösterilen alarm türleridir
* 🕐 `AlarmClock` yapısı olarak ele alınır
* 🙇‍♂️ Genellikle uyandırma çağrıları için kullanılır

## 🔗 Faydalı Bağlantılar

{% embed url="https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-3-working-in-the-background/lesson-8-alarms-and-schedulers/8-2-c-alarms/8-2-c-alarms.html" %}

{% embed url="https://codelabs.developers.google.com/codelabs/android-training-alarm-manager/\#0" %}

