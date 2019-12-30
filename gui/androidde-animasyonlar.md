# 🎪 Android'de Animasyonlar

## ⏫ Aşağıdan Gelme Animasyonu

* Proje dizinine `anim` adlı klasör oluşturup içinde bir `resource file'`a bu kodları yazıyoruz.
* `Main`'e alttakiler eklenmeli:
  * `btn_start` = Get Started adlı view'ın ID'si
  * `frombutton` = üstteki kodların yazıldığı dosyanın adı

{% tabs %}
{% tab title="⭐ Görsel" %}
![](../.gitbook/assets/image%20%2826%29.png)
{% endtab %}

{% tab title="📜 XML Kodları" %}
```markup
<?xml version="1.0" encoding="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android">
    <translate
          android:duration="800"
          android:fromXDelta="0%p"
          android:fromYDelta="100%p"/>
</set>
```
{% endtab %}

{% tab title="👨‍💻 Main Kodları" %}
```java
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)
        btn_start.animation = AnimationUtils.loadAnimation(this, R.anim.frombuttom)
   }}
    ...
}
```
{% endtab %}
{% endtabs %}

## 👁‍🗨 Soluk Belirme

{% tabs %}
{% tab title="⭐ Görsel" %}
![](../.gitbook/assets/image%20%2827%29.png)
{% endtab %}

{% tab title="📜 XML Kodları" %}
```markup
<?xml version="1.0" encoding="utf-8"?>
<alpha xmlns:android="http://schemas.android.com/apk/res/android"
    android:duration="2000"
    android:fromAlpha="0.0"
    android:toAlpha="1.0"    />
```
{% endtab %}
{% endtabs %}

