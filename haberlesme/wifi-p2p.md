---
description: Destekleyen cihazlar için android WiFi P2P bağlantısı
---

# 📶 WiFi P2P \(Direct\)

## 🏗️ WiFi P2P Activity Oluşturma

### 👨‍💻 WiFi P2P Sınıfını Kodlama 

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
class WifiDirectActivity : AppCompatActivity() {

    /**
     * Konum izni isteme kodu
     */
    private val PRC_ACCESS_FINE_LOCATION = 1

    /**
     * Wifi alıcısı için filtreleme
     */
    private val wifiFilter = IntentFilter().apply {
        addAction(WifiP2pManager.WIFI_P2P_STATE_CHANGED_ACTION)
        addAction(WifiP2pManager.WIFI_P2P_PEERS_CHANGED_ACTION)
        addAction(WifiP2pManager.WIFI_P2P_CONNECTION_CHANGED_ACTION)
        addAction(WifiP2pManager.WIFI_P2P_THIS_DEVICE_CHANGED_ACTION)
    }

    /**
     * WiFi değişikliklerinde reciever'ı çalıştırma
     */
    private lateinit var manager: WifiP2pManager

    /**
     * Wi-Fi P2P Frameworkü ile uygulamamıza bağlanmayı sağlayacak obje
     */
    private lateinit var channel: WifiP2pManager.Channel

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_wi_fi_direct)

        initWifiP2p()
    }

    private fun initWifiP2p(): Unit {
        manager = getSystemService(Context.WIFI_P2P_SERVICE) as WifiP2pManager
        channel = manager.initialize(this, mainLooper, null)

        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            // getWifiP2pPermissions()
        }
    }
    // ...
}
```
{% endtab %}

{% tab title="Java" %}
```java
public class WiFiDirectActivity extends AppCompatActivity {

    public static final String TAG = WiFiDirectActivity.class.getSimpleName();
    private static final int PRC_ACCES_FINE_LOCATION = 1;
    private final IntentFilter wifiFilter = new IntentFilter();
    WifiP2pManager manager;
    Channel channel;
    BroadcastReceiver wifiReceiver;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_wifi_direct);

        initWifiFilter();
        initDependences();
    }

    /**
     * Wifi alıcısına aktarılacak broadcast türlerini belirleme
     */
    private void initWifiFilter() {
        wifiFilter.addAction(WifiP2pManager.WIFI_P2P_STATE_CHANGED_ACTION);
        wifiFilter.addAction(WifiP2pManager.WIFI_P2P_PEERS_CHANGED_ACTION);
        wifiFilter.addAction(WifiP2pManager.WIFI_P2P_CONNECTION_CHANGED_ACTION);
        wifiFilter.addAction(WifiP2pManager.WIFI_P2P_THIS_DEVICE_CHANGED_ACTION);
    }

    private void initDependences() {
        // WiFi değişikliklerinde reciever'ı çalıştırma
        manager = (WifiP2pManager) getSystemService(Context.WIFI_P2P_SERVICE);
        Objects.requireNonNull(manager);

        // Wi-Fi P2P Frameworkü ile uygulamamıza bağlanmayı sağlayacak obje
        channel = manager.initialize(this, getMainLooper(), null);
        
        // İzinleri alma alanı bir alt aşamadadır
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            getRequiredPermissions();
        }
    }
    
    // ...
}
```
{% endtab %}
{% endtabs %}

### 💎 WiFi P2P Durumları

| 🧐 Intent Filter | 🕊️ Tetiklenme Sebebi |
| :--- | :--- |
| [`WIFI_P2P_CONNECTION_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_CONNECTION_CHANGED_ACTION) | Cihazın WiFi bağlantısının durumu değişikliği |
| [`WIFI_P2P_PEERS_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_PEERS_CHANGED_ACTION) | Bağlanacak cihazları [`discoverPeers()`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#discoverPeers%28android.net.wifi.p2p.WifiP2pManager.Channel,%20android.net.wifi.p2p.WifiP2pManager.ActionListener%29) metodu ile keşfetmek istediğimiz zaman tetiklenir. Genellikle  [`requestPeers()`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#requestPeers%28android.net.wifi.p2p.WifiP2pManager.Channel,%20android.net.wifi.p2p.WifiP2pManager.PeerListListener%29) metodu ile eşleşen cihazları alma amaçlı kullanılır. |
| [`WIFI_P2P_STATE_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_STATE_CHANGED_ACTION) | WiFi P2P'in aktifliğinin değişmesi \(açık / kapalı\) |
| [`WIFI_P2P_THIS_DEVICE_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_THIS_DEVICE_CHANGED_ACTION) | Cihaz detaylarının \(örn: ismi\) değişmesi |

## 👮‍♂️ Gerekli İzinlerin Alınması

### 📝 Manifest Dosyasına İzinleri Kaydetme

Manifest dosyasının içeriğine aşağıdaki kodu ekleyin

{% code title="AndroidManifest.xml" %}
```markup
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.example.android.nsdchat"
    ...
    
    <!-- Wi-Fi Direct (P2P) bağlatısı için eklendi -->
    <uses-permission android:name="android.permission.ACCESS_WIFI_STATE" />
    <uses-permission android:name="android.permission.CHANGE_WIFI_STATE" />
    <uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
    <uses-permission android:name="android.permission.INTERNET" />
    ...
```
{% endcode %}

### 🧰 API için Ek Olarak Gereken İzinler

Aşağıdaki metotlar **Location Mode \(Konum Hizmeti\)** özelliğinin aktif olmasına da ihtiyaç duyar

* [`discoverPeers`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#discoverPeers%28android.net.wifi.p2p.WifiP2pManager.Channel,%20android.net.wifi.p2p.WifiP2pManager.ActionListener%29)
* [`discoverServices`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager#discoverServices%28android.net.wifi.p2p.WifiP2pManager.Channel,%2520android.net.wifi.p2p.WifiP2pManager.ActionListener%29)
* [`requestPeers`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager#requestPeers%28android.net.wifi.p2p.WifiP2pManager.Channel,%2520android.net.wifi.p2p.WifiP2pManager.PeerListListener%29)

### 👨‍💻 Kod Tarafında İzin İsteme

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
val PRC_ACCESS_FINE_LOCATION = 1

@RequiresApi(Build.VERSION_CODES.M)
fun getWifiP2pPermissions(): Unit {
    Manifest.permission.ACCESS_FINE_LOCATION.let {
        if (!hasPermission(it)) {
            requestPermissions(arrayOf(it), PRC_ACCESS_FINE_LOCATION)
        }
    }
}

@RequiresApi(Build.VERSION_CODES.M)
fun hasPermission(permission: String): Boolean {
    return checkSelfPermission(permission) == PackageManager.PERMISSION_GRANTED
}

override fun onRequestPermissionsResult(
    requestCode: Int,
    permissions: Array<out String>,
    grantResults: IntArray
) {
    when (requestCode) {
        PRC_ACCESS_FINE_LOCATION -> {
            if (grantResults[0] != PackageManager.PERMISSION_GRANTED) {
                Log.w(TAG, "onRequestPermissionsResult: Konum izni gereklidir")
                Toast.makeText(this, "İzinler gereklidir 😥", Toast.LENGTH_SHORT)
                    .show()
            }
        }
    }
}
```
{% endtab %}

{% tab title="Java" %}
```java
/**
 * Wi-Fi P2P için gerekli izinleri alma
 */
@RequiresApi(api = Build.VERSION_CODES.M)
private void getRequiredPermissions() {
    if (!hasPermission(Manifest.permission.ACCESS_FINE_LOCATION)) {
        requestPermissions(
                new String[]{Manifest.permission.ACCESS_FINE_LOCATION},
                WiFiDirectActivity.PRC_ACCES_FINE_LOCATION
        );
    }
}

@RequiresApi(api = Build.VERSION_CODES.M)
public boolean hasPermission(String permission) {
    return checkSelfPermission(permission) == PackageManager.PERMISSION_GRANTED;
}

@Override
public void onRequestPermissionsResult(
        int requestCode,
        @NonNull String[] permissions,
        @NonNull int[] grantResults
) {
    if (requestCode == PRC_ACCES_FINE_LOCATION) {
        if (grantResults[0] != PackageManager.PERMISSION_GRANTED) {
            Log.w(TAG, "onRequestPermissionsResult:Fine location izni gereklidir");
            Toast.makeText(this, "İzinler gereklidir 😥", Toast.LENGTH_SHORT)
                .show();
        }
    }
}
```
{% endtab %}
{% endtabs %}

{% page-ref page="../temel/izinlerin-yoenetimi.md" %}

## 📻 WiFi için Broadcast Receiver Tanımlama

### 📡 Broadcast Alıcısı Tanımlama

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
open class WifiDirectBroadcastReceiver(
    var manager: WifiP2pManager,
    var channel: WifiP2pManager.Channel,
    var wifiDirectActivity: WifiDirectActivity
) : BroadcastReceiver() {

    companion object {
        val TAG = WifiDirectActivity::javaClass.name
    }

    override fun onReceive(context: Context, intent: Intent) {
        when (intent.action) {
            WifiP2pManager.WIFI_P2P_STATE_CHANGED_ACTION -> onStateChanged()
            WifiP2pManager.WIFI_P2P_PEERS_CHANGED_ACTION -> onPeerChanged()
            WifiP2pManager.WIFI_P2P_CONNECTION_CHANGED_ACTION -> 
                onConnectionChanged()
            WifiP2pManager.WIFI_P2P_THIS_DEVICE_CHANGED_ACTION -> 
                onThisDeviceChanged()
        }
    }

    private fun onStateChanged(): Unit {
        Log.i(TAG, "onStateChanged: ")
    }

    private fun onPeerChanged(): Unit {
        Log.i(TAG, "onPeerChanged: ")
    }

    private fun onConnectionChanged(): Unit {
        Log.i(TAG, "onConnectionChanged: ")
    }

    private fun onThisDeviceChanged(): Unit {
        Log.i(TAG, "onThisDeviceChanged: ")
    }
}
```
{% endtab %}

{% tab title="Java" %}
```java
/**
 * Önemli WiFi olaylarını yayınlayan sınıf
 * https://developer.android.com/guide/topics/connectivity/wifip2p.html#create-br
 */
public class WiFiDirectBroadcastReciever extends BroadcastReceiver {

    public static final String TAG = WiFiDirectBroadcastReciever
        .class.getSimpleName();
    public static final int reconnect = 1;
    private static final String DEVICE_PATTERN = "HUAWEI P20 lite";

    WifiP2pManager manager;
    Channel channel;
    WiFiDirectActivity wifiDirectActivity;

    /**
     * Eşleşilen cihazların bilgileri
     */
    private List<WifiP2pDevice> peers = new ArrayList<>();

    public WiFiDirectBroadcastReciever(
        WifiP2pManager manager,
        Channel channel,
        WiFiDirectActivity wifiDirectActivity
    ) {
        super();

        this.manager = manager;
        this.channel = channel;
        this.wifiDirectActivity = wifiDirectActivity;
    }


    @Override
    public void onReceive(Context context, Intent intent) {
        String action = intent.getAction();

        // TODO
        if (action != null) {
            switch (action) {
            // ...
            }
        }
    }
}
```
{% endtab %}
{% endtabs %}

{% page-ref page="broadcast/olusturma.md" %}

### 🎫 Broadcast Alıcısını Kaydetme

* ▶️ Uygulama çalıştığında alıcının kayıt edilmesi
* 🧹 Durdurulduğunda kaydın silinmesi gerekir
* 🎳 Kayıt silinmezse gereksiz yere sistemi yorar ve alıcı kayıtları defalarca kaydedilebilir

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
class WifiDirectActivity : AppCompatActivity() {
     
     // ...
     
    /**
     * Wifi alıcısı
     */
     private lateinit var wifiReceiver: WifiDirectBroadcastReceiver
     
     // ...
     
     private fun initWifiP2p(): Unit {
        manager = getSystemService(Context.WIFI_P2P_SERVICE) as WifiP2pManager
        channel = manager.initialize(this, mainLooper, null)
        
        // Yeni kısım 👇
        wifiReceiver = WifiDirectBroadcastReceiver(manager, channel, this)
        
        if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.M) {
            getPermissions(Manifest.permission.ACCESS_FINE_LOCATION)
        }
    }
     
    private fun registerWifiReceiver(): Unit {
        registerReceiver(wifiReceiver, wifiFilter)
    }
    
    private fun unregisterWifiReceiver(): Unit {
        unregisterReceiver(wifiReceiver)
    }
    
    override fun onResume() {
        super.onResume()
        registerWifiReceiver()
    }
    
    override fun onPause() {
        super.onPause()
        unregisterWifiReceiver()
    }
}
```
{% endtab %}

{% tab title="Java" %}
```java
@Override
protected void onResume() {
    super.onResume();

    registerWifiFilter();
}

@Override
protected void onPause() {
    super.onPause();

    unregisterWifiFilter();
}

/**
 * Broadcast alıcısını oluşturma ve sisteme kaydetme
 */
private void registerWifiFilter() {
    wifiReceiver = new WiFiDirectBroadcastReciever(manager, channel, this);
    registerReceiver(wifiReceiver, wifiFilter);
}

/**
 * Broadcast alıcısının kaydını silme
 */
private void unregisterWifiFilter() {
    unregisterReceiver(wifiReceiver);
}

```
{% endtab %}
{% endtabs %}

{% page-ref page="broadcast/receiver.md" %}

## 🐞 Hata Çözümleri

### 🚫 Peers objesinin boş dönmesi

* 👮‍♂️ İlk olarak `ACCESS_FINE_LOCATION` iznini cihaza vermelisin
* 📍 Ardından cihazda **konum** hizmetini açmalısın
* 🎉 Eşleşen cihazlar belirmeye başlayacaktır

{% hint style="info" %}
‍🧙‍♂ Detaylı bilgi için [Android Wifi Direct detects peers but Peer List is empty](https://stackoverflow.com/questions/30653646/android-wifi-direct-detects-peers-but-peer-list-is-empty) alanına bakabilirsin.
{% endhint %}

## 🔗 Faydalı Bağlantılar

* [⭐ EasyWifiP2P ~ Wifi P2P kütüphanesi](https://github.com/cubesky/EasyWifiP2P)
* [⭐ SocketChannel ~ Socket yönetimi](https://github.com/cubesky/SocketChannel)
* [📖 WiFi Direct P2P Overview](https://developer.android.com/guide/topics/connectivity/wifip2p.html)
* [📖 Create P2P connections with Wi-Fi Direct](https://developer.android.com/training/connect-devices-wirelessly/wifi-direct#kotlin) \(Eski\)
* [👨‍💻 WiFi Direct Demo](https://android.googlesource.com/platform/development/+/master/samples/WiFiDirectDemo/src/com/example/android/wifidirect?autodive=0%2F)
* [📺 WiFi Direct Tutorial](https://www.youtube.com/watch?v=nw627o-8Fok&list=PLFh8wpMiEi88SIJ-PnJjDxktry4lgBtN3)

