---
description: Arkaplan işlemleri için kullanılan yapıları
---

# 🔂 AsyncTask ve AsyncTaskLoader

## 🆚 İkisi Arasındaki Temel Farklar

| `AsyncTask` | `AsyncTaskLoader` |
| :--- | :--- |
| Direkt olan çalışır | Dolaylı olarak çalışır |
| Sistemi bloklar | Sistemden bağımsız çalışır |
| Geri dönüş vermeyecek işlemlerde kullanılır | Geri dönüşümlü işlemlerde kullanılır |
| Kısa ve iptal edilebilir işlemlerde tercih edilir | Uzun ve iptal edilemeyecek işlemlerde tercih edilir |

{% hint style="success" %}
Genel olarak `AsyncTaskLoader` en sık kullanılan yapıdır.
{% endhint %}

## 🧱 UI Thread

Android'teki tüm görüntü işlemlerinin yapıldı alandır.

* UI Thread engellenmemeli
* UI Thread sadece görsel işlemler için kullanılmalıdır
* Tüm işlemler 16ms'den kısa bir sürede tamamlanmalıdır

![](../.gitbook/assets/image%20%2818%29.png)

{% hint style="danger" %}
Yaklaşık olarak 5s'den uzun süren işlemler  "[application not responding](http://developer.android.com/guide/practices/responsiveness.html)" \(ANR\) diyaloğunu oluşturur ve kullanıcı bunu görmesi durumunda uygulamayı kapatıp, siler 😥
{% endhint %}

## 🔁 AsyncTask

{% tabs %}
{% tab title="🎈 Kullanım" %}
![](../.gitbook/assets/image%20%2812%29.png)

![](../.gitbook/assets/image%20%289%29.png)

| 💠 Metot | 📜 Açıklama |
| :--- | :--- |
| `onPreExecute()` | İşlem tamamlanmadan önce ara ara çağrılan metottur, genellikle % dolum bilgisi vermek için kullanılır |
| `doInBackground(Params...)` | `onPreExecute()` metodu bittiği an çalışır, arkaplan işlemlerini yapan kısımdır. `publishProgress()` metodu ile değişikleri UI Thread'e aktarır. Bittiğinde `onPostExecure()` metoduna sonucu aktarır. |
| `onProgressUpdate(Progress...)` | `publishProgress()` metodundan sonra çalışır, genellikle raporlama veye ilerleme adımlarını kullanıcıya göstermek için kullanılır |
| `onPostExecute(Result)` | Arkaplan işlemi tamamlandığında sonuç buraya aktarılır, UI Thread bu metot üzerinden sonucu kullanır. |

{% hint style="warning" %}
`onProgressUpdate` metodunda tüm adımları ele alırsanız, asenkron çalışma yapısı bozulur ve senkronize olarak çalışır
{% endhint %}
{% endtab %}

{% tab title="🧱 Prototip" %}
```java
public class MyAsyncTask extends AsyncTask <String, Void, Bitmap>{}
```

* `String` değişkeni, `doInBackground` metoduna aktarılacak verilerdir
* `Void` yapısı, `publishProgress` ve `onProgressUpdate` metotlarının kullanılmayacağını belirtir
* `Bitmap` tipi de, `onPostExecute` ile aktarılan işlem sonucunun tipini belirtir

![](../.gitbook/assets/image%20%287%29.png)

{% hint style="warning" %}
Son iki parametre \(`Void` ve `Bitmap`\) dışarıdan verilmez, sınıf içi parametrelerdir
{% endhint %}
{% endtab %}

{% tab title="👨‍💻 Kod" %}
```java
private class DownloadFilesTask extends AsyncTask<URL, Integer, Long> {
     protected Long doInBackground(URL... urls) {
         int count = urls.length;
         long totalSize = 0;
         for (int i = 0; i < count; i++) {
             totalSize += Downloader.downloadFile(urls[i]);
             publishProgress((int) ((i / (float) count) * 100));
             // Escape early if cancel() is called
             if (isCancelled()) break;
         }
         return totalSize;
     }

     protected void onProgressUpdate(Integer... progress) {
         // 0. eleman en son adımı belirtir. (FIFO)
         setProgressPercent(progress[0]);
     }

     protected void onPostExecute(Long result) {
         showDialog("Downloaded " + result + " bytes");
     }
 }
 
 // UI Thread'te kullanımı
 new DownloadFilesTask().execute(url1, url2, url3);
```
{% endtab %}
{% endtabs %}



