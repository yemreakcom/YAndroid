# 🚧 AsyncTask ve AsyncTaskLoader

### AsyncTask <a id="asynctask"></a>

When `AsyncTask` is executed, it goes through four steps:

1. `onPreExecute()` is invoked on the UI thread before the task is executed. This step is normally used to set up the task, for instance by showing a progress bar in the UI.
2. `doInBackground(Params...)` is invoked on the background thread immediately after `onPreExecute()` finishes. This step performs a background computation, returns a result, and passes the result to `onPostExecute()`. The `doInBackground()` method can also call `publishProgress(Progress...)` to publish one or more units of progress.
3. `onProgressUpdate(Progress...)` runs on the UI thread after `publishProgress(Progress...)` is invoked. Use `onProgressUpdate()` to report any form of progress to the UI thread while the background computation is executing. For instance, you can use it to pass the data to animate a progress bar or show logs in a text field.
4. `onPostExecute(Result)` runs on the UI thread after the background computation has finished. The result of the background computation is passed to this method as a parameter.

### Example of an AsyncTask <a id="example-of-an-asynctask"></a>

```java
private class DownloadFilesTask extends AsyncTask<URL, Integer, Long> {
     protected Long doInBackground(URL... urls) {
         int count = urls.length;
         long totalSize = 0;
         for (int i = 0; i < count; i++) {
             totalSize += Downloader.downloadFile(urls[i]);
             publishProgress((int) ((i / (float) count) * 100));
             // Escape early if cancel() is called
             if (isCancelled()) break;
         }
         return totalSize;
     }

     protected void onProgressUpdate(Integer... progress) {
         setProgressPercent(progress[0]);
     }

     protected void onPostExecute(Long result) {
         showDialog("Downloaded " + result + " bytes");
     }
 }
```

```java
new DownloadFilesTask().execute(url1, url2, url3);
```

The example above goes through three of the four basic `AsyncTask` steps:

* `doInBackground()` downloads content, a long-running task. It computes the percentage of files downloaded from the index of the `for` loop and passes it to `publishProgress()`. The check for `isCancelled()` inside the `for` loop ensures that if the task has been cancelled, the system does not wait for the loop to complete.
* `onProgressUpdate()` updates the percent progress. It is called every time the `publishProgress()` method is called inside `doInBackground()`, which updates the percent progress.
* `doInBackground()` computes the total number of bytes downloaded and returns it. `onPostExecute()` receives the returned result and passes it into `onPostExecute()`, where it is displayed in a dialog.

The parameter types used in this example are:

* `URL` for the "Params" parameter type. The `URL` type means you can pass any number of URLs into the call, and the URLs are automatically passed into the `doInBackground()` method as an array.
* `Integer` for the "Progress" parameter type.
* `Long` for the "Result" parameter type.

### Cancelling Task <a id="executing-an-asynctask"></a>

You can cancel a task at any time, from any thread, by invoking the [`cancel()`](https://developer.android.com/reference/android/os/AsyncTask.html#cancel%28boolean%29) method.

* The `cancel()` method returns `false` if the task could not be cancelled, typically because it has already completed normally. Otherwise, `cancel()` returns `true`.
* To find out whether a task has been cancelled, check the return value of `isCancelled()` periodically from `doInBackground(Object[])`, for example from inside a loop as shown in the example below. The `isCancelled()` method returns `true` if the task was cancelled before it completed normally.
* After an `AsyncTask` task is cancelled, `onPostExecute()` will not be invoked after `doInBackground()` returns. Instead, [`onCancelled(Object)`](https://developer.android.com/reference/android/os/AsyncTask.html#onCancelled%28Result%29) is invoked. The default implementation of `onCancelled(Object)` calls [`onCancelled()`](https://developer.android.com/reference/android/os/AsyncTask.html#onCancelled%28%29) and ignores the result.
* By default, in-process tasks are allowed to complete. To allow `cancel()` to interrupt the thread that's executing the task, pass `true` for the value of `mayInterruptIfRunning`.

### Limitations of AsyncTask <a id="limitations-of-asynctask"></a>

`AsyncTask` is impractical for some use cases:

* Changes to device configuration cause problems.

  When device configuration changes while an `AsyncTask` is running, for example if the user changes the screen orientation, the activity that created the `AsyncTask` is destroyed and re-created. The `AsyncTask` is unable to access the newly created activity, and the results of the `AsyncTask` aren't published.

* Old `AsyncTask` objects stay around, and your app may run out of memory or crash.

  If the activity that created the `AsyncTask` is destroyed, the `AsyncTask` is not destroyed along with it. For example, if your user exits the app after the `AsyncTask` has started, the `AsyncTask` keeps using resources unless you call `cancel()`.

When to use `AsyncTask`:

* Short or interruptible tasks.
* Tasks that don't need to report back to UI or user.
* Low-priority tasks that can be left unfinished.

For all other situations, use `AsyncTaskLoader`, which is part of the `Loader` framework described next.

### Loaders <a id="loaders"></a>

#### Starting a loader <a id="starting-a-loader"></a>

Use the [`LoaderManager`](https://developer.android.com/reference/android/app/LoaderManager.html) class to manage one or more `Loader` instances within an activity or fragment. Use `initLoader()` to initialize a loader and make it active. Typically, you do this within the activity's `onCreate()` method. For example:

```text
// Prepare the loader. Either reconnect with an existing one,
// or start a new one.
getLoaderManager().initLoader(0, null, this);
```

```text
getSupportLoaderManager().initLoader(0, null, this);
```

The `initLoader()` method takes three parameters:

* A unique ID that identifies the loader. This ID can be whatever you want.
* Optional arguments to supply to the loader at construction, in the form of a [`Bundle`](https://developer.android.com/reference/android/os/Bundle.html). If a loader already exists, this parameter is ignored.
* A [`LoaderCallbacks`](https://developer.android.com/reference/android/app/LoaderManager.LoaderCallbacks.html) implementation, which the `LoaderManager` calls to report loader events. In this example, the local class implements the `LoaderManager.LoaderCallbacks` interface, so it passes a reference to itself, `this`.

The `initLoader()` call has two possible outcomes:

* If the loader specified by the ID already exists, the last loader created using that ID is reused.
* If the loader specified by the ID doesn't exist, `initLoader()` triggers the [`onCreateLoader()`](https://developer.android.com/reference/android/app/LoaderManager.LoaderCallbacks.html#onCreateLoader%28int,%20android.os.Bundle%29) method. This is where you implement the code to instantiate and return a new loader.

**Note:** Whether `initLoader()` creates a new loader or reuses an existing one, the given `LoaderCallbacks` implementation is associated with the loader and is called when the loader's state changes. If the requested loader exists and has already generated data, then the system calls `onLoadFinished()` immediately \(during `initLoader()`\), so be prepared for this to happen. Put the call to `initLoader()` in `onCreate()` so that the activity can reconnect to the same loader when the configuration changes. That way, the loader doesn't lose the data it has already loaded.

#### Restarting a loader <a id="restarting-a-loader"></a>

When `initLoader()` reuses an existing loader, it doesn't replace the data that the loader contains, but sometimes you want it to. For example, when you use a user query to perform a search and the user enters a new query, you want to reload the data using the new search term. In this situation, use the `restartLoader()` method and pass in the ID of the loader you want to restart. This forces another data load with new input data.

About the `restartLoader()` method:

* `restartLoader()` uses the same arguments as `initLoader()`.
* `restartLoader()` triggers the `onCreateLoader()` method, just as `initLoader()` does when creating a new loader.
* If a loader with the given ID exists, `restartLoader()` restarts the identified loader and replaces its data.
* If no loader with the given ID exists, `restartLoader()` starts a new loader.

#### LoaderManager callbacks <a id="loadermanager-callbacks"></a>

The `LoaderManager` object automatically calls `onStartLoading()` when creating the loader. After that, the `LoaderManager` manages the state of the loader based on the state of the activity and data, for example by calling `onLoadFinished()` when the data has loaded.

To interact with the loader, use one of the [`LoaderManager` callbacks](https://developer.android.com/reference/android/app/LoaderManager.LoaderCallbacks.html) in the activity where the data is needed:

* Call `onCreateLoader()` to instantiate and return a new loader for the given ID.
* Call `onLoadFinished()` when a previously created loader has finished loading. This is typically the point at which you move the data into activity views.
* Call `onLoaderReset()` when a previously created loader is being reset, which makes its data unavailable. At this point your app should remove any references it has to the loader's data.

### AsyncTaskLoader <a id="asynctaskloader"></a>

[`AsyncTaskLoader`](https://developer.android.com/reference/android/content/AsyncTaskLoader.html) is the loader equivalent of `AsyncTask`. `AsyncTaskLoader` provides a method, `loadInBackground()`, that runs on a separate thread. The results of `loadInBackground()` are automatically delivered to the UI thread, by way of the `onLoadFinished()` `LoaderManager` callback.

### AsyncTaskLoader usage <a id="asynctaskloader-usage"></a>

```java
public StringListLoader(Context context, String queryString) {
   super(context);
   mQueryString = queryString;
}
```

```java
@Override
public List<String> loadInBackground() {
    List<String> data = new ArrayList<String>;
    //TODO: Load the data from the network or from a database
    return data;
}
```

#### Implementing the callbacks <a id="implementing-the-callbacks"></a>

```java
@Override
public Loader<List<String>> onCreateLoader(int id, Bundle args) {
   return new StringListLoader(this, args.getString("queryString"));
}
```

```java
public void onLoadFinished(Loader<List<String>> loader, List<String> data) {
    mAdapter.setData(data);
}
```

