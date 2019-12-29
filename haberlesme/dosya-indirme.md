---
description: 'Android üzerinde internet üzerinden dosya indirme, ayrıştırma ve kullanma'
---

# ⏬ Dosya İndirme

## 🏗️ URI Oluşturma

```java
// Base URL for the Books API.
final String BOOK_BASE_URL =
   "https://www.googleapis.com/books/v1/volumes?";

// Parameter for the search string
final String QUERY_PARAM = "q"; 
// Parameter to limit search results.
final String MAX_RESULTS = "maxResults"; 
// Parameter to filter by print type
final String PRINT_TYPE = "printType"; 

// Build up the query URI, limiting results to 5 printed books.
Uri builtURI = Uri.parse(BOOK_BASE_URL).buildUpon()
       .appendQueryParameter(QUERY_PARAM, "pride+prejudice")
       .appendQueryParameter(MAX_RESULTS, "5")
       .appendQueryParameter(PRINT_TYPE, "books")
       .build();
```

## 💌 İndirme İsteğinde Bulunma

```java
private String downloadUrl(String myurl) throws IOException {
    InputStream inputStream = null;
    // Only display the first 500 characters of the retrieved
    // web page content.
    int len = 500;

    try {
        URL url = new URL(myurl);
        HttpURLConnection conn = 
          (HttpURLConnection) url.openConnection();
        conn.setReadTimeout(10000 /* milliseconds */);
        conn.setConnectTimeout(15000 /* milliseconds */);
        // Start the query
        conn.connect();
        int response = conn.getResponseCode();
        Log.d(DEBUG_TAG, "The response is: " + response);
        inputStream = conn.getInputStream();

        // Convert the InputStream into a string
        String contentAsString = 
           convertInputToString(inputStream, len);
        return contentAsString;

    // Close the InputStream and connection
    } finally {
          conn.disconnect();
        if (inputStream != null) {
            inputStream.close();
        }
    }‍🧙‍♂ Detaylı bilgi için TEMP alanına bakabilirsin
```

{% hint style="warning" %}
📢 İstekte bulunmadan önce [👨‍💼 Bağlantı Durumunu Yönetme](internete-baglanma.md#baglanti-durumunu-yoenetme) alanından bağlantını kontrol etmelisin
{% endhint %}

## 🔄 Veri Akışını Objeye Çevirme

```java
// Reads an InputStream and converts it to a String.
public String convertInputToString(InputStream stream, int len) 
         throws IOException, UnsupportedEncodingException {
    Reader reader = null;
    reader = new InputStreamReader(stream, "UTF-8");
    char[] buffer = new char[len];
    reader.read(buffer);
    return new String(buffer);
}
```

## 🧐 Sonucu Ayrıştırma

{% page-ref page="../veriler/json-yoenetimi.md" %}

## 🔗 Faydalı Bağlantılar

* 📖 [Concepts - 7.2: Internet connection](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-3-working-in-the-background/lesson-7-background-tasks/7-2-c-internet-connection/7-2-c-internet-connection.html#parse_results)
* [👨‍💻 Code 7.2: AsyncTask and AsyncTaskLoader](https://codelabs.developers.google.com/codelabs/android-training-asynctask-asynctaskloader).

