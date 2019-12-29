---
description: Telefonda Shared Preference yapısı ile veri saklama
---

# 👐 Shared Preferences

## 🚧 Dosyaları Oluşturma

```java
private String sharedPrefFile = "com.example.android.hellosharedprefs";
mPreferences = getSharedPreferences(sharedPrefFile, MODE_PRIVATE);
```

## 💾 Dosyaları Kaydetme

```java
@Override
protected void onPause() {
    super.onPause();
    SharedPreferences.Editor preferencesEditor = mPreferences.edit();
    preferencesEditor.putInt("count", mCount);
    preferencesEditor.putInt("color", mCurrentColor);
    preferencesEditor.apply();
}
```

## 🔄 Dosyaları Geri Alma

```java
mPreferences = getSharedPreferences(sharedPrefFile, MODE_PRIVATE);
if (savedInstanceState != null) {
    mCount = mPreferences.getInt("count", 1);
    mShowCount.setText(String.format("%s", mCount));

    mCurrentColor = mPreferences.getInt("color", mCurrentColor);
    mShowCount.setBackgroundColor(mCurrentColor);
} else { ... }
```

## 🧹 Dosyaları Temizleme

```java
SharedPreferences.Editor preferencesEditor = mPreferences.edit();
preferencesEditor.putInt("number", 42);
preferencesEditor.clear();
preferencesEditor.apply();
```

## 👀 Değişikleri Takip Etme

```java
public class SettingsActivity extends PreferenceActivity
                              implements OnSharedPreferenceChangeListener {
    public static final String KEY_PREF_SYNC_CONN = 
       "pref_syncConnectionType";

    // ...

    public void onSharedPreferenceChanged(
                              SharedPreferences sharedPreferences,
                              String key) {
        if (key.equals(KEY_PREF_SYNC_CONN)) {
            Preference connectionPref = findPreference(key);
            // Set summary to be the user-description for 
            // the selected value
            connectionPref.setSummary(
               sharedPreferences.getString(key, ""));
        }
    }
}
```

```java
@Override
protected void onResume() {
    super.onResume();
    getPreferenceScreen().getSharedPreferences()
            .registerOnSharedPreferenceChangeListener(this);
}

@Override
protected void onPause() {
    super.onPause();
    getPreferenceScreen().getSharedPreferences()
            .unregisterOnSharedPreferenceChangeListener(this);
}
```

```java
SharedPreferences.OnSharedPreferenceChangeListener listener =
    new SharedPreferences.OnSharedPreferenceChangeListener() {
       public void onSharedPreferenceChanged(
                            SharedPreferences prefs, String key) {
          // listener implementation
       }
};

prefs.registerOnSharedPreferenceChangeListener(listener);
```

## 🗑️ Eski Notlar

### 🏗️ Veri Oluşturma ve Alma

* `val veri= this.getSharedPreferences(this.packageName, android.content.Context.MODE_PRIVATE) // Veri kaydını değişkene atama`
  * this.packageName : paket ismi \(com.... en üst satırdaki\)
  * MODE\_PRIVATE : sadece benim uygulamamdan erişilebilirlik
* `var age1 = 30`
* `veri.edit().putInt("userAge", age1).apply() // Veriyi kaydetme`
  * userAge : anahtar
  * age1 : değer / değişken
* `val age2= veri.getInt("userAge", 0) // Kayıtlı veriyi alma`
  * userAge : anahtar \(put'takini almak için aynı olmalı\)
  * 0 : Eğer anahtar yoksa, varsayılan değer ataması
* `println("stored age : $storedAge") // veriyi gösterme`

### ✨ Veri Güncelleme

```text
age = 31
veri.edit().putInt("userAge", age).apply() // Daha önceden olan bir anahtarın üstüne kaydedilirse güncelleme olur.
```

### 🧼 Veri Silme

* `veri.edit().remove("userAge").apply() // Veri silindi`
  * userAge : silinecek anahtar
* `val age3 = veri.getInt("userAge", 0) // Veri olmadığı için age3 = 0 olacak.`
  * userAge : anahtar
  * 0 : varsayılan değer

## 🔗 Faydalı Bağlantılar

* 📖 [Concepts 9.1: Shared preferences](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-4-saving-user-data/lesson-9-preferences-and-settings/9-1-c-shared-preferences/9-1-c-shared-preferences.html)
* 👨‍💻  [Code 9.1: Shared preferences](https://codelabs.developers.google.com/codelabs/android-training-shared-preferences).

