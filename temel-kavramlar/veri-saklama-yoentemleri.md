---
description: Android üzerinde verileri kaydetme
---

# 💾 Veri Saklama Yöntemleri

## 👐 SharedPreference ile Veri Saklama

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

## 📂 SQLite ile Database Oluşturma

### SQLite Giriş Temelleri

İlk olarak try - catch yapısı kurulur ve olası sorunda programın kapanması engellenir.

```text
try {
    ...
}
catch (e : Exception){
    e.printStackTrace()
}
```

> Bütün kodları `...` olan yere yazacağız. Artık başlayabiliriz.

### SQLite ile Basit DB Oluşturma

`database = openOrCreateDatabase("Datas", Context.MODE_PRIVATE, null)`

* "Datas" : Oluştumak istediğimiz database'in adı \("Veriler", "Hey", "hop" vb.\)
  * Yazım kuralları gereği database adı büyük harfle başlamalı
* Context.MODE\_PRİVATE : Database'i private \(özel\) sadece bizim erişebileceğimiz halde kurmak.
  * \(Context.MODE yazıp ALT+ SPACE yaparsanız detaylar çıkacaktır karşınıza\)
* null : CursorFactory

### SQLite DB Oluşturma Kodları

```text
try {
    val database = openOrCreateDatabase("Datas", Context.MODE_PRIVATE, null)

    database.execSQL("CREATE TABLE IF NOT EXIST datas (name VARCHAR, age INT(2)")
// INT(2) ile 2 rakam olacağını belli ediyoruz
} catch (e : Exception){
    e.printStackTrace()
}
```

* `CREATE TABLE IF NOT EXITS` table oluşturma
* `datas` table ismi
* `VARCHAR` char
* `INT` Int

### SQLite DB İşlemleri Değiştirme

Temel yapısı `database.execSQL("...")` şeklindedir.

```text
database.execSQL("INSERT INTO datas (name, age) VALUES ('Yunus' , 21)") // Veri Ekleme
database.execSQL("INSEER INTO datas (name, age) VALUES ('Emre', 15") // Veri Ekleme
database.execSQL("UPDATE datas SET age = 21 WHERE name = 'Yunus") // Veri güncelleme
database.execSQL("DELETE FROM datas WHERE age = 15") // Veri silme
database.execSQL("SELECT FROM datas WHERE name = 'Yunus") // Yunus isimli olan dataları alır
database.execSQL("SELECT FROM datas WHERE name LIKE '%s'") // sonunda 's' harfi olanları alır
database.execSQL("SELECT FROM datas WHERE name LIKE 'y%") // başında 'y' harfi olanları alır
database.execSQL("SELECT FROM datas WHERE name LIKE '%u%") // içinde 'u' harfi olanları alır
```

* `INSERT INTO` Veri ekleme için SQL kodu
* `UPDATE` Veri güncelleme
* `SELECT` Veri seçme
* `datas` table ismi
* `name` değişken ismi
* `age` değişken ismi
* `VALUES` değerleri atamak için SQL kodu
* `'Yunus'` VARCHAR \(string\) tipindeki veri
* `21` INT\(2\) \(Int\) tipindeki veri

### SQLite DB Okuma

```text
if (database != null) {
    val cursor = database!!.rawQuery("SELECT * FROM datas", null)

    val nameIndex = cursor.getColumnIndex("name")
    val ageIndex = cursor.getColumnIndex("age")

    cursor.moveToFirst()

    while (cursor != null){
        println("İsim : ${cursor.getString(nameIndex)}" )
        println("Yaş : ${cursor.getString(ageIndex)}")

        cursor.moveToNext()
    }
}
```

* `rawQuery(...)` SQL kodu ile veri alma
* `SELECT * FROM` Bütün verileri almak için SQL kodu
* `nameIndex` name sütünundaki verilerin indexi
* `ageIndex` age sütunundaki verilerin indexi
* `Cursor.moveToFirst()` Cursoru ilk elemana atıyoruz
* `Cursor.getString()` İstenen indexteki string olarak döndürür.
* `Cursor.moveToNext()` cursoru bir sütün aşağı indirme

## 📜 JSON Verilerini İşleme

{% embed url="https://stackoverflow.com/a/9606629/9770490" %}

