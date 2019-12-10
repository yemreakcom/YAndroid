---
description: "Bu yazı bilgisayar �� ve \U0001F4F1 telefon bağlantılarını ele alır."
---

# 📲 Telefonu Bilgisayara Bağlama

## 📶 Telefonu WiFi Üzerinden PC'ye Bağlama

ADB ile Telefonu PC'ye Bağlama işlemi olarak da geçmektedir.

* `adb` komutlarını kullanmak için [Android SDK](https://developer.android.com/studio) içerisinde olan platform-tools'a sahip olmanız gerekmektedir
  * **Command line tools only** alanından sadece platform-tools'u indirebilirsiniz
* Windows için `cd %userprofile%\AppData\Local\Android\Sdk\platform-tools` komutu ile gerekli dizinine girmelisiniz

**👨‍💻 Terminal \(cmd / bash\) üzerinden alttaki komutları sırasıya yazın:**

```bash
# Telefonu USB ile bağlayın
adb usb # USB moduna alır
adb devices # Cihazları listeler
adb tcpip <port> # Port açar
adb connect <IP>:<port> # IP'ye verilen açılan port ile bağlanma
adb devices # Bağlanıldığını kontrol etme
```

> IP değerini öğrenmek için `Ayarları - WiFi - Gelişmiş` kısmına bakabilirsiniz \(ya da `adb shell netcfg`\).

**İsteğe bağlı komutlar:**

```bash
# ADB deamon işlemleri https://stackoverflow.com/a/52458945
which adb # Adb konumunu görme
locate adb
```

## 🎮 Telefonunu PC'den Kontrol Etme

* Chrome eklentisi olan [Vysor](http://www.vysor.io/) ile bu işlemi yapabilirsin
* Nasıl yapacağına dair açıklamalara [buradan](http://codetheory.in/android-debug-bridge-adb-wireless-debugging-over-wi-fi/) erişebilirsin

