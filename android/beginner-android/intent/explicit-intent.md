# Understanding Explicit Intents in Kotlin Android

Explicit Intents in Android allow you to navigate from one activity to another or trigger specific components within your app.

## What is an Explicit Intent?

An Explicit Intent specifies the exact activity or component that should handle the request. It is typically used when you want to navigate from one screen to another within your app.

### Basic Example: Navigating Between Activities

Let’s say you have two activities: `MainActivity` and `SecondActivity`. Here’s how you can use an Explicit Intent to move from `MainActivity` to `SecondActivity`.

### Step-by-Step Guide:

1. **Define the Intent in the MainActivity:**
   When a button is clicked, an intent is used to explicitly state the target activity to navigate to.

   ```kotlin
   package com.siaptekno.myintentapp

   import android.content.Intent
   import android.os.Bundle
   import android.view.View
   import android.widget.Button
   import androidx.appcompat.app.AppCompatActivity

   class MainActivity : AppCompatActivity(), View.OnClickListener {
       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
           setContentView(R.layout.activity_main)

           // Find the button in the layout
           val moveActivityButton = findViewById<Button>(R.id.moveActivityButton)
           moveActivityButton.setOnClickListener(this)
       }

       // Define click event for the button
       override fun onClick(v: View?) {
           when (v?.id) {
               R.id.moveActivityButton -> {
                   // Explicit Intent to move from MainActivity to SecondActivity
                   val moveIntent = Intent(this@MainActivity, SecondActivity::class.java)
                   startActivity(moveIntent) // Starts the SecondActivity
               }
           }
       }
   }
   ```

2. **Create the Target Activity (SecondActivity):**
   In the `SecondActivity`, display some text to confirm the navigation.

   ```kotlin
   package com.siaptekno.myintentapp

   import android.os.Bundle
   import androidx.appcompat.app.AppCompatActivity

   class SecondActivity : AppCompatActivity() {
       override fun onCreate(savedInstanceState: Bundle?) {
           super.onCreate(savedInstanceState)
           setContentView(R.layout.activity_second) // Layout for SecondActivity
       }
   }
   ```

3. **Update the AndroidManifest.xml:**
   Don’t forget to declare the second activity in your AndroidManifest.

   ```xml
   <activity android:name=".SecondActivity" />
   ```

With this setup, clicking the button in `MainActivity` triggers an explicit intent, navigating the user to `SecondActivity`.

### Passing Data Between Activities

Often, you need to pass data to the new activity. You can do this using the `putExtra` method.

**Example: Passing a String to SecondActivity**

1. **Modify the Intent to Add Data:**

   ```kotlin
   val moveIntent = Intent(this@MainActivity, SecondActivity::class.java)
   moveIntent.putExtra("EXTRA_MESSAGE", "Hello from MainActivity")
   startActivity(moveIntent)
   ```

2. **Receive the Data in SecondActivity:**

   In `SecondActivity`, you can retrieve the data sent via the intent.

   ```kotlin
   val message = intent.getStringExtra("EXTRA_MESSAGE")
   // Now you can display this message, e.g., in a TextView
   ```

### Different Approach: Navigating and Receiving Results

Explicit Intents can also be used to start an activity and get results back. Here’s how you can achieve this using `startActivityForResult`.

1. **Start the Activity for Result in MainActivity:**

   ```kotlin
   val moveIntent = Intent(this@MainActivity, SecondActivity::class.java)
   startActivityForResult(moveIntent, REQUEST_CODE)
   ```

2. **Return the Result from SecondActivity:**

   In `SecondActivity`, set the result before finishing the activity.

   ```kotlin
   val resultIntent = Intent()
   resultIntent.putExtra("RESULT_DATA", "Data from SecondActivity")
   setResult(RESULT_OK, resultIntent)
   finish() // Ends SecondActivity
   ```

3. **Handle the Result in MainActivity:**

   Override `onActivityResult` in `MainActivity` to capture the result.

   ```kotlin
   override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
       super.onActivityResult(requestCode, resultCode, data)
       if (requestCode == REQUEST_CODE && resultCode == RESULT_OK) {
           val resultData = data?.getStringExtra("RESULT_DATA")
           // Use the resultData, e.g., display it in a TextView
       }
   }
   ```

### Using `startActivityForResult` Deprecated Notice
As of Android API level 30, `startActivityForResult` has been deprecated. The new `ActivityResultLauncher` should be used instead. 

Here’s a simplified example:

1. **Declare the Launcher:**

   ```kotlin
   private val resultLauncher = registerForActivityResult(ActivityResultContracts.StartActivityForResult()) { result ->
       if (result.resultCode == Activity.RESULT_OK) {
           val resultData = result.data?.getStringExtra("RESULT_DATA")
           // Handle the result
       }
   }
   ```

2. **Launch the SecondActivity:**

   ```kotlin
   val intent = Intent(this, SecondActivity::class.java)
   resultLauncher.launch(intent)
   ```

### Conclusion

Explicit Intents are an essential part of Android development, allowing you to navigate between activities and pass data in a clear and structured way. By following the examples above, you should be able to implement explicit intents and navigate within your Android app effectively.

Remember to always declare your activities in the AndroidManifest file and use `putExtra` to pass data between them. When dealing with results, prefer the newer `ActivityResultLauncher` for a cleaner, modern approach.