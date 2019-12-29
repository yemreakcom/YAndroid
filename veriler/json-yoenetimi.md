---
description: JSON verilerini ayrıştırma ve parsing işlemi
---

# 📜 JSON Yönetimi

## 🔰 Nedir?

* 🌍 Web sayfalarında saklanan veri formatıdır
* 👁️ JavaScript Object Notation

## ⭐ Örnek JSON Verisi

```yaml
{"menu": {
  "id": "file",
  "value": "File",
  "popup": {
    "menuitem": [
      {"value": "New", "onclick": "CreateNewDoc()"},
      {"value": "Open", "onclick": "OpenDoc()"},
      {"value": "Close", "onclick": "CloseDoc()"}
    ]
  }
}
```

## 🪁 Ayrıştırma İşlemi

```java
JSONObject data = new JSONObject(responseString);
JSONArray menuItemArray = data.getJSONArray("menuitem");
JSONObject thirdItem = menuItemArray.getJSONObject(2);
String onClick = thirdItem.getString("onclick");
```

## 🔗 Faydalı Bağlantılar

{% embed url="https://stackoverflow.com/a/9606629/9770490" %}

