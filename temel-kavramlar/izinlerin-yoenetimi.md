---
description: Android üzerinde izin (permission) yapısı
---

# 👮‍♂️ İzinlerin Yönetimi

## 📑 Manifest'e Kaydetme

`AndroidManifest.xml` dosyasına `<uses-permission` satırı eklenir.

```markup
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
          package="com.example.snazzyapp">

    <uses-permission android:name="android.permission.SEND_SMS"/>

    <application ...>
        ...
    </application>
</manifest>
```

{% hint style="info" %}
🧙‍♂️ Detaylı bilgi [Request App Permission ](https://developer.android.com/training/permissions/requesting)alanında ele alınmaktadır
{% endhint %}

## 🙏 İzin İsteğinde Bulunma

Alttaki kodlar herhangi bir Activity içerisinde yazılmalıdır

```java
// thisActivity içerisinde olduğumuz varsayılmıştır
if (ContextCompat.checkSelfPermission(this,
        Manifest.permission.READ_CONTACTS)
        != PackageManager.PERMISSION_GRANTED) {

    // İzin alınmamışsa
    // Açıklama yapılmak isteniyorsa yapalım
    if (ActivityCompat.shouldShowRequestPermissionRationale(this,
            Manifest.permission.READ_CONTACTS)) {
        // Kullanıcı cevabını beklerken UI thread'i bloklamamak için
        // asenkron olarak işlemleri sürdürmeliyiz
        // Ardından da izin istenmelidir
    } else {
        // Açıklmaya gerek yoksa direkt izin isteme
        ActivityCompat.requestPermissions(this,
                new String[]{Manifest.permission.READ_CONTACTS},
                IZIN_KODU);

        // IZIN_KODU değişkeninin herhangi bir değer ile tanımlanması gerekir
        // Bir sonraki aşamada kullanılacaktır
    }
} else {
    // İzin zaten verilmiş
}
```

## 🤝 İsteği Kontrol Etme

* İstenilen izni aynı class içerisinde `onRequestPermissionResult` metodunu override ederek yönetiriz
* `requestCode` değişkeni `IZIN_KODU`'na eşit olması durumunda izin kontrolü yapılır

```java
@Override
public void onRequestPermissionsResult(
    int requestCode,
    @NonNull String[] permissions, 
    @NonNull int[] grantResults
) {
    if (requestCode == IZIN_KODU) {
        // Eğer sonucun uzunluğu 0 ise kullanıcı başka bir yere basarak iptal
        // etmiştir.
        if (
            grantResults.length > 0 &&
            grantResults[0] == PackageManager.PERMISSION_GRANTED
        ) {
            // İzin alındı 🚀
            startTelemetryService();
        }
    } else {
        // İzin alınamadı 😔
    }
}
```

