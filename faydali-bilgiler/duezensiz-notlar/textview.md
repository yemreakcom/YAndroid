# 🔤 TextView

## TextView Attributes

You can use XML attributes for a `TextView` to control:

* Where the `TextView` is positioned in a layout \(like any other view\)
* How the `TextView` itself appears, such as with a background color
* What the text looks like within the `TextView`, such as the initial text and its style, size, and color

For example, to set the width, height, and initial text value of the view:

```markup
<TextView
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:text="Hello World!"
   <!-- more attributes -->
/>
```

You can extract the text string into a string resource \(perhaps called `hello_world`\) that's easier to maintain for multiple-language versions of the app, or if you need to change the string in the future. After extracting the string, use the string resource name with `@string/` to specify the text:

```markup
<TextView
   android:layout_width="wrap_content"
   android:layout_height="wrap_content"
   android:text="@string/hello_world"
   <!-- more attributes -->
/>
```

In addition to `android:layout_width` and `android:layout_height` \(which are required for a `TextView`\), the most often used attributes with `TextView` are the following:

* [`android:text`](https://developer.android.com/reference/android/widget/TextView.html#attr_android:text): Set the text to display.
* [`android:textColor`](https://developer.android.com/reference/android/widget/TextView.html#attr_android:textColor): Set the color of the text. You can set the attribute to a color value, a predefined resource, or a theme. Color resources and themes are described in other chapters.
* [`android:textAppearance`](https://developer.android.com/reference/android/widget/TextView.html#attr_android:textAppearance): The appearance of the text, including its color, typeface, style, and size. You set this attribute to a predefined style resource or theme that already defines these values.
* [`android:textSize`](https://developer.android.com/reference/android/widget/TextView.html#attr_android:textSize): Set the text size \(if not already set by `android:textAppearance`\). Use `sp` \(scaled-pixel\) sizes such as `20sp` or `14.5sp`, or set the attribute to a predefined resource or theme.
* [`android:textStyle`](https://developer.android.com/reference/android/widget/TextView.html#attr_android:textStyle): Set the text style \(if not already set by `android:textAppearance`\). Use `normal`, `bold`, `italic`, or `bold`\|`italic`.
* [`android:typeface`](https://developer.android.com/reference/android/widget/TextView.html#attr_android:typeface): Set the text typeface \(if not already set by `android:textAppearance`\). Use `normal`, `sans`, `serif`, or `monospace`.
* [`android:lineSpacingExtra`](https://developer.android.com/reference/android/widget/TextView.html#attr_android:lineSpacingExtra): Set extra spacing between lines of text. Use `sp` \(scaled-pixel\) or dp \(device-independent pixel\) sizes, or set the attribute to a predefined resource or theme.
* [`android:autoLink`](https://developer.android.com/reference/android/widget/TextView.html#attr_android:autoLink): Controls whether links such as URLs and email addresses are automatically found and converted to clickable \(touchable\) links.

Use one of the following with `android:autoLink`:

* `none`: Match no patterns \(default\).
* `web`: Match web URLs.
* `email`: Match email addresses.
* `phone`: Match phone numbers.
* `map`: Match map addresses.
* `all`: Match all patterns \(equivalent to web\|email\|phone\|map\).

For example, to set the attribute to match web URLs, use `android:autoLink="web"`.

## Referring to a TextView in code

To refer to a `TextView` in your Java code, use its resource `id`. For example, to update a `TextView` with new text, you would:

1. Find the `TextView` and assign it to a variable. You use the [`findViewById()`](https://developer.android.com/reference/android/view/View.html#findViewById%28int%29) method of the `View` class, and refer to the view you want to find using this format:

   ```java
    R.id.view_id
   ```

   In which _view\_id_ is the resource identifier for the view \(such as `show_count`\) :

   ```java
   mShowCount = (TextView) findViewById(R.id.show_count);
   ```

2. After retrieving the `View` as a `TextView` member variable, you can then set the text to new text \(in this case, `mCount_text`\) using the [`setText()`](https://developer.android.com/reference/android/widget/TextView.html#setText%28java.lang.CharSequence%29) method of the `TextView` class:

   ```java
    mShowCount.setText(mCount_text)
   ```

