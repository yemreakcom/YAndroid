# Activity Notları <!-- omit in toc -->

Android üzerinde her sayfa activity olarak adlandırılır, burada da onlar hakkında bilgilere yer verilecektir.

## İçerikler <!-- omit in toc -->

- [Gecikmeli Activity Başlatma](#Gecikmeli-Activity-Ba%C5%9Flatma)
- [Arkaplanda Çalıştırma](#Arkaplanda-%C3%87al%C4%B1%C5%9Ft%C4%B1rma)
- [Bütün Eski Activity'leri Sonlandırıp Yeni Activity Açma](#B%C3%BCt%C3%BCn-Eski-Activityleri-Sonland%C4%B1r%C4%B1p-Yeni-Activity-A%C3%A7ma)

## Gecikmeli Activity Başlatma

```kt
Handler().postDelayed({    startActivity(Intent(this, SnakeActivity1::class.java))
}, 400)
```

## Arkaplanda Çalıştırma

```kt
override fun onCreate(savedInstanceState: Bundle?) {
    // Arkaplanda çalıştırma
    moveTaskToBack(true)
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_share)

    ...
}
```

## Bütün Eski Activity'leri Sonlandırıp Yeni Activity Açma

```kt
val intent = Intent(this, MainActivity::class.java)
intent.flags = Intent.FLAG_ACTIVITY_NEW_TASK or Intent.FLAG_ACTIVITY_CLEAR_TASK // Tüm işlemleri bitirme
finish() // İşlemi sonlandırma
startActivity(intent)
```
