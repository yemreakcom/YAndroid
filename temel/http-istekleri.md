---
description: 'Android üzerinden HTTP istekleri, request ve response yapısı'
---

# 💌 HTTP İstekleri

## 🔰 HTTP İsteği Nedir?

* 🙏 Web sitelerinden içerik istemek için kullanılırlar
* 💫 API işlemlerinde sıklıkla kullanılırlar
* 🧱 GET, SET; POST vb. istek tipleri vardır

## 🆚 Volley ile Retrofit Farkı

| 📦 [Volley](https://developer.android.com/training/volley) | 📦 [Retrofit](https://square.github.io/retrofit/) |
| :--- | :--- |
| Önbellek mekanizması | ➖ |
| Bağlantıyı tekrardan gönderme | ➖ |
| Gömülü olarak resim yüklenme desteği | ➖ |
| ➖ | JSON verilerini otomatik ayrıştırma |

## ⭐ Volley Örneği

```java
/**
 * https://developer.android.com/training/volley/simple.html
 *
 * @return
 */
static void requestNewsData(Context context, ResponseListener responseListener) {

    RequestQueue queue = Volley.newRequestQueue(context);
    StringRequest stringRequest = new StringRequest(Request.Method.GET, MAIN_URL, (response) -> {
        // https://stackoverflow.com/a/9606629/9770490
        try {
            // new JSONObject(response).getJSONArray("articles").get(1).getString("source")
            JSONObject responseObject = new JSONObject(response);

            String status = responseObject.getString("status");
            if (status.equals("ok")) {
                JSONArray articles = new JSONObject(response).getJSONArray("articles");

                ArrayList<News> newsDataList = new ArrayList<>();
                for (int i = 0; i < articles.length(); i++) {
                    JSONObject article = articles.getJSONObject(i);
                    News news =  new News();

                    news.setTitle(article.getString("title"));
                    news.setDescription(article.getString("description"));
                    news.setUrlToImage(article.getString("urlToImage"));
                    news.setUrl(article.getString("url"));
                    news.setContent(article.getString("content"));
                    news.setSource(article.getJSONObject("source").getString("name"));
                    news.setPublishedAt(article.getString("publishedAt"));

                    newsDataList.add(news);

                    Log.i(TAG, "getNewsData: " + newsDataList.get(i));
                }

                responseListener.onResponse(newsDataList);
            }
        } catch (JSONException e) {
            e.printStackTrace();
        }
    }, Throwable::printStackTrace);

    queue.add(stringRequest);
}

interface ResponseListener {
    void onResponse(ArrayList<News> newsDataList);
}
```

