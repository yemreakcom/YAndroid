---
description: Destekleyen cihazlar için android WiFi P2P bağlantısı
---

# 📶 WiFi P2P

## 👮‍♂️ Gerekli İzinler

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

## 📻 WiFi için Broadcast Receiver Tanımlama

Broadcast Receiver ile Wi-Fi durumlarını kontrol edebiliriz

{% code title="WifiActivity.java" %}
```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_wifi);

    // Wifi alıcısına aktarılacak broadcast türlerini belirleme
    wifiFilter = new IntentFilter();
    wifiFilter.addAction(WifiP2pManager.WIFI_P2P_STATE_CHANGED_ACTION);
    wifiFilter.addAction(WifiP2pManager.WIFI_P2P_PEERS_CHANGED_ACTION);
    wifiFilter.addAction(WifiP2pManager.WIFI_P2P_CONNECTION_CHANGED_ACTION);
    wifiFilter.addAction(WifiP2pManager.WIFI_P2P_THIS_DEVICE_CHANGED_ACTION);

    // WiFi değişikliklerinde reciever'ı çalıştırma
    manager = (WifiP2pManager) getSystemService(Context.WIFI_P2P_SERVICE);
    if (manager != null) {
        // Wi-Fi P2P Frameworkü ile uygulamamıza bağlanmayı sağlayacak obje
        channel = manager.initialize(this, getMainLooper(), null);

        getRequiredPermissions();
    }
}

private void getRequiredPermissions() {
    if (
        Build.VERSION.SDK_INT >= Build.VERSION_CODES.M &&
        checkSelfPermission(Manifest.permission.ACCESS_FINE_LOCATION) != PackageManager.PERMISSION_GRANTED
    ) {
        requestPermissions(new String[]{Manifest.permission.ACCESS_FINE_LOCATION}, WifiActivity.PRC_ACCES_FINE_LOCATION);
    }
}
```
{% endcode %}

## 💎 WiFi Durumları

| Intent | Tetiklenme Sebebi |
| :--- | :--- |
| [`WIFI_P2P_CONNECTION_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_CONNECTION_CHANGED_ACTION) | Cihazın WiFi bağlantısının durumu değişikliği |
| [`WIFI_P2P_PEERS_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_PEERS_CHANGED_ACTION) | Bağlanacak cihazları [`discoverPeers()`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#discoverPeers%28android.net.wifi.p2p.WifiP2pManager.Channel,%20android.net.wifi.p2p.WifiP2pManager.ActionListener%29) metodu ile keşfetmek istediğimiz zaman tetiklenir. Genellikle  [`requestPeers()`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#requestPeers%28android.net.wifi.p2p.WifiP2pManager.Channel,%20android.net.wifi.p2p.WifiP2pManager.PeerListListener%29) metodu ile eşleşen cihazları alma amaçlı kullanılır. |
| [`WIFI_P2P_STATE_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_STATE_CHANGED_ACTION) | WiFi P2P'in aktifliğinin değişmesi \(açık / kapalı\) |
| [`WIFI_P2P_THIS_DEVICE_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_THIS_DEVICE_CHANGED_ACTION) | Cihaz detaylarının \(örn: ismi\) değişmesi |

### 

## 🐞 Hata Çözümleri

### 🚫 Peers objesinin boş dönmesi

* 👮‍♂️ İlk olarak `ACCESS_FINE_LOCATION` iznini cihaza vermelisin
* 📍 Ardından cihazda **konum** hizmetini açmalısın
* 🎉 Eşleşen cihazlar belirmeye başlayacaktır

{% hint style="info" %}
🧙‍♂️ İstersen bu cevabı yazdığım Stackoverflow sorusuna bakabilirsin
{% endhint %}

## 🔗 Faydalı Bağlantılar

{% embed url="https://developer.android.com/guide/topics/connectivity/wifip2p.html" %}



