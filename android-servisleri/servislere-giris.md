# 🔰 Servislere Giriş

## ✍ Yazılı Notlarım <a id="servislere-genel-bakis"></a>

![](../.gitbook/assets/services_hand.png)

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

## 💫 Yaşam Döngüsü

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
class ExampleService : Service() {
    private var startMode: Int = 0             // indicates how to behave if the service is killed
    private var binder: IBinder? = null        // interface for clients that bind
    private var allowRebind: Boolean = false   // indicates whether onRebind should be used

    override fun onCreate() {
        // The service is being created
    }

    override fun onStartCommand(intent: Intent?, flags: Int, startId: Int): Int {
        // The service is starting, due to a call to startService()
        return mStartMode
    }

    override fun onBind(intent: Intent): IBinder? {
        // A client is binding to the service with bindService()
        return mBinder
    }

    override fun onUnbind(intent: Intent): Boolean {
        // All clients have unbound with unbindService()
        return mAllowRebind
    }

    override fun onRebind(intent: Intent) {
        // A client is binding to the service with bindService(),
        // after onUnbind() has already been called
    }

    override fun onDestroy() {
        // The service is no longer used and is being destroyed
    }
}
```
{% endtab %}

{% tab title="Java" %}
```java
public class ExampleService extends Service {
    int startMode;       // indicates how to behave if the service is killed
    IBinder binder;      // interface for clients that bind
    boolean allowRebind; // indicates whether onRebind should be used

    @Override
    public void onCreate() {
        // The service is being created
    }
    @Override
    public int onStartCommand(Intent intent, int flags, int startId) {
        // The service is starting, due to a call to startService()
        return mStartMode;
    }
    @Override
    public IBinder onBind(Intent intent) {
        // A client is binding to the service with bindService()
        return mBinder;
    }
    @Override
    public boolean onUnbind(Intent intent) {
        // All clients have unbound with unbindService()
        return mAllowRebind;
    }
    @Override
    public void onRebind(Intent intent) {
        // A client is binding to the service with bindService(),
        // after onUnbind() has already been called
    }
    @Override
    public void onDestroy() {
        // The service is no longer used and is being destroyed
    }
}
```
{% endtab %}
{% endtabs %}

