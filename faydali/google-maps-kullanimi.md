# 📍 Google Maps Kullanımı

## 🏗️ URL Oluşturma

* 🔗 URL ile google maps uygulamasına istekte bulunabilirsiniz
* 📍 Raptiye ile URL oluşturacaktır
* 🕵️‍♂️ `z` parametresi ile harita yakınlık derecesi belirlenebilir

```kotlin
val uri = "https://maps.google.com/?q=${lat},${lng}&ll=${lat},${lng}&z=${zoom}"
val intent = Intent(Intent.ACTION_VIEW, Uri.parse(uri))
intent.setPackage("com.google.android.apps.maps")
if (intent.resolveActivity(context.packageManager) != null)
   context.startActivity(intent)
```

| 💎 Parametre | 📝 Açıklama |
| :--- | :--- |
| `lat` | Latitude \(enlem\) |
| `lng` | Longitude \(boylam\) |
| `zoom` | Yakınlık değeri \[1-20\] arası |

{% hint style="info" %}
‍🧙‍♂ Detaylı bilgi için [URL Oluşturma ~ YLib](https://lib.yemreak.com/google/google-maps#url-yapisi) alanına bakabilirsin.
{% endhint %}

