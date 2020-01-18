# 🔰 Servislere Giriş

## ✍ Yazılı Notlarım <a id="servislere-genel-bakis"></a>

![](../../.gitbook/assets/services_hand.png)

## 👀 Servislere Genel Bakış <a id="servislere-genel-bakis"></a>

* 🤔 Arkaplanda çalışan arayüzü olmayan Activity'ler olarak adlandırılabilir
* 🚧 Android'de arkaplanda çalışmak için [Background Tasks](https://developer.android.com/guide/background) dokümanına bakılmalıdır
* ⏳ Uzun süreli işlemler için hızı ve verimliliği artırma adına [multi-threading](https://developer.android.com/training/multiple-threads/) yapısı önerilir

## 🔋 Android Pil Koruması Önlemleri

* 👮‍♂️ Android arka plan işlemlerini bataryayı korumak adına kısıtlar.
* 🚧 Kullanıcıya arayüz \(UI\) sağlamayan her arkaplan işlemi \([Background Service](https://developer.android.com/guide/components/services)\) kısıtlı sürede çalışır
* 🌞 Kısıtlanmayı engellemek adına [Foreground Service](https://app.gitbook.com/@yemreak/s/android-yemreak/~/drafts/-LuP8elEQ8QcAhafEg9U/temel-kavramlar/arkaplanda-calisma/foreground-service) yapısı kullanılmalıdır
  * 🔔 Kullanıcıya [kaldırılamayan bir bildirim](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#foreground-service) gösterilir
  * 👁‍🗨 Kullanıcı arkaplan işlemlerinden haberdar olur
* 🌙 Cihaz uyku moduna girdiğinde arka plan işlemleri aksamaya başlar.
  * 🙇‍♂️ [WakeLock](https://developer.android.com/training/scheduling/wakelock#java) özelliğinin aktif olması gerekir

{% hint style="info" %}
🧙‍♂️ Detaylı bilgi için [Challenges in background processing](https://developer.android.com/guide/background#challenges_in_background_processing) alanına bakabilirsin.
{% endhint %}

{% page-ref page="foreground-service.md" %}

## ✅ İstek Türüne Göre Servis Seçimi

* 🦄 Servis tek bir işle baş edecek ise [IntentService](https://developer.android.com/guide/components/services#ExtendingIntentService) \(eski adı [JobIntentService](https://developer.android.com/reference/android/support/v4/app/JobIntentService)\) yapısı kullanılabilir
  * 👮‍♂️ Çok fazla kısıtlamaya tabi tutulan
  * ❌ İşi bittiğinde kapanan bir sistemdir
* 👪 Servisin birden fazla istekle baş etmesi gerekirse [IntentService](https://developer.android.com/guide/components/services#ExtendingIntentService) yerine [Service](https://developer.android.com/guide/components/services#ExtendingService) kullanılır

## 📢 Servis Tanımlama

```markup
<manifest ... >
  ...
  <application ... >
      <service android:name="ExampleService"
               android:exported="false" />
      ...
  </application>
</manifest>
```

