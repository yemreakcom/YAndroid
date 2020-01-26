---
description: Android üzerinde Java veya Kotlin ile tarih işlemleri
---

# 📅 Tarih işlemleri

## 🔨 Zaman Formatlama

{% tabs %}
{% tab title="☕ Java" %}
```java
SimpleDateFormat dateFormat = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss");
Date myDate = dateFormat.parse(dateString);
```
{% endtab %}
{% endtabs %}

## 🕖 Önceki Tarihleri Alma

{% tabs %}
{% tab title="☕ Java" %}
```java
// Date mydate;

float ONE_DAY = 7 * 24 * 60 * 60 * 1000;
Date newDate = new Date(myDate.getTime() - ONE_DAY);
```
{% endtab %}
{% endtabs %}

## 📅 Takvim API'si ile Tarih

{% tabs %}
{% tab title="☕ Java" %}
```java
Calendar calendar = Calendar.getInstance();
calendar.setTime(myDate);
calendar.add(Calendar.DAY_OF_YEAR, -7);
Date newDate = calendar.getTime();

// String'e aktarılmak istenirse
String date = dateFormat.format(newDate);
```
{% endtab %}
{% endtabs %}

## 🔗 Faydalı Bağlantılar

{% embed url="https://stackoverflow.com/a/3747561/9770490" %}



