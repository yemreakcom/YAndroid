# 🍱 Menu

## ⭐ Menu Türleri

* ⚙️ Option menu
* 📃 Contextual
* 🍢 Contextual Action Bar
* 🎈 Popup

![](../.gitbook/assets/image%20%2824%29.png)

## ⚙️ App bar ve Option Menu

1. 🏹 Navigation
2. 🔤 Title
3. 🏃‍♂️ Actions
4. 🗃️ Overflow

![](../.gitbook/assets/image%20%2813%29.png)

1. 🍢 App bar
2. 🏃‍♂️ Action icons
3. 🎛️ Overflow Button
4. 🗃️ Overflow menu

## 📦 Bağımlılıkları Dahil Etme

```groovy
 compile 'com.android.support:appcompat-v7:26.1.0'
 compile 'com.android.support:design:26.1.0'
```

## ✨ Tema Kullanma

```markup
 <!-- Base application theme. -->
 <style name="AppTheme" parent="Theme.AppCompat.Light.DarkActionBar">
       <!-- Customize your theme here. -->
       <item name="colorPrimary">@color/colorPrimary</item>
       <item name="colorPrimaryDark">@color/colorPrimaryDark</item>
       <item name="colorAccent">@color/colorAccent</item>
 </style>
```

```markup
 <style name="AppTheme.NoActionBar">
       <item name="windowActionBar">false</item>
       <item name="windowNoTitle">true</item>
 </style>

 <style name="AppTheme.AppBarOverlay" 
             parent="ThemeOverlay.AppCompat.Dark.ActionBar" />

 <style name="AppTheme.PopupOverlay"   
             parent="ThemeOverlay.AppCompat.Light" />
```

```markup
 <activity
      <!-- android:name and android:label code goes here. -->
      android:theme="@style/AppTheme.NoActionBar">
      <!-- intent filter code would go here if needed. -->
 </activity>
```

## 📝 Activity Dosyası

```markup
 <android.support.design.widget.CoordinatorLayout 
     xmlns:android="http://schemas.android.com/apk/res/android"
     xmlns:app="http://schemas.android.com/apk/res-auto"
     xmlns:tools="http://schemas.android.com/tools"
     android:layout_width="match_parent"
     android:layout_height="match_parent"
     tools:context="com.example.android.droidcafeinput.MainActivity">

     <android.support.design.widget.AppBarLayout
         android:layout_width="match_parent"
         android:layout_height="wrap_content"
         android:theme="@style/AppTheme.AppBarOverlay">

         <android.support.v7.widget.Toolbar
             android:id="@+id/toolbar"
             android:layout_width="match_parent"
             android:layout_height="?attr/actionBarSize"
             android:background="?attr/colorPrimary"
             app:popupTheme="@style/AppTheme.PopupOverlay" />

     </android.support.design.widget.AppBarLayout>

     <include layout="@layout/content_main" />

 </android.support.design.widget.CoordinatorLayout>
```

## 🔗 Faydalı Kaynaklar

* 📖 [Concepts ~ 4.3: Menus and pickers](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/unit-2-user-experience/lesson-4-user-interaction/4-3-c-menus-and-pickers/4-3-c-menus-and-pickers.html)



