---
description: Destekleyen cihazlar için android WiFi P2P bağlantısı
---

# 📶 WiFi P2P \(Direct\)

## 🏗️ WiFi P2P Activity Oluşturma

### 👨‍💻 WiFi P2P Sınıfını Kodlama 

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
class WifiP2pActivity : AppCompatActivity() {

    /**
     * WiFi değişikliklerinde receiver'ı çalıştırma
     */
    private lateinit var manager: WifiP2pManager

    /**
     * WiFi P2P Framework'ü ile uygulamamıza bağlanmayı sağlayacak obje
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
            // getWifiP2PPermissions()
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
    
    <!-- Wi-Fi Direct (P2P) bağlantısı için eklendi -->
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
	private fun getWifiP2PPermissions() {
		if (!hasWifiP2PPermission()) {
			requestWifiP2PPermissions()
		}
	}

	@RequiresApi(Build.VERSION_CODES.M)
	private fun hasWifiP2PPermission(): Boolean {
		return checkSelfPermission(Manifest.permission.ACCESS_FINE_LOCATION) 
		== PackageManager.PERMISSION_GRANTED
	}

	@RequiresApi(Build.VERSION_CODES.M)
	private fun requestWifiP2PPermissions() {
		requestPermissions(
			arrayOf(Manifest.permission.ACCESS_FINE_LOCATION), 
			PRC_ACCESS_FINE_LOCATION
		)
	}

	override fun onRequestPermissionsResult(
		requestCode: Int, 
		permissions: Array<out String>, 
		grantResults: IntArray
	) {
		super.onRequestPermissionsResult(requestCode, permissions, grantResults)
		when(requestCode) {
			PRC_ACCESS_FINE_LOCATION -> 
				if (grantResults[0] != PackageManager.PERMISSION_GRANTED) {
					Toast.makeText(this, "Konum izni gereklidir", Toast.LENGTH_SHORT).show()
					finish()
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

## 📻 WiFi için Broadcast Receiver Tanımlama

### 📡 Broadcast Alıcısı Tanımlama

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
open class WifiP2PBroadcastReceiver(
    var manager: WifiP2pManager,
    var channel: WifiP2pManager.Channel,
    var wifiP2pActivity: WifiP2pActivity
) : BroadcastReceiver() {

    companion object {
        val TAG = this::class.java.simpleName
    }

    @RequiresPermission(Manifest.permission.ACCESS_FINE_LOCATION)
    override fun onReceive(context: Context, intent: Intent) {
    		when (intent.action) {
    			WifiP2pManager.WIFI_P2P_STATE_CHANGED_ACTION -> onStateChanged(intent)
    			WifiP2pManager.WIFI_P2P_PEERS_CHANGED_ACTION -> onPeerChanged()
    			WifiP2pManager.WIFI_P2P_CONNECTION_CHANGED_ACTION ->
    				onConnectionChanged()
    			WifiP2pManager.WIFI_P2P_THIS_DEVICE_CHANGED_ACTION ->
    				onThisDeviceChanged()
    		}
	  }

    private fun onStateChanged(intent: Intent): Unit {
        Log.d(TAG, "onStateChanged: Wifi P2P durumu değişti")
    }

    private fun onPeerChanged(): Unit {
        Log.d(TAG, "onPeerChanged: WiFi eşleri değişti")
    }

    private fun onConnectionChanged(): Unit {
        Log.d(TAG, "onConnectionChanged: WiFi P2P bağlantısı değişti")
    }

    private fun onThisDeviceChanged(): Unit {
        Log.d(TAG, "onThisDeviceChanged: Cihazın WiFi P2P durumu değişti")
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

### 🎫 Broadcast Alıcısını Kaydetme

* ▶️ Uygulama çalıştığında alıcının kayıt edilmesi
* 🧹 Durdurulduğunda kaydın silinmesi gerekir
* 🎳 Kayıt silinmezse gereksiz yere sistemi yorar ve alıcı kayıtları defalarca kaydedilebilir

{% tabs %}
{% tab title="Kotlin" %}
```kotlin
class WifiP2pActivity : AppCompatActivity() {
     
     // ...
     
     /**
     * WiFi alıcısı için filtreleme
     */
    private val wifiFilter = IntentFilter().apply {
        addAction(WifiP2pManager.WIFI_P2P_STATE_CHANGED_ACTION)
        addAction(WifiP2pManager.WIFI_P2P_PEERS_CHANGED_ACTION)
        addAction(WifiP2pManager.WIFI_P2P_CONNECTION_CHANGED_ACTION)
        addAction(WifiP2pManager.WIFI_P2P_THIS_DEVICE_CHANGED_ACTION)
    }
     
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

## 👷‍♂️ P2P Durum Değişikliklerini Algılama

* 👮‍♂️ Keşfetme işlemlerine başlamadan önce WiFi durumu kontrol edilmelidir
* ✖️ Eğer WiFi P2P aktif değilse keşif yapılamaz

{% code title="activity\_wifip2p.xml" %}
```markup
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".WifiP2pActivity">

    <TextView
        android:id="@+id/tvP2pStatus"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="false"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintHorizontal_bias="0.498"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintVertical_bias="0.053" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
{% endcode %}

{% code title="WifiP2pActivity.java" %}
```kotlin
/**
 * WiFi P2P aktiflik durumu
 */
var p2pEnable: Boolean = false
		set(value) {
			field = value
			tvP2pStatus.text = value.toString()
		}
```
{% endcode %}

{% code title="WifiP2PBroadcastReceiver.java" %}
```kotlin
/**
 * Wifi P2P durum değişikliklerinde tetiklenir
 */
private fun onStateChanged(intent: Intent): Unit {
	Log.d(TAG, "onStateChanged: Wifi P2P durumu değişti")

	wifiP2pActivity.p2pEnable = when (
		intent.getIntExtra(WifiP2pManager.EXTRA_WIFI_STATE, -1)
		) {
		WifiP2pManager.WIFI_P2P_STATE_ENABLED -> true
		else -> false
	}
}
```
{% endcode %}

## 🔍 Eşleşebilecek Cihazları Arama

* 🔍 Keşif işlemi `manager.discover` metodu ile yapılır
* ✔️ Keşif başarılı olursa, `WIFI_P2P_PEERS_CHANGED_ACTION` haberi salınır
* 🕵️‍♂️ `BroadcastReceiver` üzerinden haber durumunda ne yapılacağına karar verilir
* 💁‍♂️ `onPeerChanged` metodu tetiklenecektir

{% code title="acitivity\_wifip2p.xml" %}
```markup
<androidx.constraintlayout.widget.ConstraintLayout>

    <!-- ... -->
    
    <Button
    android:id="@+id/btnDiscover"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_marginTop="16dp"
    android:text="Discover"
    android:onClick="onDiscoverButtonClicked"
    app:layout_constraintEnd_toEndOf="parent"
    app:layout_constraintStart_toStartOf="parent"
    app:layout_constraintTop_toBottomOf="@+id/tvP2pStatus" />

</androidx.constraintlayout.widget.ConstraintLayout>
```
{% endcode %}

{% code title="WifiP2pActivity.java" %}
```kotlin
@RequiresPermission(Manifest.permission.ACCESS_FINE_LOCATION)
fun onDiscoverButtonClicked(view: View) {
    Log.d(TAG, "onDiscoverButtonClicked: Butona tıklandı")

    manager.discoverPeers(channel, P2pActionListener("Keşif"))
}

class P2pActionListener(private val purpose: String) : WifiP2pManager.ActionListener {
	override fun onSuccess() {
		Log.d(TAG, "onSuccess: $purpose başarılı")
	}

	override fun onFailure(reason: Int) {
		val reasonMsg = when (reason) {
			WifiP2pManager.P2P_UNSUPPORTED -> "P2P desteklenmiyor"
			WifiP2pManager.ERROR -> "hata oluştur"
			WifiP2pManager.BUSY -> "cihaz başka bir bağlantı ile meşgul"
			else -> ""
		}

		Log.e(TAG, "onDiscoverButtonClick: $purpose başarısız, $reasonMsg")
	}
}
```
{% endcode %}

## 🧾 Eşleşilebilir Cihazları Listeleme

* 🔍 Keşfetme \(discover\) işlemi başarıyla yapıldıktan sonra çalışır
* 🙆‍♂️ Eşleşilebilir cihazların listesi `requestPeers` ile talep edilir
* 💾 Talep edilen liste `onPeerAvailable` metodu içerisinde `peerList` objesine kaydedilir

{% code title="WifiP2pActivity.java" %}
```kotlin
/**
 * Eşleşilebilir cihazların listesi
 */
val peerList = ArrayList<WifiP2pDevice>()

// ...

/**
 * Cihazları peerList objesine kaydetme
 */
fun storePeers(peers: WifiP2pDeviceList) {
	peers.apply {
		Log.v(TAG, "onPeersAvailable: $deviceList")

		peerList.apply {
			if (this != deviceList) {
				clear()
				addAll(deviceList)
			}
		}
	}
}
```
{% endcode %}

{% code title="WifiP2PBroadcastReceiver.java" %}
```kotlin
// ...

@RequiresPermission(Manifest.permission.ACCESS_FINE_LOCATION)
private fun onPeerChanged(): Unit {
    Log.d(TAG, "onPeerChanged: WiFi eşleri değişti")

    manager.requestPeers(channel, wifiP2pActivity::storePeers)
}

// ...
```
{% endcode %}

## 📶 Eşlerden Birine Bağlanma

* 🔗 Bağlanma işlemi için `manager.connect` metodu kullanılır
* 👨‍💼 Bağlantı değiştiğinde [`WIFI_P2P_CONNECTION_CHANGED_ACTION`](https://developer.android.com/reference/android/net/wifi/p2p/WifiP2pManager.html#WIFI_P2P_CONNECTION_CHANGED_ACTION) haberi salınır
* 💡 Bağlantı bilgisi almak için `manager.requestConnectionInfo` metodu ile istekte bulunur
* 🕊️ Bilgi doğrultusunda sunucu ve istemci türüne karar verilir

{% code title="WifiP2pActivity.java" %}
```kotlin
@RequiresPermission(Manifest.permission.ACCESS_FINE_LOCATION)
fun connectPeer(peer: WifiP2pDevice) {
    val config = WifiP2pConfig().apply {
        deviceAddress = peer.deviceAddress
    }

    manager.connect(channel, config, P2pActionListener("Bağlantı"))
}
```
{% endcode %}

{% code title="WifiP2PBroadcastReceiver.java" %}
```kotlin
// WifiP2pManager.WIFI_P2P_CONNECTION_CHANGED_ACTION -> onConnectionChanged()

private fun onConnectionChanged(): Unit {
    Log.d(TAG, "onConnectionChanged: WiFi P2P bağlantısı değişti")

    manager.requestConnectionInfo(channel, wifiP2pActivity::createSocket)
}
```
{% endcode %}

{% code title="WifiP2pActivity.java" %}
```kotlin
fun createSocket(info: WifiP2pInfo) {
	  when {
			isServer(info) -> { /* createServerSocket() */ }
			isClient(info) -> { /* createClientSocket() */ }
		}
}

private fun isServer(info: WifiP2pInfo): Boolean {
		return info.run {
				groupFormed && isGroupOwner
		}
}

private fun isClient(info: WifiP2pInfo): Boolean {
		return info.run {
				groupFormed && !isGroupOwner
		}
}
```
{% endcode %}

## 🕳️ Socket Oluşturma

* 🏹 Veri aktarımı Socket üzerinden yapılmaktadır
* 🏗️ Aktarılmadan önce Client veya Server Socket oluşturulmalıdır
* 📶 Server Socket'in IP adresi Client'e aktarılmalıdır

{% hint style="warning" %}
📢 Socket işlemleri Thread içerisinde yapılmalıdır, UI Thread'in engellenmemesi gerekir
{% endhint %}

```kotlin
inner class ServerClass : Thread() {

    private lateinit var serverSocket: ServerSocket
    private lateinit var socket: Socket

    override fun run() {
        serverSocket = ServerSocket(8888)
        socket = serverSocket.accept()

        socketCreated = true

        sendReceive = SendReceive(socket)
        sendReceive.start()
        Log.i(TAG, "run: ServerClass başarılı")
    }
}
```

```kotlin
inner class ClientClass(inetAddress: InetAddress) : Thread() {

    private val socket = Socket()
    private val hostAddress: String = inetAddress.hostAddress

    override fun run() {
        try {
            socket.apply {
                bind(null)
                connect(InetSocketAddress(hostAddress, 8888), 500)
            }

            sendReceive = SendReceive(socket)
            sendReceive.start()

            Log.i(TAG, "run: ClientClass başarılı")
        } catch (e: Exception) {
            Log.e(TAG, "run", e)
        }
    }
}
```

```kotlin
val MESSAGE_READ = 1
val handler = Handler {
    when (it.what) {
        MESSAGE_READ -> {
            (it.obj as ByteArray).run {
                // tvMsg layout dosyasına sonradan tanımlanacak
                tvMsg.text = String(this, 0, it.arg1) 
            }
        }
    }
    return@Handler true
}
```

```kotlin
inner class SendReceive(private val socket: Socket) : Thread() {

    private val inputStream = socket.getInputStream()
    private val outputStream = socket.getOutputStream()

    override fun run() {
        val buffer = ByteArray(1024)
        var bytes = 0

        while (socket != null) {
            inputStream!!.read(buffer).let {
                if (it > 0) {
                    handler.obtainMessage(
                        MESSAGE_READ, it, -1, buffer
                    ).sendToTarget()
                }
            }
            Log.d(TAG, "SendReceive: Socket kontrol edildi")
        }
    }

    fun write(byteArray: ByteArray) {
        GlobalScope.launch {
            withContext(Dispatchers.IO) {
                outputStream.write(byteArray)
                Log.i(TAG, "write: Yazma başarılı")
            }
        }
    }

    fun write(fileUri: Uri) {
        
        Log.i(TAG, "SendReceive: Dosya gönderme işlemine başlandı")
    
        val cr = contentResolver
        val inputStream = cr.openInputStream(fileUri)
    
        var result: Boolean? = null
        if (inputStream != null) {
            result = SocketAPI.copyFile(inputStream, outputStream!!)
        }
    
        when (result) {
            null -> Log.d(
                    TAG,
                    "write: Sockete yazacak dosya yok ${fileUri.path}"
            )
            true -> Log.d(
                    TAG,
                    "write: Sockete yazma başarılı ${fileUri.path}"
            )
            else -> Log.e(
                    TAG,
                    "write: Sockete yazma başarısız ${fileUri.path}"
            )
        }
    }
    
}
```

```kotlin
fun onSendButtonClick(view: View) {
  Log.d(TAG, "onSendButtonClick: Send butonuna tıklandı}")

  sendReceive.write("hello ${Random.nextInt(10)}".toByteArray())
}
```

```markup
<androidx.constraintlayout.widget.ConstraintLayout>

    <!-- ... -->

    <Button
        android:id="@+id/btnSend"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Send"
        android:onClick="onSendButtonClick"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <TextView
        android:id="@+id/tvMsg"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_marginBottom="17dp"
        android:text="Message"
        app:layout_constraintBottom_toTopOf="@+id/btnSend"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />

</androidx.constraintlayout.widget.ConstraintLayout>
```

```kotlin
fun createServerSocket() {
    ServerClass().also { it.start() }
    tvMsg.text = "Server"
}

fun createClientSocket() {
    ClientClass().also { it.start() }
    tvMsg.text = "Client"
}
```

## 🐞 Hata Çözümleri

### 🚫 Peers objesinin boş dönmesi

* 👮‍♂️ İlk olarak `ACCESS_FINE_LOCATION` iznini cihaza vermelisin
* 📍 Ardından cihazda **konum** hizmetini açmalısın
* 🎉 Eşleşen cihazlar belirmeye başlayacaktır

{% hint style="info" %}
‍🧙‍♂ Detaylı bilgi için [Android Wifi Direct detects peers but Peer List is empty](https://stackoverflow.com/questions/30653646/android-wifi-direct-detects-peers-but-peer-list-is-empty) alanına bakabilirsin.
{% endhint %}

## 🔗 Faydalı Bağlantılar

* [⭐ WiFi P2P \(Direct\) işlemleri için demo](https://github.com/yedhrab/WiFiP2PDemo)
* [⭐ EasyWifiP2P ~ Wifi P2P kütüphanesi](https://github.com/cubesky/EasyWifiP2P)
* [⭐ SocketChannel ~ Socket yönetimi](https://github.com/cubesky/SocketChannel)
* [📖 WiFi Direct P2P Overview](https://developer.android.com/guide/topics/connectivity/wifip2p.html)
* [📖 Create P2P connections with Wi-Fi Direct](https://developer.android.com/training/connect-devices-wirelessly/wifi-direct#kotlin) \(Eski\)
* [👨‍💻 WiFi Direct Demo](https://android.googlesource.com/platform/development/+/master/samples/WiFiDirectDemo/src/com/example/android/wifidirect?autodive=0%2F)
* [📺 WiFi Direct Tutorial](https://www.youtube.com/watch?v=nw627o-8Fok&list=PLFh8wpMiEi88SIJ-PnJjDxktry4lgBtN3)

