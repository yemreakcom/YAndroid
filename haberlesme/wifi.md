---
description: Destekleyen cihazlar için android WiFi P2P bağlantısı
---

# 📶 WiFi P2P

## 📻 WiFi için Broadcast Receiver Tanımlama

### 💎 WiFi Durumları

| Intent | Tetiklenme Sebebi |
| :--- | :--- |
| [`WIFI_P2P_CONNECTION_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_CONNECTION_CHANGED_ACTION) | Cihazın WiFi bağlantısının durumu değişikliği |
| [`WIFI_P2P_PEERS_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_PEERS_CHANGED_ACTION) | Bağlanacak cihazları [`discoverPeers()`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#discoverPeers%28android.net.wifi.p2p.WifiP2pManager.Channel,%20android.net.wifi.p2p.WifiP2pManager.ActionListener%29) metodu ile keşfetmek istediğimiz zaman tetiklenir. Genellikle  [`requestPeers()`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#requestPeers%28android.net.wifi.p2p.WifiP2pManager.Channel,%20android.net.wifi.p2p.WifiP2pManager.PeerListListener%29) metodu ile eşleşen cihazları alma amaçlı kullanılır. |
| [`WIFI_P2P_STATE_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_STATE_CHANGED_ACTION) | WiFi P2P'in aktifliğinin değişmesi \(açık / kapalı\) |
| [`WIFI_P2P_THIS_DEVICE_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_THIS_DEVICE_CHANGED_ACTION) | Cihaz detaylarının \(örn: ismi\) değişmesi |

### 👨‍💻 Kod Tarafında Tanımlama

Broadcast Receiver ile WiFi durumlarını kontrol edebiliriz

{% code title="WifiActivity.java" %}
```java
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
```
{% endcode %}

{% page-ref page="broadcast/olusturma.md" %}

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

Aşağıdaki metotlar **Location Mode** iznine de ihtiyaç duyar

* [`discoverPeers`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#discoverPeers%28android.net.wifi.p2p.WifiP2pManager.Channel,%20android.net.wifi.p2p.WifiP2pManager.ActionListener%29)
* [`discoverServices`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager#discoverServices%28android.net.wifi.p2p.WifiP2pManager.Channel,%2520android.net.wifi.p2p.WifiP2pManager.ActionListener%29)
* [`requestPeers`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager#requestPeers%28android.net.wifi.p2p.WifiP2pManager.Channel,%2520android.net.wifi.p2p.WifiP2pManager.PeerListListener%29)

### 👨‍💻 Kod Tarafında İzin İsteme

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
            Log.w(TAG, "onRequestPermissionsResult: Fine location izni gereklidir");
            Toast.makeText(this, "İzinler gereklidir 😥", Toast.LENGTH_SHORT).show();
        }
    }
}
```

{% page-ref page="../temel/izinlerin-yoenetimi.md" %}

## 🎫 Broadcast Alıcısını Kaydetme

* ▶️ Uygulama çalıştığında alıcının kayıt edilmesi
* 🧹 Durdurulduğunda kaydın silinmesi gerekir
* 🎳 Kayıt silinmezse gereksiz yere sistemi yorar ve alıcı kayıtları defalarca kaydedilebilir

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

* [📖 WiFi Direct P2P Overview](https://developer.android.com/guide/topics/connectivity/wifip2p.html)
* [👨‍💻 WiFi Direct Demo](https://android.googlesource.com/platform/development/+/master/samples/WiFiDirectDemo/src/com/example/android/wifidirect?autodive=0%2F)

