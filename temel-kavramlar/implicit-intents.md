# 🏹 Implicit intents

## 🎯 Hangi Amaçla Kullanılır

İşletim sistemi tarafından yönetilen isteklerdir

* Hangi uygulamanın çalıştırılacağına işletim sistemi karar verir
* Diğer uygulamalara istekte bulunmayı sağlar
* [✅ App Chooser](implicit-intents.md#app-chooser) adı verilen yapı ile kullanıcıya seçim hakkı tanınır

![](../.gitbook/assets/image%20%285%29.png)

## ✅ App Chooser

![](../.gitbook/assets/image%20%2812%29.png)

## 

## ✨ Implicit Intent Oluşturma

* `Intent` oluşturmadan önce isteği karşılayabilecek `Activity` var mı kontrol edilmelidir.
* İsteklerini sağlayacak `Activity` olmazsa uygulama kapanır

```java
// Implicit intent oluşturma
Intent sendIntent= new Intent();
sendIntent.setAction(Intent.ACTION_SEND);
sendIntent.putExtra(Intent.EXTRA_TEXT, textMessage);
sendIntent.setType("text/plain");

// Intent başlığını ayarlama
String title = getResources().getString(R.string.chooser_title);

// App Chooser oluşturma
Intent chooser = Intent.createChooser(sendIntent, title);

// İsteği sağlayacak Activity var mı kontrolü
if (sendIntent.resolveActivity(getPackageManager()) != null) {
    startActivity(chooser);
}
```

## 🔛 Implicit Intent Alma

* `AndroidManifest.xml`dosyasında `intent-filter` ile tanımlanan uygulamalardan biri seçilir
* `intent-filter` 0 veya daha fazla `action`,`category`veya `data` içerir
* `intent-filter` içermeyen `Activity`'ler sadece explicit intent ile çağrılabilir
* Birden fazla `intent-filter` veya bir `intent-filter` için birden fazla `action`, `category` veya `data` tanımlanabilir

```markup
<intent-filter>
    <!-- Açılış Activity'si oluduğunu belirtlir -->
    <action android:name="android.intent.action.MAIN" />
    <!-- Launcher ekranında (telefon ana ekranında) gözükmesini sağlar -->
    <category android:name="android.intent.category.LAUNCHER" />
</intent-filter>
```

```markup
<activity android:name="ShareActivity">
    <intent-filter>
        <action android:name="android.intent.action.SEND"/>
        <category android:name="android.intent.category.DEFAULT"/>
        <data android:mimeType="text/plain"/>
    </intent-filter>
</activity>
```

{% tabs %}
{% tab title="Actions" %}
* Action yapısı Intent üzerinde `ACTION_` ön eki ile kullanılır

```markup
<intent-filter>
    <!-- Intent sendIntent = new Intent(Intent.ACTION_SEND); -->
    <action android:name="android.intent.action.EDIT" />
    <action android:name="android.intent.action.VIEW" />
</intent-filter>
```
{% endtab %}

{% tab title="Categories" %}
* Category yapısı Intent üzerinde `CATEGORY_` ön eki ile kullanılır
* Tüm implicit intent objelerine varsayılan olarak `android.intent.category.DEFAULT` atanır

```markup
<intent-filter>
    <category android:name="android.intent.category.DEFAULT" />
    <category android:name="android.intent.category.BROWSABLE" />
</intent-filter>
```
{% endtab %}

{% tab title="Data" %}
Alttaki yapıları vardır

* URI Scheme
* URI Host
* URI Path
* Mime type

```markup
<intent-filter>
    <data android:mimeType="video/mpeg" android:scheme="http" />
    <data android:mimeType="audio/mpeg" android:scheme="http" />
</intent-filter>
```
{% endtab %}
{% endtabs %}

## 🔀 `ShareCompat.IntentBuilder` ile Veri Paylaşma

* Sosyal ağ uygulamalarında veri paylaşmak için kullanılan yöntemdir
* Implicit intent yerine, Android sunduğu bu yapı daha faydalıdır

```java
ShareCompat.IntentBuilder
    .from(this)         // information about the calling activity
    .setType(mimeType)  // mime type for the data
    .setChooserTitle("Share this text with: ") //title for the app chooser
    .setText(txt)       // intent data
    .startChooser();    // send the intent
```

## 👨‍💼 Görevleri Yönetme

* Android'in çalışma yapısı gereği, `Activity`'ler eski açık olanı kullanmak yerine kendileri yeni `Activity` oluştururlar \(Şekil 1\)
* Implicit intent ile açılan `Activity`'ler de, asıl çalışan `Activity`'den bağımsız olarak açılır \(Şekil 2\)

{% hint style="info" %}
Bu yapı **Android Launch Modes** ile değiştirilebilmektedir.
{% endhint %}

![](../.gitbook/assets/image%20%283%29.png)

![](../.gitbook/assets/image.png)

## 🏁 Activity launch modes

`AndroidManifest.xml` dosyası içerisindeki `<activity>` alanının değiştirilmesi ile yönetilir

### 💠 Activity Özellikleri

* Belirtilen `launchMode`değerlerinden biri kullanılır
* Varsayılan olarak `standart` değeri seçilir

| Launch Mode | Anlamı |
| :--- | :--- |
| `standart` | Android'in varsayılan modu |
| `singleTop` | Activity, stack'te en tepede ise yeni işlerde yeni activity oluşturulmaz |
| `singleTask` | Activity için yeni bir işlem tanımlandığında, işlem yapan activity kullanılır, yeni oluşturulmaz |
| `singleInstance` | Activity yalnızca bir kez oluşturulur |

```markup
<activity
   android:name=".SecondActivity"
   android:label="@string/activity2_name"
   android:parentActivityName=".MainActivity"
   android:launchMode="standard">
   <!-- More attributes ... -->
</activity>
```

### 🏴 Intent flags

* Activity attributes gibidir, ama çakışma durumunda bayraklar ele alınır
* `setFlag()` ve `getFlag()` ile kullanılır

| Flag | Launch Mode karşılığı | Anlamı |
| :--- | :--- | :--- |
| [`FLAG_ACTIVITY_NEW_TASK`](https://developer.android.com/reference/android/content/Intent.html#FLAG_ACTIVITY_NEW_TASK) | `singleTask` | İşlem için var olan Activity'i kullanır |
| [`FLAG_ACTIVITY_SINGLE_TOP`](https://developer.android.com/reference/android/content/Intent.html#FLAG_ACTIVITY_SINGLE_TOP) | `singleTop` | Activity, stack'te en tepede ise yeni işlerde yeni activity oluşturulmaz |
| [`FLAG_ACTIVITY_CLEAR_TOP`](https://developer.android.com/reference/android/content/Intent.html#FLAG_ACTIVITY_CLEAR_TOP) |  | Eğer activity stack'te varsa, onu tepeye alıp, üstündeki her activity'i `destroy` eder. FLAG\_ACTIVITY\_NEW\_TASK ile kullanılırsa activity işlemlerini ön plana taşır |

### ‍👨‍💼 Yeni Intent Oluşumunu Yönetme

* Genellikle `onResume()`'den sonra çalışır
* `getIntent()` metodu her zaman, `Activity`'nin kendi `intent`'ini döndürdüğünden bu yapı kullanılır
* `setIntent()` ile Activity intent'i değiştirilir

```java
@Override 
public void onNewIntent(Intent intent) { 
    super.onNewIntent(intent); 
    // Use the new intent, not the original one
    setIntent(intent); 
}
```



