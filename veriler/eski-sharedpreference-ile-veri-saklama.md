# 👐 Eski SharedPreference ile Veri Saklama

### Veri Oluşturma ve Alma

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

### Veri Güncelleme

```text
age = 31
veri.edit().putInt("userAge", age).apply() // Daha önceden olan bir anahtarın üstüne kaydedilirse güncelleme olur.
```

### Veri Silme

* `veri.edit().remove("userAge").apply() // Veri silindi`
  * userAge : silinecek anahtar
* `val age3 = veri.getInt("userAge", 0) // Veri olmadığı için age3 = 0 olacak.`
  * userAge : anahtar
  * 0 : varsayılan değer

