# 📃 Activity ve Intent'ler

## ✨ Activity Oluşturma

* Normal olarak **Activity** class'ı extend edilir
* Eski sürümleri desteklemek için **AppCompat** class'ı extend edilir

```java
public class MainActivity extends AppCompatActivity {
   @Override
   protected void onCreate(Bundle savedInstanceState) {
       super.onCreate(savedInstanceState);
       setContentView(R.layout.activity_main);
   }
}
```

## 📑 Activity Tanımlama

Activity `AndroidManifest.xml` dosyasına aşağıdaki gibi tanıtılmalıdır

```java
<activity android:name=".MainActivity" >
   <intent-filter>
      <action android:name="android.intent.action.MAIN" />
      <category android:name="android.intent.category.LAUNCHER" />
   </intent-filter>
</activity>
```

## ⏫ Explicit Intent ile Activity Başlatma

Yeni `Activity` oluşturulduğunda eskisi **paused** olur

```java
Intent messageIntent = new Intent(this, ShowMessageActivity.class);
startActivity(messageIntent);
```

{% tabs %}
{% tab title="🎈 Basit Kullanım" %}
```java
Intent messageIntent = new Intent(this, ShowMessageActivity.class);
startActivity(messageIntent);
```
{% endtab %}

{% tab title="Data Ekleme" %}
```java
Intent messageIntent = new Intent(this, ShowMessageActivity.class);
// A web page URL
messageIntent.setData(Uri.parse("http://www.google.com")); 
// a Sample file URI
messageIntent.setData(Uri.fromFile(new File("/sdcard/sample.jpg")));
// A sample content: URI for your app's data model
messageIntent.setData(Uri.parse("content://mysample.provider/data")); 
// Custom URI 
messageIntent.setData(Uri.parse("custom:" + dataID + buttonId));

startActivity(messageIntent);
```
{% endtab %}

{% tab title="Extras Ekleme" %}
```java
Intent messageIntent = new Intent(this, ShowMessageActivity.class);

// Tek tek koyma
messageIntent.putExtra(EXTRA_MESSAGE, "this is my message");
messageIntent.putExtra(EXTRA_POSITION_X, 100);
messageIntent.putExtra(EXTRA_POSITION_Y, 500);

// Bundle yapısı
Bundle extras = new Bundle();
extras.putString(EXTRA_MESSAGE, "this is my message");
extras.putInt(EXTRA_POSITION_X, 100);
extras.putInt(EXTRA_POSITION_Y, 500);

messageIntent.putExtras(extras);

startActivity(messageIntent);
```
{% endtab %}

{% tab title="👨‍💻 " %}
```java
Intent intent = getIntent();

// Data alma
Uri locationUri = intent.getData();

// Extra alma
String message = intent.getStringExtra(MainActivity.EXTRA_MESSAGE); 
int positionX = intent.getIntExtra(MainActivity.EXTRA_POSITION_X);
int positionY = intent.getIntExtra(MainActivity.EXTRA_POSITION_Y);

// Bundle alma
Bundle extras = intent.getExtras();
String message = extras.getString(MainActivity.EXTRA_MESSAGE);
```
{% endtab %}
{% endtabs %}

## 🔙 Başlatılan Activity Üzerinden Intent Verilerini Alma

```java
Intent intent = getIntent();

// Data alma
Uri locationUri = intent.getData();

// Extra alma
String message = intent.getStringExtra(MainActivity.EXTRA_MESSAGE); 
int positionX = intent.getIntExtra(MainActivity.EXTRA_POSITION_X);
int positionY = intent.getIntExtra(MainActivity.EXTRA_POSITION_Y);

// Bundle alma
Bundle extras = intent.getExtras();
String message = extras.getString(MainActivity.EXTRA_MESSAGE);
```

## 🔙 Intent Verilerini Eski Activity'e Aktarma

### 🎈 Activity'i `startActivityForResult()` ile Başlatma

```java
startActivityForResult(messageIntent, TEXT_REQUEST);

// İstek tipleri
public static final int PHOTO_REQUEST = 1;
public static final int PHOTO_PICK_REQUEST = 2;
public static final int TEXT_REQUEST = 3;
```

### 🖐 Başlatılan Activity'den Sonucu Alma

```java
Intent returnIntent = new Intent();

public final static String EXTRA_RETURN_MESSAGE = 
                                  "com.example.mysampleapp.RETURN_MESSAGE";

messageIntent.putExtra(EXTRA_RETURN_MESSAGE, mMessage);
setResult(RESULT_OK,replyIntent);

// Activity'i sonlandırma
finish();
```

{% hint style="warning" %}
Gönderilen ve alınan verilerin karışmaması için yeni Intent oluşturarak `new Intent()` işlemler yapın
{% endhint %}

### 👀 Sonuç Verisini Okuma

* `onActivityResult()` yapısından gelen veriyi işleyerek sonucu ele alırız
* Aktivity'den yanıt geldiğinde çalışır

```java
public void onActivityResult(int requestCode, int resultCode,  Intent data) {
    super.onActivityResult(requestCode, resultCode, data);
    if (requestCode == TEXT_REQUEST) {
        if (resultCode == RESULT_OK) {
            String reply = 
               data.getStringExtra(SecondActivity.EXTRA_RETURN_MESSAGE);
               // process data
        }
    }
}
```

## 🏹 Activity Navigation

### Back navigation, tasks, and the back stack

![](../.gitbook/assets/image%20%286%29.png)

### Implement Up navigation with a parent Activity

```markup
<application
    android:allowBackup="true"
    android:icon="@mipmap/ic_launcher"
    android:label="@string/app_name"
    android:roundIcon="@mipmap/ic_launcher_round"
    android:supportsRtl="true"
    android:theme="@style/AppTheme">
    <!-- The main activity (it has no parent activity) -->
    <activity android:name=".MainActivity">
       <intent-filter>
            <action android:name="android.intent.action.MAIN" />

            <category android:name="android.intent.category.LAUNCHER" />
       </intent-filter>
    </activity>

    <!-- The child activity) -->
    <activity android:name=".SecondActivity"
       android:label = "Second Activity"
       android:parentActivityName=".MainActivity">
       <meta-data
          android:name="android.support.PARENT_ACTIVITY"
          android:value="com.example.android.twoactivities.MainActivity" />
       </activity>
</application>
```

{% hint style="info" %}
Eski android sürümlerini desteklemek için `<meta-data>` yapısı kullanılır
{% endhint %}

