---
description: Android üzerinde verileri kaydetme
---

# 🔸 Veri Saklama Yöntemleri

## 🆚 Shared Preference vs Saved Instance State

| 👐 Shared Preference | 💾 Saved Instance State |
| :--- | :--- |
| Uygulama hatta cihaz kapatılsa dahi veriler korunur | Sadece Activity üzerinde verileri saklar |
| Temel veriler için kullanılır | Anlık veriler için tercih edilir |
| Az sayıda anahtar / değer çifti ile saklanır | Az sayıda anahtar / değer çifti ile saklanır |
| Veriler uygulamaya özgüdür \(gizli\) | Veriler uygulamaya özgüdür \(gizli\) |
| Genellikle kullanıcı tercihleri için kullanılır | Cihazın yapılandırma ayarlarında veri kaybını engellemek için kullanılır |

{% hint style="info" %}
‍🧙‍♂ Detaylar için  [Shared preferences vs. saved instance state](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-4-saving-user-data/lesson-9-preferences-and-settings/9-1-c-shared-preferences/9-1-c-shared-preferences.html#whentouse) alanına bakabilirsin.
{% endhint %}

## 🆚 Database vs Shared Preference

| 🗃️ Database | 👐 Shared Preference |
| :--- | :--- |
| 🎳 Yüksek miktarda veriler | ⚙️ Kullanıcı yapılandırmaları |
| 🔍 Veriler içerisinde kompleks aramalar \(SQL\) | 💞 Anahtar / Değer çiftleri |
| 📉 Daha yavaş okuma hızı | 📈 Daha hızlı okuma hızı |

{% hint style="info" %}
‍🧙‍♂ Detaylı bilgi için [Pros and Cons of SQL and Shared Preferences](https://stackoverflow.com/a/6276936/9770490) alanına bakabilirsin.
{% endhint %}

## 🔗 Faydalı Bağlantılar

{% embed url="https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-4-saving-user-data/lesson-9-preferences-and-settings/9-0-c-data-storage/9-0-c-data-storage.html" %}

