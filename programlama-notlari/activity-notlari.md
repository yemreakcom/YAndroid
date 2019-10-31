---
description: >-
  Android üzerinde her sayfa activity olarak adlandırılır, burada da onlar
  hakkında bilgilere yer verilecektir.
---

# 📃 Activity Notları

## 🚶‍♂️ Gecikmeli Activity Başlatma

```text
Handler().postDelayed({    startActivity(Intent(this, SnakeActivity1::class.java))
}, 400)
```

## 🌃 Arka planda Çalıştırma

```text
override fun onCreate(savedInstanceState: Bundle?) {
    // Arkaplanda çalıştırma
    moveTaskToBack(true)
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_share)

    ...
}
```

## 🧹 Bütün Eski Activity'leri Sonlandırıp Yeni Activity Açma

```text
val intent = Intent(this, MainActivity::class.java)
intent.flags = Intent.FLAG_ACTIVITY_NEW_TASK or Intent.FLAG_ACTIVITY_CLEAR_TASK // Tüm işlemleri bitirme
finish() // İşlemi sonlandırma
startActivity(intent)
```

