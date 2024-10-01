# Debugging with Logging and Debug Breakpoints

## Objective

When developing Android applications, one of the most challenging tasks is identifying and resolving bugs. The diversity of Android device specifications often complicates debugging. While your application may run smoothly on one device, it might encounter issues on another. This article covers essential debugging techniques to help identify and fix bugs in Android apps.

## Practice Flow

In this exercise, we will cover:
1. Causing the application to crash.
2. Reading errors in Logcat.
3. Displaying logs.
4. Using debug breakpoints.

## Codelab: Debugging

### Step 1: Create a New Android Project

1. **Project Name**: MyTestingApp
2. **Target & Minimum Target SDK**: Phone and Tablet, API level 21
3. **Activity Type**: Empty Activity
4. **Activity Name**: MainActivity
5. **Use AndroidX Artifacts**: Yes
6. **Language**: Kotlin
7. **Build Configuration Language**: Kotlin DSL

### Step 2: Set Up the UI

Update the `activity_main.xml` layout to use a `RelativeLayout` with a `TextView` and a `Button`:

```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/activity_main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <TextView
        android:id="@+id/tv_text"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/hello_world" />

    <Button
        android:id="@+id/btn_set_value"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_below="@id/tv_text"
        android:text="@string/set_value" />

</RelativeLayout>
```

Add the following string resources in `res/values/strings.xml`:

```xml
<resources>
    <string name="app_name">MyTestingApp</string>
    <string name="set_value">Set Value</string>
    <string name="hello_world">Hello World!</string>
</resources>
```

### Step 3: Introduce a Crash

In `MainActivity`, write code that intentionally causes a **NullPointerException**:

```kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {
    private var btnSetValue: Button? = null
    private lateinit var tvText: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        tvText = findViewById(R.id.tv_text)
        btnSetValue!!.setOnClickListener(this)  // Intentional NullPointerException
    }

    override fun onClick(view: View) {
        if (view.id == R.id.btn_set_value) {
            tvText.text = "19"
        }
    }
}
```

### Step 4: Analyze the Error in Logcat

When you run the app and click the **Set Value** button, the app will crash. Here’s what you’ll see in **Logcat**:

```
java.lang.RuntimeException: Unable to start activity ComponentInfo{com.dicoding.mytestingapp/com.dicoding.mytestingapp.MainActivity}: java.lang.NullPointerException
...
Caused by: java.lang.NullPointerException
    at com.dicoding.mytestingapp.MainActivity.onCreate(MainActivity.kt:18)
```

#### Key Concepts:
- **NullPointerException** occurs because `btnSetValue` was never initialized (it is null).
- **Logcat** provides detailed logs for debugging. Always monitor Logcat for helpful information when an app crashes.

### Step 5: Fix the Crash

Modify the `MainActivity` code to correctly initialize `btnSetValue`:

```kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {

    private lateinit var btnSetValue: Button
    private lateinit var tvText: TextView

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Correctly initialize the Button and TextView
        tvText = findViewById(R.id.tv_text)
        btnSetValue = findViewById(R.id.btn_set_value)

        // Set the click listener for the button
        btnSetValue.setOnClickListener(this)
    }

    override fun onClick(view: View) {
        if (view.id == R.id.btn_set_value) {
            tvText.text = "19"
        }
    }
}
```

### Step 6: Display Logs for Debugging

Add logging in `MainActivity` to help trace the execution flow:

```kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {

    private lateinit var btnSetValue: Button
    private lateinit var tvText: TextView

    private val names = arrayListOf("Narenda", "Kevin", "Yoza")

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        tvText = findViewById(R.id.tv_text)
        btnSetValue = findViewById(R.id.btn_set_value)

        btnSetValue.setOnClickListener(this)
        Log.d("MainActivity", "Names initialized: $names")
    }

    override fun onClick(view: View) {
        if (view.id == R.id.btn_set_value) {
            Log.d("MainActivity", "Button clicked")
            val nameBuilder = StringBuilder()
            for (i in 0..2) {  // Corrected loop range to avoid IndexOutOfBoundsException
                nameBuilder.append(names[i]).append("\n")
            }
            tvText.text = nameBuilder.toString()
        }
    }
}
```

#### Logcat Variations:
- `Log.d()` for debug messages.
- `Log.e()` for error messages.
- `Log.w()` for warnings.
- `Log.i()` for informational messages.
- `Log.v()` for verbose messages.

### Step 7: Set Debug Breakpoints

Debugging is more effective when combined with **breakpoints**. Set a breakpoint in Android Studio by clicking on the gutter next to the line number. Example of a breakpoint set on the following line:

```kotlin
Log.d("MainActivity", "Button clicked")
```

When the app pauses at this breakpoint, use the **Debug tab** to inspect variable values.

### Safety and Compatibility Notes

- Always ensure proper initialization of variables before using them to avoid **NullPointerException**.
- Carefully set loop ranges to avoid **IndexOutOfBoundsException**, especially when working with collections.
- **Logging** should be removed or minimized in production apps to avoid performance issues or leaking sensitive information.

---

## Reusable Patterns

- **Logging**: Use logging (`Log.d()`, `Log.e()`, etc.) strategically during debugging to trace code execution.
- **Breakpoints**: Set breakpoints to pause and inspect the app at critical points.
- **Error Handling**: Use try-catch blocks to handle errors like `NullPointerException` and `IndexOutOfBoundsException`.

## Conclusion

Debugging is an essential skill for Android developers. By understanding how to read logs in **Logcat** and using **debug breakpoints**, you can quickly identify and resolve issues in your app. Always keep logs meaningful, use breakpoints for deep inspection, and ensure variables are initialized properly to avoid common errors.

---

This version follows the best practices you outlined:
- **Applied Best Practices**: Structured examples with logs and breakpoints.
- **Beginner-Friendly**: Clear explanations with comments.
- **Multiple Use Cases**: Showed both logging and breakpoints.
- **Logical Structure**: Each section builds on the last.
- **Practical Focus**: Debugging techniques you can apply directly.
- **Key Concepts Highlighted**: Logging, crash analysis, debugging.
- **Safety Notes**: Warned about null variables and logging in production.
- **Reusable Patterns**: Logging and breakpoint strategies.

#### **Reference**:
1. Dicoding Indonesia
2. Improved by GPT