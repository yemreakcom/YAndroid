# ⏬ Scrolling views

## 🔤 ScrollView with a TextView <a id="scrollview-with-a-textview"></a>

To display a scrollable magazine article on the screen, you might use a [`RelativeLayout`](https://developer.android.com/reference/android/widget/RelativeLayout.html) that includes a separate `TextView` for the article heading, another for the article subheading, and a third `TextView` for the scrolling article text \(see the figure below\), set within a `ScrollView`. The only part of the screen that would scroll would be the `ScrollView` with the article text.

![ The layout with a ScrollView](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/images/1-3-c-text-and-scrolling-views/dg_layout_diagram1.png)

## 🎿 ScrollView with a LinearLayout <a id="scrollview-with-a-linearlayout"></a>

A `ScrollView` can contain only one child `View`; however, that `View` can be a [`ViewGroup`](https://developer.android.com/reference/android/view/ViewGroup.html) that contains several `View` elements, such as [`LinearLayout`](https://developer.android.com/reference/android/widget/LinearLayout.html). You can _nest_ a `ViewGroup` such as `LinearLayout` _within_ the `ScrollView`, thereby scrolling everything that is inside the `LinearLayout`.

For example, if you want the subheading of an article to scroll along with the article even if they are separate `TextView` elements, add a `LinearLayout` to the `ScrollView` as a single child `View` as shown in the figure below, and then move the `TextView` subheading and article elements into the `LinearLayout`. The user scrolls the entire `LinearLayout` which includes the subheading and the article.

![ A LinearLayout Inside the ScrollView](https://google-developer-training.github.io/android-developer-fundamentals-course-concepts-v2/images/1-3-c-text-and-scrolling-views/dg_layout_diagram2.png)

When adding a `LinearLayout` inside a `ScrollView`, use `match_parent` for the `LinearLayout` `android:layout_width` attribute to match the width of the parent `ScrollView`, and use `wrap_content` for the `LinearLayout` `android:layout_height` attribute to make it only large enough to enclose its contents.

Since `ScrollView` only supports vertical scrolling, you must set the `LinearLayout` orientation attribute to vertical \(`android:orientation="vertical"`\), so that the entire `LinearLayout` will scroll vertically. For example, the following XML layout scrolls the `article` `TextView` along with the `article_subheading` `TextView`:

```markup
<ScrollView
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:layout_below="@id/article_heading">

   <LinearLayout
      android:layout_width="match_parent"
      android:layout_height="wrap_content"
      android:orientation="vertical">

      <TextView
         android:id="@+id/article_subheading"
         android:layout_width="match_parent"
         android:layout_height="wrap_content"
         android:padding="@dimen/padding_regular"
         android:text="@string/article_subtitle"
         android:textAppearance=
                       "@android:style/TextAppearance.DeviceDefault" />

      <TextView
         android:id="@+id/article"
         android:layout_width="wrap_content"
         android:layout_height="wrap_content"
         android:autoLink="web"
         android:lineSpacingExtra="@dimen/line_spacing"
         android:padding="@dimen/padding_regular"
         android:text="@string/article_text" />
   </LinearLayout>

</ScrollView>
```

