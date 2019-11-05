# ⏩ Activity ve Intent'ler

##  Create the Activity

When you create a new project in Android Studio and choose the **Backwards Compatibility \(AppCompat\)** option, the `MainActivity` is, by default, a subclass of the [`AppCompatActivity`](https://developer.android.com/reference/android/support/v7/app/AppCompatActivity.html) class. The `AppCompatActivity` class lets you use up-to-date Android app features such as the app bar and Material Design, while still enabling your app to be compatible with devices running older versions of Android.

Here is a skeleton subclass of `AppCompatActivity`:

```java
public class MainActivity extends AppCompatActivity {
   @Override
   protected void onCreate(Bundle savedInstanceState) {
       super.onCreate(savedInstanceState);
       setContentView(R.layout.activity_main);
   }
}
```

### 📑 Declare the Activity in AndroidManifest.xml

```java
<activity android:name=".MainActivity" >
   <intent-filter>
      <action android:name="android.intent.action.MAIN" />
      <category android:name="android.intent.category.LAUNCHER" />
   </intent-filter>
</activity>
```

## ⏫ Starting an Activity with an explicit Intent

Yeni aktivity oluşturulduğunda eskisi **Paused** olur

```java
Intent messageIntent = new Intent(this, ShowMessageActivity.class);
startActivity(messageIntent);
```

## 💾 Add data to the Intent

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

### Add extras to the Intent

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

### Retrieve the data from the Intent in the started Activity

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

## 🔙 Getting data back from an Activity

### Use startActivityForResult\(\) to launch the Activity

```java
startActivityForResult(messageIntent, TEXT_REQUEST);

// İstek tipleri
public static final int PHOTO_REQUEST = 1;
public static final int PHOTO_PICK_REQUEST = 2;
public static final int TEXT_REQUEST = 3;
```

### Return a response from the launched Activity

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
To avoid confusing sent data with returned data, use a new `Intent` object rather than reusing the original sending `Intent` object.
{% endhint %}

### Read response data in onActivityResult\(\)

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

![](../.gitbook/assets/image%20%284%29.png)

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
To support older versions of Android, include `<meta-data>` information to define the parent `Activity` explicitly
{% endhint %}

