---
description: Android üzerinde Global Variable kavramı ve oluşturulması
---

# 🌍 Global Değişkenler

## 🔰 Neden kullanılır?

* 🎳 Activity arası yüksek boyutlu veri taşımak maliyetlidir, bu yapı ile statik olarak saklanır
* 💫 Her class tarafından değişkenler aktarılmadan, direkt buradan kullanılır
* 👮‍♂️ Güvenli bir yapı olduğundan, sorunsuz çalışmayı sağlar
* ✨ Hızlı ve kullanışlıdır

## ⭐ Global Variable Örneği

```java
import androidx.annotation.NonNull;

class Globals {

    private News selectedNews;

    private static Globals INSTANCE;

    private Globals() {
    }

    static synchronized Globals getInstance() {
        if (INSTANCE == null) {
            INSTANCE = new Globals();
        }
        return INSTANCE;
    }

    void setSelectedNews(@NonNull News news) {
        selectedNews = news;
    }

    @NonNull
    News getSelectedNews() throws NullPointerException {
        if (selectedNews == null) {
            throw new NullPointerException("The selected news invoked without creation");
        }
        return selectedNews;
    }
}
```

## 🔗 Faydalı Kaynaklar

{% embed url="https://androidresearch.wordpress.com/2012/03/22/defining-global-variables-in-android/" %}

