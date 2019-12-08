---
description: Android üzerinde uyguladığım kodlama stratejim
---

# ✨ Verimli Programlama

## 📝 Dokümantasyon Yapısı

* 🔗 İlgili metot üzerine yararlandığım kaynağın bağlantısını bırakmak

```java
/**
* ...
* https://developer.android.com/training/volley/simple.html
*/
public static void volleyExample() {
    // ...
}
```

## 👨‍💻 Kodlama Yapısı

* 💡 Gerekli adımları **Log** yapısı ve uygun etiket ile yazdırma
* ✨ Metotlar içerisinde özel **interface** yapısı kullanma
* 🗂️ Her sınıfın kullandığı veri yapısını kendi içerisinde tanımlama

```java
class NewsAPI {
    static final String TAG = "NewsAPI"; // Loglama için etiket yapısı
    
    public void logEx() {
        Log.i(TAG, "YEmreAk.com");
    }

    public static void someRequest(ResponseListener responseListener) {
        // İşlemler...
        // newDataList = ArrayList<NewsData>...
        responseListener.onResponse(newsDataList); // Interface kullanımı
    }
    
    interface ResponseListener {
            void onResponse(ArrayList<NewsData> newsDataList);
        }
    
        public static class NewsData {
            String title;
            String description;
            String urlToImage;
    
            NewsData(String title, String description, String urlToImage) {
                this.title = title;
                this.description = description;
                this.urlToImage = urlToImage;
            }
        }
}
```

