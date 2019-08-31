# YAndroid
 
Mobil 📱 programlama notları

## Yakında

Bu alanda Android notları derlenecek

## Telefonu WiFi Üzerinden PC'ye Bağlama

- Windows için `cd %userprofile%\AppData\Local\Android\Sdk\platform-tools` komutu ile gerekli dizinine girmelisiniz

```sh
# Telefonu USB ile bağlayın
adb usb # USB moduna alır
adb devices # Cihazları listeler
adb tcpip <port> # Port açar
adb connect <IP>:<port> # IP'ye verilen açılan port ile bağlanma
adb devices # Bağlanıldığını kontrol etme

# ADB deamon işlemleri https://stackoverflow.com/a/52458945
which adb # Adb konumunu görme
locate adb
```

> IP değerini öğrenmek için `Ayarları - WiFi - Gelişmiş` kısmına bakabilirsiniz (ya da `adb shell netcfg`).

- Açıklamalara [buradan](http://codetheory.in/android-debug-bridge-adb-wireless-debugging-over-wi-fi/) erişebilirsin.
- Telefonu PC üzerinden yönetmek için [buraya](https://android.gadgethacks.com/how-to/fully-control-your-android-device-from-any-computer-0164097/) bakabilirsin

## Default App Hatası

Alttaki alan olmadığı sürece otomatik olarak belirlenmez.

```kt
<application>
    <intent-filter>
        <action android:name="android.intent.action.MAIN" />

        <category android:name="android.intent.category.LAUNCHER" />
    </intent-filter>
</application>
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

## Faydalı Bağlantılar

- [Uygulamaya diğer uygulamadan veri gönderme (Share)](https://developer.android.com/training/basics/intents/filters)
  - [Uygulama ile paylaş özelliği ekleme](https://blog.blundellapps.co.uk/add-your-app-to-the-android-menu/)
- [Youtube-dl Android](https://github.com/yausername/youtubedl-android)
- [İzin (permission) işlemleri](https://developer.android.com/training/permissions/requesting#kotlin)
- [How To Create, Start, Stop Android Background Service](https://www.dev2qa.com/how-to-create-start-stop-android-background-service/s)
