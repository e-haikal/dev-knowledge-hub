# Simulating Asynchronous Processes in Android with Kotlin Coroutines

In this comprehensive tutorial, we will explore how to use Kotlin Coroutines to manage background tasks in an Android application. By the end of this guide, you will have built a simple app that simulates a long-running task and updates the user interface (UI) to show progress. This tutorial is designed for beginners, so each step is explained clearly and simply.

## Table of Contents
1. [What Are Coroutines?](#what-are-coroutines)
2. [When to Use Coroutines](#when-to-use-coroutines)
3. [Project Setup](#project-setup)
4. [Add Dependencies](#add-dependencies)
5. [Enable View Binding](#enable-view-binding)
6. [Creating the User Interface](#creating-the-user-interface)
7. [Implementing the Background Task](#implementing-the-background-task)
8. [Running the Application](#running-the-application)
9. [Summary of Key Concepts](#summary-of-key-concepts)

---

## 1. What Are Coroutines?

Coroutines are a way to write asynchronous code in a sequential manner. They allow you to perform tasks that may take time (like network requests or file operations) without blocking the main thread, which keeps your app responsive. In simpler terms, coroutines let you do multiple things at once without making your app feel sluggish.

### Analogy
Think of coroutines like a waiter at a restaurant. Instead of standing still and waiting for one customer to finish ordering before moving to the next, the waiter takes orders from multiple tables simultaneously. This way, everyone gets served in a timely manner.

---

## 2. When to Use Coroutines

You should consider using coroutines in the following scenarios:
- **Network Requests**: Fetching data from the internet can take time. Using coroutines prevents the UI from freezing while waiting for a response.
- **Database Operations**: Reading from or writing to a database can also be time-consuming. Coroutines help keep the app responsive during these operations.
- **Heavy Computation**: Performing tasks that require a lot of processing power (like image processing) should be done in a background thread.

---

## 3. Project Setup

### Step 1: Create a New Project
1. Open Android Studio.
2. Click on **New Project**.
3. Choose **Empty Activity** and click **Next**.
4. Name your application (e.g., `MyBackgroundThread`), choose a package name, and select **Kotlin** as the language.
5. Click **Finish** to create your project.

---

## 4. Add Dependencies

Kotlin Coroutines are included in the standard library, but you need to add specific dependencies for Android.

### Step 2: Open the `build.gradle` (Module: app) File
1. In the **Project** view, navigate to `app/build.gradle`.
2. Add the following dependencies inside the `dependencies` block:

```groovy
dependencies {
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-core:1.7.3'
    implementation 'org.jetbrains.kotlinx:kotlinx-coroutines-android:1.7.3'
}
```

### Step 3: Sync the Project
- After adding the dependencies, click **Sync Now** in the notification bar to download the required libraries.

---

## 5. Enable View Binding

View Binding simplifies accessing UI elements in your layouts, making your code cleaner.

### Step 4: Enable View Binding
1. Open the `build.gradle` (Module: app) file.
2. Inside the `android` block, add the following line:

```groovy
android {
    ...
    viewBinding {
        enabled = true
    }
}
```

3. Sync the project again.

---

## 6. Creating the User Interface

Next, we’ll design the layout for our application. The layout will have a button to start the task and a text view to display the progress.

### Step 5: Create the Layout File
1. In the **Project** view, navigate to `res/layout`.
2. Open `activity_main.xml` and replace its content with the following:

```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    tools:context=".MainActivity">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:gravity="center"
        android:orientation="vertical"
        android:padding="16dp">

        <Button
            android:id="@+id/btn_start"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="300dp"
            android:text="@string/start_task" />

        <TextView
            android:id="@+id/tv_status"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            tools:text="@string/status" />

        <TextView
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="400dp"
            android:text="@string/hidden_text" />
    </LinearLayout>
</ScrollView>
```

### Explanation
- **`ScrollView`**: Allows the user to scroll if the content is larger than the screen.
- **`LinearLayout`**: Arranges its child views in a single column.
- **`Button`**: Starts the background task when clicked.
- **`TextView`**: Displays the current status of the task.

---

## 7. Implementing the Background Task

Now, let’s implement the logic for the background task in the `MainActivity.kt` file.

### Step 6: Update MainActivity.kt

1. Open `MainActivity.kt` in the `java` directory.
2. Replace its content with the following code:

```kotlin
package com.siaptekno.mybackgroundthread

import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import com.siaptekno.mybackgroundthread.databinding.ActivityMainBinding
import kotlinx.coroutines.Dispatchers
import kotlinx.coroutines.delay
import kotlinx.coroutines.launch
import kotlinx.coroutines.withContext

class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding  // View Binding reference

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()  // Makes the app use the full screen
        binding = ActivityMainBinding.inflate(layoutInflater)  // Initialize binding
        setContentView(binding.root)  // Set the content view using binding

        // Set up a click listener for the button
        binding.btnStart.setOnClickListener {
            startCompressionTask()  // Start the background task
        }
    }

    private fun startCompressionTask() {
        lifecycleScope.launch(Dispatchers.Default) {  // Start a coroutine in the background
            for (i in 0..10) {  // Loop from 0 to 10
                delay(500)  // Simulate a task taking time (500 milliseconds)
                val percentage = (i + 1) * 10  // Calculate progress percentage

                // Switch to the main thread to update the UI
                withContext(Dispatchers.Main) {
                    if (percentage == 100) {
                        binding.tvStatus.setText(R.string.task_completed)  // Update status to completed
                    } else {
                        binding.tvStatus.text = String.format(getString(R.string.compressing), percentage)  // Update status with progress
                    }
                }
            }
        }
    }
}
```

### Explanation
- **View Binding**: We use `ActivityMainBinding` to easily access UI elements without needing `findViewById()`.
- **`lifecycleScope`**: This runs the coroutine safely tied to the activity's lifecycle, meaning it will be canceled if the activity is destroyed.
- **`Dispatchers.Default`**: Executes the coroutine in a background thread, suitable for heavy tasks.
- **`delay(500)`**: Simulates work being done for half a second.
- **`withContext(Dispatchers.Main)`**: Ensures that UI updates occur on the main thread, which is essential for safe UI manipulation.

---

## 8. Running the Application

Now that we have our code and layout ready, it’s time to run the application.

### Step 7: Run Your App
1. Connect your Android device or start an emulator.
2. Click the **Run** button in Android Studio.
3. Once the app starts, tap the **Start Task** button.
4. Observe the status text update as the simulated task progresses.

---

## 9. Summary of Key Concepts

- **Kotlin Coroutines**: An elegant way to handle asynchronous programming, allowing for non-blocking operations.
- **When to Use**: Ideal for network requests, database operations, or any task that could cause delays.
- **Lifecycle Management**: Using `lifecycleScope` ensures that coroutines are automatically canceled when the activity is destroyed, preventing memory leaks.
- **UI Thread Management**: Always switch back to the main thread for UI updates to avoid crashes.
- **View Binding**: Streamlines accessing UI elements and reduces boilerplate code.

With this guide, you now have a foundational understanding of how to use coroutines in Android. You can expand on this example by integrating actual network requests, database operations, or other heavy processing tasks

Enjoy coding!

#### Reference:
1. Dicoding Indonesia
2. Explained by GPT