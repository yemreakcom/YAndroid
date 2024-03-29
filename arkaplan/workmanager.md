# 👷‍♂️ WorkManager \(👨‍🔬\)

## ❔ Hangi Koşullarda Kullanılmalı

* 🔄 Arka planda sunucu ile haberleşme işlemleri
* 📜 Sunucuya raporları göndermek
* 🕐 Ertelenebilir işlemler
* 💁‍♂️ Cihaz yeniden başlatılsa, uygulama kapansa bile devam etmesi gerekenler

## 🐥 Çalışma Durumları

| 🔸 İşlem | 🔙 Devreye girme süresi |
| :--- | :--- |
| 👨‍💼️ Görev yöneticisinden kapatılma | ⌚ Belli bir süre sonra |
| 🔁 Cihazı yeninden başlatma | 🕐 Cihaz yeniden başlatıldıktan sonra |
| 👮‍♂️ Uygulamayı zorla durdurma | ✖️ Uygulama yeniden açılınca |
| 🧹 Zorla cihazı yeniden başlatma | ❌ Uygulama yeniden açılınca |

{% hint style="info" %}
‍🧙‍♂ Detaylı bilgi için [Is WorkManager running when app is closed?](https://stackoverflow.com/questions/50682061/android-is-workmanager-running-when-app-is-closed) sorusuna bakabilirsin.
{% endhint %}

## 🕐 Periyodik Olarak Çalıştırma

* ⭐ 15 dk içerisinde, son 5 dakikalık süre içerisinde çalışır

```kotlin
workManager.enqueue(
			PeriodicWorkRequest.Builder(
				SyncCoWorker::class.java,
				15,
				TimeUnit.MINUTES,
				5,
				TimeUnit.MINUTES
			).build()
		)
```

![](../.gitbook/assets/workmanager_period.png)

## ❌ WorkManager Sonlandırma

```kotlin
WorkManager.getInstance().cancelAllWork()
```

## 🧐 Kaynaklar

* [📖 Schedule tasks with WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager)
* [📖 Getting started with WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager/basics)
* [👨‍🏫 Background Work with WorkManager - Kotlin](https://codelabs.developers.google.com/codelabs/android-workmanager-kt/#0)
* [👨‍🔬 Testing with WorkManager 2.1.0](https://developer.android.com/topic/libraries/architecture/workmanager/how-to/testing-210)
* [📃 Workout your tasks with WorkManager — Intro](https://proandroiddev.com/workout-your-tasks-with-workmanager-intro-db5aefe14d66)
* [📃 Workout your tasks with WorkManager — Main Components](https://proandroiddev.com/workout-your-tasks-with-workmanager-main-components-1c0c66317a3e)
* [📃 Workout your tasks with WorkManager — Advanced Topics](https://proandroiddev.com/workout-your-tasks-with-workmanager-advanced-topics-c469581c235b)

{% hint style="success" %}
🚀 Bu alandaki bağlantılar [YEmoji ~Bağlantılar](https://emoji.yemreak.com/kullanim/baglantilar) yapısına uygundur
{% endhint %}

