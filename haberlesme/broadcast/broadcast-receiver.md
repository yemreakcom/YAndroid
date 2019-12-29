# 📡 Broadcast Receiver

## ❣️ Receiver Hakkında

* 🚫 UI thread üzerinden gerçekleştiğinden uzun işlemler yapılmamalı
* ⛔  `onReceive()` metodu içerisinde asenkron işlemler yapmayın
  * 🤷‍♂️ Yapsanız bile `return`metodundan sonra broadcast işlemleri sonlandırılır
  * ☠️ Haliyle işlem asenkron olsa bile broadcast yapısına bağlı olduğundan ölecektir
* 🗨 `AlertDialog` gibi işlemler yerine `Notification` yapısı tercih edilmelidir

![](../../.gitbook/assets/image%20%282%29.png)

```java
//Subclass of the BroadcastReceiver class.
private class myReceiver extends BroadcastReceiver {
   // Override the onReceive method to receive the broadcasts
   @Override
   public void onReceive(Context context, Intent intent) {
      //Check the Intent action and perform the required operation
        if (intent.getAction().equals(ACTION_SHOW_TOAST)) {
            CharSequence text = "Broadcast Received!";
            int duration = Toast.LENGTH_SHORT;

            Toast toast = Toast.makeText(context, text, duration);
            toast.show();
       }
   }
}
```

{% hint style="info" %}
‍‍🧙‍♂ Detaylı bilgi için  [Broadcast receivers](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-3-working-in-the-background/lesson-7-background-tasks/7-3-c-broadcasts/7-3-c-broadcasts.html#broadcast_receivers) alanına bakabilirsin.
{% endhint %}

## 🔸 Receiver Türleri

### 🎳 Static Receiver

* 📝 Manifest üzerinden kayıt edilmeleri gerekir
* 😥 Uygulamamızı hedef almayan yayınlarını Android 8.0'dan itibaren alamaz
* 🎈 [implicit broadcast exceptions](https://developer.android.com/guide/components/broadcast-exceptions) yayınlarını hala alabilmektedir

```java
<receiver
 android:name=".AlarmReceiver"
 android:exported="false">
 <intent-filter>
     <action android:name=    
          "com.example.myproject.intent.action.ACTION_SHOW_TOAST"/>
 </intent-filter>
</receiver>
```

### ✨ Dynamic Receiver

* 👀 Uygulama üzerinden ilgilendiğimiz broadcast'e erişmek için `IntentFilter` kullanırız
* 🏗️ Genel kullanımı `onCreate` üzerinde yapılmaktadır \(?\)

```java
 IntentFilter intentFilter = new IntentFilter();
 filter.addAction(Intent.ACTION_POWER_CONNECTED);
 filter.addAction(Intent.ACTION_POWER_DISCONNECTED);
```

### 🎫 Broadcast Kayıtları

* 🎌 İlk olarak `receiver` yapısını uygulamamıza `registerReceiver` ile kaydederiz
* 🙋‍♂️ Genelde `onResume` içerisinde `registerReceiver` işlemi yapılır
* 🚫 `onPause` içerisinde `unregisterReceiver` metodu ile kaldırırız

```java
mReceiver = new AlarmReceiver();
this.registerReceiver(mReceiver, intentFilter);
```

```java
 unregisterReceiver(mReceiver);
```

{% hint style="info" %}
‍🧙‍♂ Detaylı bilgi için  [Broadcast receivers](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-3-working-in-the-background/lesson-7-background-tasks/7-3-c-broadcasts/7-3-c-broadcasts.html#broadcast_receivers) alanına bakabilirsin.
{% endhint %}

## 🏠 Local Broadcast Alımı

* 👮‍♂️ [Local Broadcast](broadcast-olusturma.md#local-broadcast-yerel), [Dynamic Receiver](broadcast-receiver.md#dynamic-receiver) ile alınmak zorundadır

```java
LocalBroadcastManager.getInstance(this)
    .registerReceiver(mReceiver, 
           new IntentFilter(CustomReceiver.ACTION_CUSTOM_BROADCAST));
```

```text
 LocalBroadcastManager.getInstance(this)
    .unregisterReceiver(mReceiver);
```

## 🔗 Faydalı Bağlantılar

{% embed url="https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-3-working-in-the-background/lesson-7-background-tasks/7-3-c-broadcasts/7-3-c-broadcasts.html\#broadcast\_receivers" %}

