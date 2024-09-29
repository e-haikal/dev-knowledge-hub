# **Sending Data with Parcelable in Kotlin (Beginner-Friendly Guide)**

In Android development, it's common to pass data between activities. This tutorial will focus on how to send complex data, like objects, using Parcelable in Kotlin. Parcelable is an efficient method provided by Android to serialize objects so they can be passed between components like activities and fragments.

## **When to Use Parcelable?**
Use Parcelable when:
- You need to send custom objects (such as data models) between activities or fragments.
- Your object contains multiple fields, including primitive types (e.g., `Int`, `String`), and you want efficient serialization.
- You're building Android apps where memory and speed are crucial because Parcelable is faster than Java’s Serializable.

**Common use cases**:
1. **Navigating between activities**: Send user data, settings, or a configuration object from one activity to another.
2. **Passing data in fragments**: Share data between fragments in the same activity.

If you're only sending primitive types (like `Int`, `Boolean`, or `String`), it's not necessary to use Parcelable. You can directly use Intents or Bundles for that. However, if you need to send complex objects, Parcelable is a better approach than Serializable for performance reasons.

---

### **Step-by-Step Guide**

### 1. Understanding Data Class in Kotlin

In Kotlin, a **Data Class** is used to hold data. It automatically generates functions like `toString()`, `equals()`, and `copy()` for you.

#### **Kotlin Data Class Example**:
```kotlin
data class Person(
    val name: String?,
    val age: Int?,
    val email: String?,
    val city: String?
)
```
This `Person` class holds user information, making it easy to send this structured data between activities.

### 2. Implementing Parcelable in Kotlin

When you need to send a custom object (like `Person`) between activities, Parcelable is the recommended way in Android due to its efficiency.

#### **What Types Can You Use with Parcelable?**
Parcelable supports:
- **Primitive types**: `Int`, `Long`, `Boolean`, `Double`, etc.
- **Strings and CharSequence**.
- **Other Parcelable objects** (if you have objects inside objects).
- **Lists and Arrays** of Parcelable types.

For complex data (like a list of objects), ensure each object also implements Parcelable.

#### **How to Implement Parcelable in Kotlin:**

Use the `@Parcelize` annotation to make your data class implement Parcelable. This eliminates the need to write a lot of boilerplate code.

**Updated Person Class with Parcelable**:
```kotlin
import android.os.Parcelable
import kotlinx.parcelize.Parcelize

@Parcelize
data class Person(
    val name: String?,
    val age: Int?,
    val email: String?,
    val city: String?
) : Parcelable
```
Here, `Person` implements `Parcelable`, meaning we can now pass `Person` objects between activities.

---

### 3. Adding Parcelable Support to Your Project

To use the `@Parcelize` annotation, you need to enable the Kotlin Parcelize plugin.

#### **Steps**:
1. Open the `build.gradle.kts` file (Module: app).
2. Add the `kotlin-parcelize` plugin:
```kotlin
plugins {
    id("com.android.application")
    id("org.jetbrains.kotlin.android")
    id("kotlin-parcelize")  // Enable Parcelize plugin
}
```
3. Sync your project by clicking "Sync Now."

---

### 4. Creating a Layout with Buttons

Now, we need a basic UI layout with a button to trigger the activity transition where we send the `Person` object.

#### **activity_main.xml**:
```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <Button
        android:id="@+id/btn_move_activity_object"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="16dp"
        android:text="Move with Object" />
</LinearLayout>
```

### 5. Sending Data with Intent

When the user clicks the button, we will create a `Person` object and send it to another activity using `Intent`.

#### **MainActivity.kt**:
```kotlin
import android.content.Intent
import android.os.Bundle
import android.view.View
import android.widget.Button
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity(), View.OnClickListener {

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val btnMoveWithObject: Button = findViewById(R.id.btn_move_activity_object)
        btnMoveWithObject.setOnClickListener(this)
    }

    override fun onClick(v: View?) {
        when (v?.id) {
            R.id.btn_move_activity_object -> {
                // Create a Person object to send
                val person = Person(
                    "John Doe",
                    25,
                    "john.doe@example.com",
                    "New York"
                )
                // Create an intent to start MoveWithObjectActivity
                val moveWithObjectIntent = Intent(this, MoveWithObjectActivity::class.java)
                // Attach the Person object to the intent using putExtra
                moveWithObjectIntent.putExtra(MoveWithObjectActivity.EXTRA_PERSON, person)
                // Start the new activity
                startActivity(moveWithObjectIntent)
            }
        }
    }
}
```

---

### 6. Receiving Data in the Target Activity

In the target activity, we will retrieve the `Person` object and display its data.

#### **MoveWithObjectActivity.kt**:
```kotlin
import android.os.Build
import android.os.Bundle
import android.widget.TextView
import androidx.appcompat.app.AppCompatActivity

class MoveWithObjectActivity : AppCompatActivity() {

    companion object {
        const val EXTRA_PERSON = "extra_person"
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_move_with_object)

        val tvObject: TextView = findViewById(R.id.tv_object_received)

        // Retrieve the Person object from the intent
        val person = if (Build.VERSION.SDK_INT >= 33) {
            intent.getParcelableExtra(EXTRA_PERSON, Person::class.java)
        } else {
            @Suppress("DEPRECATION")
            intent.getParcelableExtra<Person>(EXTRA_PERSON)
        }

        // Display the Person object data
        person?.let {
            val text = "Name: ${it.name}\nEmail: ${it.email}\nAge: ${it.age}\nLocation: ${it.city}"
            tvObject.text = text
        }
    }
}
```

#### **activity_move_with_object.xml**:
```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp">

    <TextView
        android:id="@+id/tv_object_received"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Object Received" />
</RelativeLayout>
```

---

### 7. Running the App

1. When you click the "Move with Object" button in the `MainActivity`, a `Person` object will be sent to `MoveWithObjectActivity`.
2. The `MoveWithObjectActivity` will display the object’s details, such as name, email, age, and city.

---

### **What Types of Data Should Be Passed Using Parcelable?**
- **Use Parcelable** for passing custom data objects between activities/fragments.
- **Use Bundles/Intents directly** if you're only sending primitive types (`Int`, `String`, `Boolean`, etc.).
- **Avoid large objects or sensitive data** as Parcelable is stored in memory and can lead to performance issues with large data.

---

### **Key Notes for Beginners**:
- **Parcelable vs Serializable**: Parcelable is faster and more optimized for Android than Serializable.
- **Data Types Supported**: Parcelable can handle primitive types, strings, arrays, and other parcelable objects.
- **@Parcelize** annotation**: This helps you implement Parcelable without writing boilerplate code.

### Reference
See tutorials from [Dicoding](https://www.dicoding.com/academies/51/tutorials/29095?from=1197).