# 🏁 Activity launch modes

## 🔰 Nasıl Yönetilir

`AndroidManifest.xml` dosyası içerisindeki `<activity>` alanının değiştirilmesi ile yönetilir

## 💠 Activity Özellikleri

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

## 🏴 Intent flags

* Activity attributes gibidir, ama çakışma durumunda bayraklar ele alınır
* `setFlag()` ve `getFlag()` ile kullanılır

| Flag | Launch Mode karşılığı | Anlamı |
| :--- | :--- | :--- |
| [`FLAG_ACTIVITY_NEW_TASK`](https://developer.android.com/reference/android/content/Intent.html#FLAG_ACTIVITY_NEW_TASK) | `singleTask` | İşlem için var olan Activity'i kullanır |
| [`FLAG_ACTIVITY_SINGLE_TOP`](https://developer.android.com/reference/android/content/Intent.html#FLAG_ACTIVITY_SINGLE_TOP) | `singleTop` | Activity, stack'te en tepede ise yeni işlerde yeni activity oluşturulmaz |
| [`FLAG_ACTIVITY_CLEAR_TOP`](https://developer.android.com/reference/android/content/Intent.html#FLAG_ACTIVITY_CLEAR_TOP) |  | Eğer activity stack'te varsa, onu tepeye alıp, üstündeki her activity'i `destroy` eder. FLAG\_ACTIVITY\_NEW\_TASK ile kullanılırsa activity işlemlerini ön plana taşır |

## ‍👨‍💼 Yeni Intent Oluşumunu Yönetme

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



