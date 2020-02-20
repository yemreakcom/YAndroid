---
description: Google Play Store ile bilmemiz gerekenler
---

# 👜 Google Play Store

## 🚙 Google Play Store'a Uygulamayı Aktarma

Uygulamalarınızı google play store'a yüklemek için **developer** hesabı açmanı gerekmektedir.

* Google tek seferlik **25$**'lık bir ücret almaktadır
  * Hesabınızı kapatmanız durumunda bu ücret **iade** edilecektir
  * Hesabınızdaki uygulamaları başka hesaplara aktarmak için [buraya](https://play.google.com/apps/publish/?account=6847951054083969806#AppTransferPlace) bakabilirsin
  * Detaylı bilgi için [buraya](https://support.appmachine.com/hc/en-us/articles/218378068-Transfer-your-app-from-one-Google-Play-developer-account-to-another) bakabilirsin.
* Uygulama satışlarının **%30**'u _Google_'a gitmektedir

## 🛰️ Yayınlamadan Önce

* 👀 Yayınlama işlemlerinden önce [📖 Publish your app](https://developer.android.com/studio/publish) yazısından yapman gerekenleri okumalısın
* ✨ Uygulama sürümünü `build.gradle (app)` içerisinde `versionCode` ve `versionName` alanlarını artırarak yenilemelisin. [📖 Version your app](https://developer.android.com/studio/publish/versioning)
* 👀 Son olarak [📖 App Sign In](https://developer.android.com/studio/publish/app-signing) alanında gözden geçirmek bir kaç detay var
* [🚀 Upload your app to the Play Console](https://developer.android.com/studio/publish/upload-bundle) ile play store'a aktarabilirsin

{% hint style="info" %}
‍🧙‍♂ Detaylı bilgi için [Faydalı Bağlantılar](google-play-store.md#faydali-baglantilar) alanına bakabilirsin.
{% endhint %}

{% hint style="warning" %}
📢 Eğer key bilgini kaybedersen, [Developer Console - App singing](https://play.google.com/apps/publish/?account=8950082947306358822#KeyManagementPlace:p=com.yemreak.depremya&appid=4975744681878245790) üzerinden Google'a mail atabilirsin.
{% endhint %}

## 🦶 Uygulamayı Sıkıştırma

* ➕ `build.gradle` \(app\) dosyasına alttaki `release` yapılandırmasını ekleyin
* 💦 Gereksiz kodları temizleyecektir
* 🗃️ Kaynakları sıkıştıracaktır

```groovy
release {
   minifyEnabled true
   shrinkResources true
   // proguardFiles ...
}
```

## 🔗 Faydalı Bağlantılar

* [📖 Publish your app](https://developer.android.com/studio/publish)
* [📖 Version your app](https://developer.android.com/studio/publish/versioning)
* [📖 App Sign In](https://developer.android.com/studio/publish/app-signing)
* [📖 Upload your app to the Play Console](https://developer.android.com/studio/publish/upload-bundle)

{% hint style="success" %}
🚀 Bu alandaki bağlantılar [YEmoji ~Bağlantılar](https://emoji.yemreak.com/kullanim/baglantilar) yapısına uygundur
{% endhint %}

