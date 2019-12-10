---
description: Karşılaştığım hatalar hakkında bulduğum çözümler
---

# 🐛 Hata Notları

## 🐞 Default App Hatası

Alttaki alan olmadığı sürece otomatik olarak belirlenmez.

```text
<application>
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</application>
```

