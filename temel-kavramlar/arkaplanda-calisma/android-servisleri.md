# 🌠 Android Servisleri

## 👀 Servislere Genel Bakış

* 🚧 Android'de arkaplanda çalışmak için [Background Tasks](https://developer.android.com/guide/background) dokümanına bakılmalıdır
* 👮‍♂️ Android arka plan işlemlerini, [resmi dokümanında](https://developer.android.com/guide/background) da bataryayı korumak adına [buradaki](https://developer.android.com/about/versions/oreo/background.html) kısıtları uyguladığını belirtmektedir
  * Kullanıcıya arayüz \(UI\) sağlamayan her arkaplan işlemi \(Background Service\) kısıtlı sürede çalışır
  * Kısıtlanmayı engellemek adına [Foreground Service](https://developer.android.com/guide/components/services#Foreground) yapısı kullanılır
    * Kullanıcıya [kaldırılamayan bir bildirim](https://developer.android.com/guide/topics/ui/notifiers/notifications.html#foreground-service) gösterilir
    * Kullanıcı arkaplan işlemlerinden haberdar olur
  * Servisin birden fazla istekle baş etmesi gerekirse [IntentService](https://developer.android.com/guide/components/services#ExtendingIntentService) yerine [Service](https://developer.android.com/guide/components/services#ExtendingService) kullanılır
  * Servis tek bir işle baş edecek ise [IntentService](https://developer.android.com/guide/components/services#ExtendingIntentService) yapısı kullanılabilir
    * Çok fazla kısıtlamaya tabi tutulan
    * Tek bir istek için kullanılan
    * İşi bittiğinde kapanan bir sistemdir
* 🌑 Cihaz uyku moduna girdiğinde arka plan işlemleri aksamaya başlar, bundan dolayı [WakeLock](https://developer.android.com/training/scheduling/wakelock#java) özelliğinin aktif olması gerekir
* ⏳ Uzun süreli işlemler için hızı ve verimliliği artırma adına [multi-threading](https://developer.android.com/training/multiple-threads/) yapısı tercih edilmelidir



