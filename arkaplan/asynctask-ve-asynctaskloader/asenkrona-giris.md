# 🌃 Asenkrona Giriş

## 👀 Asenkron İşlemleri Tanıyalım

* 💫 Ayrı bir Thread üzerinden gerçekleşen bu işlemleri sistemin ilerlemesi engellemez
* 🙋‍♂️ İşleri tamamlandığı zaman UI Thread'e dahil olurlar
* ⭐ **AsyncTask** veya **AsycnTaskLoader** yapıları kullanılır

## 🏷️ Etiketleme ile Asenkron

* 👷‍♂️ `@WorkerThread` gibi etiketlerle asenkron çalışması gereken metotlar tanımlanır
* 🦸‍♂️ Kod hakimiyetini artırmak için tercih edilir

{% hint style="info" %}
‍🧙‍♂ Detaylı bilgi için [Thread annotations](https://developer.android.com/studio/write/annotations#thread-annotations) alanına bakabilirsin.
{% endhint %}

## 🆚 İkisi Arasındaki Temel Farklar

Her ikisi de sistemi bloklamadan çalışan bir yapıya sahiptir

| `AsyncTask` | `AsyncTaskLoader` |
| :--- | :--- |
| Direkt olan çalışır | Dolaylı olarak çalışır |
| Yapılandırma ayarları değiştiğinde iptal olur ve yeniden başlatılır | Yapılandırma ayarlarından etkilenmez |
| Geri dönüş vermeyecek işlemlerde kullanılır | Geri dönüşümlü işlemlerde kullanılır |
| Kısa ve iptal edilebilir işlemlerde tercih edilir | Uzun ve iptal edilemeyecek işlemlerde tercih edilir |

> Telefonu döndürme gibi işlemler yapılandırma ayarlarını değiştirir.

{% hint style="success" %}
Genel olarak `AsyncTaskLoader` en sık kullanılan yapıdır.
{% endhint %}

## 🧱 UI Thread

Android'teki tüm görüntü işlemlerinin yapıldı alandır.

* UI Thread engellenmemeli
* UI Thread sadece görsel işlemler için kullanılmalıdır
* Tüm işlemler 16ms'den kısa bir sürede tamamlanmalıdır

![](../../.gitbook/assets/async_task_ui_thread.png)

{% hint style="danger" %}
Yaklaşık olarak 5s'den uzun süren işlemler  "[application not responding](http://developer.android.com/guide/practices/responsiveness.html)" \(ANR\) diyaloğunu oluşturur ve kullanıcı bunu görmesi durumunda uygulamayı kapatıp, siler 😥
{% endhint %}

## 🔗 Harici Bağlantılar

{% embed url="https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-3-working-in-the-background/lesson-7-background-tasks/7-1-c-asynctask-and-asynctaskloader/7-1-c-asynctask-and-asynctaskloader.html" %}

