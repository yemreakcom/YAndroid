---
description: Android üzerinde SQLite yerine üretilmiş yeni db formatı
---

# 💽 Room Database

## 💫 Synchronized ile DB'yi Koruma

* 👮‍♂️ Veri tabanına birden çok istek gelmesini engeller
* 🐞 Birden çok isteğin eş zamanlı yapılmaya çalışması **conflict** oluşturacaktır
* 💔 Conflict yapısı veri tabanındaki verilerin uyuşmazlığını belirtir
* 🚫 Birden fazla Thread gelmesi durumunda engellemek için **synchronized** anahtar kelimesi kullanılır
* ✨ Gereksiz Thread engelinden sakınmak için, synchronized yapısı içerisinde tekrardan **if kontrolü** yapılmalıdır

![](../.gitbook/assets/image%20%2822%29.png)

{% hint style="info" %}
👀 Detaylar için [Multi-threading](multithreading.md) alanına bakabilirsin.
{% endhint %}

