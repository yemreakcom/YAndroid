# 💫 Activity lifecycle and state

## Activity states and lifecycle callback methods

![](../.gitbook/assets/image%20%2823%29.png)

### Temel Kullanım

```java
@Override
public void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    // The activity is being created.
}
```

## Saving Activity instance state

* Lifecycle metodlarından değildir
* Kullanıcı `Activity`'den ayrılıken çağırılır.
* Bazen `onStop()`'tan önce çalışır

```java
@Override
public void onSaveInstanceState(Bundle savedInstanceState) {
    super.onSaveInstanceState(savedInstanceState);

    // Save the user's current game state
    savedInstanceState.putInt("score", mCurrentScore);
    savedInstanceState.putInt("level", mCurrentLevel);
}
```

### Restoring Activity instance state

* Kaydedilen `Bundle` verileri `onCreate()` callback metodunda kullanılmakta
* `Activity` oluşturulduktan sonra çalışan `onStart()` metodunun ardından çalışan `onRestoreInstanceState()`callback metodunda da kullanılabilir

{% hint style="warning" %}
İlk kez uygulama oluşturulduğunda `Bundle` verisi olmayacağından `null` kontrolü yapılması gerekir.
{% endhint %}

```java
@Override
protected void onCreate(Bundle savedInstanceState) {
    // Always call the superclass first
    super.onCreate(savedInstanceState); 

    // Check if recreating a previously destroyed instance.
    if (savedInstanceState != null) {
        // Restore value of members from saved state.
        mCurrentScore = savedInstanceState.getInt("score");
        mCurrentLevel = savedInstanceState.getInt("level");
    } else {
        // Initialize members with default values for a new instance.
        // ...
    }
    // ... Rest of code
}
```

