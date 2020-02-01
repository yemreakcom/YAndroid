# 👷‍♂️ WorkManager

## 🚴‍♂️ WorkManager'a Giriş

* 🔄 Arka planda sunucu ile haberleşme işlemleri
* 📜 Sunucuya raporları göndermek
* 🕐 Ertelenebilir işlemler
* 💁‍♂️ Cihaz yeniden başlatılsa, uygulama kapansa bile devam eder

## 🕐 Periyodik Olarak Çalıştırma

```text
workManager.enqueue(
			PeriodicWorkRequest.Builder(
				SyncCoWorker::class.java,
				15,
				TimeUnit.MINUTES,
				15,
				TimeUnit.MINUTES
			).build()
		)
```

![](../.gitbook/assets/workmanager_period.png)

## 🧐 Kaynaklar

* [📖 Schedule tasks with WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager)
* [🏂 Getting started with WorkManager](https://developer.android.com/topic/libraries/architecture/workmanager/basics)
* [👨‍🏫Background Work with WorkManager - Kotlin](https://codelabs.developers.google.com/codelabs/android-workmanager-kt/#0)
* [👨‍🔬 Testing with WorkManager 2.1.0](https://developer.android.com/topic/libraries/architecture/workmanager/how-to/testing-210)

