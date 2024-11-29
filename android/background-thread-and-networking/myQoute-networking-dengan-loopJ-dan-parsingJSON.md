

# MyQuote App: A Beginner's Guide to Networking with LoopJ & JSON Parsing

In this tutorial, you will learn how to create a simple Android app called **MyQuote**. This app fetches random quotes and displays them using the LoopJ library for networking and JSON for data parsing. The instructions are designed to be easy to follow, even if you're new to Android development.

## Table of Contents

1. [Introduction to LoopJ](#introduction-to-loopj)
2. [Setting Up the Project](#setting-up-the-project)
3. [Creating the Main Activity](#creating-the-main-activity)
   - 3.1 [Fetching a Random Quote](#fetching-a-random-quote)
4. [Creating the Quote Adapter](#creating-the-quote-adapter)
5. [Listing Quotes Activity](#listing-quotes-activity)
   - 5.1 [Fetching the List of Quotes](#fetching-the-list-of-quotes)
6. [XML Layout Files](#xml-layout-files)
7. [Summary of Key Concepts](#summary-of-key-concepts)

## Introduction to LoopJ

**LoopJ** is a library that simplifies HTTP networking in Android. Imagine it as a waiter in a restaurant: you place your order (make a request), and the waiter brings your food (response) without you having to go into the kitchen (deal with the underlying network details).

## Setting Up the Project

### Step 1: Create a New Project

1. Open Android Studio and create a new project.
2. Name it **MyQuote** and choose an empty activity.

### Step 2: Add LoopJ Dependency

Open the `build.gradle` file (Module: app) and add the following line in the `dependencies` section:

```groovy
implementation 'com.loopj.android:android-async-http:1.4.10'
```

Sync your project to download the library.

### Step 3: Enable View Binding

In your `build.gradle` file, add the following under the `android` block:

```groovy
viewBinding {
    enabled = true
}
```

Sync the project again.

## Creating the Main Activity

The main activity will fetch a random quote when the app starts.

### Step 1: Create MainActivity

Create a new Kotlin file named `MainActivity.kt` and add the following code:

```kotlin
package com.siaptekno.myquote

import android.content.Intent
import android.os.Bundle
import android.util.Log
import android.view.View
import android.widget.Toast
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import com.loopj.android.http.AsyncHttpClient
import com.loopj.android.http.AsyncHttpResponseHandler
import com.siaptekno.myquote.databinding.ActivityMainBinding
import cz.msebera.android.httpclient.Header
import org.json.JSONObject

class MainActivity : AppCompatActivity() {

    private lateinit var binding: ActivityMainBinding  // View Binding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()  // For full-screen mode
        binding = ActivityMainBinding.inflate(layoutInflater)  // Inflate layout
        setContentView(binding.root)

        // Set padding for system UI
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }

        getRandomQuote()  // Fetch a random quote when the app starts

        // Navigate to list of quotes when button is clicked
        binding.btnAllQuotes.setOnClickListener {
            startActivity(Intent(this@MainActivity, ListQuotesActivity::class.java))
        }
    }

    // Fetch a random quote from the API
    private fun getRandomQuote() {
        binding.progressBar.visibility = View.VISIBLE  // Show loading spinner
        val client = AsyncHttpClient()
        val url = "https://quote-api.dicoding.dev/random"  // API endpoint
        
        client.get(url, object : AsyncHttpResponseHandler() {
            override fun onSuccess(statusCode: Int, headers: Array<Header>, responseBody: ByteArray) {
                binding.progressBar.visibility = View.INVISIBLE  // Hide loading spinner
                val result = String(responseBody)  // Convert response to String
                Log.d("MainActivity", result)  // Log the response
                try {
                    val responseObject = JSONObject(result)  // Parse JSON
                    val quote = responseObject.getString("en")  // Get quote text
                    val author = responseObject.getString("author")  // Get author name
                    binding.tvQuote.text = quote  // Set quote text
                    binding.tvAuthor.text = author  // Set author text
                } catch (e: Exception) {
                    Toast.makeText(this@MainActivity, e.message, Toast.LENGTH_SHORT).show()  // Show error
                }
            }

            override fun onFailure(statusCode: Int, headers: Array<out Header>, responseBody: ByteArray, error: Throwable) {
                binding.progressBar.visibility = View.INVISIBLE  // Hide loading spinner
                val errorMessage = when (statusCode) {
                    401 -> "$statusCode : Bad Request"
                    403 -> "$statusCode : Forbidden"
                    404 -> "$statusCode : Not Found"
                    else -> "$statusCode : ${error.message}"
                }
                Toast.makeText(this@MainActivity, errorMessage, Toast.LENGTH_SHORT).show()  // Show error
            }
        })
    }
}
```

### Explanation

- **View Binding**: This allows easier access to UI components without using `findViewById()`.
- **getRandomQuote() Method**: This method fetches a random quote from the API, parses the JSON response, and updates the UI.
- **Error Handling**: It gracefully handles errors and informs the user if something goes wrong.

## Creating the Quote Adapter

To display a list of quotes, we need an adapter to manage how each quote is displayed.

### Step 1: Create QuoteAdapter

Create a new Kotlin file named `QuoteAdapter.kt` and add the following code:

```kotlin
package com.siaptekno.myquote

import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.recyclerview.widget.RecyclerView

class QuoteAdapter(private val listReview: ArrayList<String>) : RecyclerView.Adapter<QuoteAdapter.ViewHolder>() {

    // Create new views
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): ViewHolder {
        val view = LayoutInflater.from(parent.context).inflate(R.layout.item_quote, parent, false)  // Inflate layout
        return ViewHolder(view)  // Return the view holder
    }

    // Bind data to views
    override fun onBindViewHolder(viewHolder: ViewHolder, position: Int) {
        viewHolder.tvItem.text = listReview[position]  // Set the quote text for each item
    }

    override fun getItemCount(): Int {
        return listReview.size  // Return the size of the quotes list
    }

    // ViewHolder class to hold the view
    class ViewHolder(view: View) : RecyclerView.ViewHolder(view) {
        val tvItem: TextView = view.findViewById(R.id.tvItem)  // Reference to quote text view
    }
}
```

### Explanation

- **ViewHolder**: This class holds references to the views for each quote, improving performance by avoiding repeated calls to `findViewById()`.
- **Adapter Methods**: The `onCreateViewHolder`, `onBindViewHolder`, and `getItemCount` methods manage how data is displayed in the RecyclerView.

## Listing Quotes Activity

Next, we’ll create an activity to list multiple quotes.

### Step 1: Create ListQuotesActivity

Create a new Kotlin file named `ListQuotesActivity.kt` and add the following code:

```kotlin
package com.siaptekno.myquote

import android.os.Bundle
import android.util.Log
import android.view.View
import android.widget.Toast
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import androidx.recyclerview.widget.DividerItemDecoration
import androidx.recyclerview.widget.LinearLayoutManager
import com.loopj.android.http.AsyncHttpClient
import com.loopj.android.http.AsyncHttpResponseHandler
import com.siaptekno.myquote.databinding.ActivityListQuotesBinding
import cz.msebera.android.httpclient.Header
import org.json.JSONArray

class ListQuotesActivity : AppCompatActivity() {

    private lateinit var binding: ActivityListQuotesBinding  // View Binding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge()  // For full-screen mode
        binding = ActivityListQuotesBinding.inflate(layoutInflater)  // Inflate layout
        setContentView(binding.root)

        // Set up the RecyclerView
        val layoutManager = LinearLayoutManager(this)  // Create layout manager
        binding.listQuotes.layoutManager = layoutManager  // Set layout manager to RecyclerView
        val itemDecoration = DividerItemDecoration(this, layoutManager.orientation)  // Add item decoration
        binding.listQuotes.addItemDecoration(itemDecoration)  // Attach decoration to RecyclerView

        getListQuotes()  // Fetch the list of quotes
    }

    // Fetch list of quotes from the API


    private fun getListQuotes() {
        binding.progressBar.visibility = View.VISIBLE  // Show loading spinner
        val client = AsyncHttpClient()
        val url = "https://quote-api.dicoding.dev/quotes"  // API endpoint
        
        client.get(url, object : AsyncHttpResponseHandler() {
            override fun onSuccess(statusCode: Int, headers: Array<Header>, responseBody: ByteArray) {
                binding.progressBar.visibility = View.INVISIBLE  // Hide loading spinner
                val result = String(responseBody)  // Convert response to String
                Log.d("ListQuotesActivity", result)  // Log the response
                try {
                    val responseArray = JSONArray(result)  // Parse JSON array
                    val listQuotes = ArrayList<String>()  // Create a list for quotes
                    for (i in 0 until responseArray.length()) {
                        val quote = responseArray.getJSONObject(i).getString("en")  // Get each quote
                        listQuotes.add(quote)  // Add quote to the list
                    }
                    val adapter = QuoteAdapter(listQuotes)  // Create adapter with quotes list
                    binding.listQuotes.adapter = adapter  // Set adapter to RecyclerView
                } catch (e: Exception) {
                    Toast.makeText(this@ListQuotesActivity, e.message, Toast.LENGTH_SHORT).show()  // Show error
                }
            }

            override fun onFailure(statusCode: Int, headers: Array<out Header>, responseBody: ByteArray, error: Throwable) {
                binding.progressBar.visibility = View.INVISIBLE  // Hide loading spinner
                val errorMessage = when (statusCode) {
                    401 -> "$statusCode : Bad Request"
                    403 -> "$statusCode : Forbidden"
                    404 -> "$statusCode : Not Found"
                    else -> "$statusCode : ${error.message}"
                }
                Toast.makeText(this@ListQuotesActivity, errorMessage, Toast.LENGTH_SHORT).show()  // Show error
            }
        })
    }
}
```

### Explanation

- **RecyclerView Setup**: The `ListQuotesActivity` sets up a RecyclerView to display a list of quotes fetched from the API.
- **getListQuotes() Method**: This method fetches quotes, parses the JSON array, and populates the RecyclerView.

## XML Layout Files

Now let’s define the XML layout files for our activities.

### activity_main.xml

This file sets up the layout for the main activity:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:padding="16dp"
    tools:context=".MainActivity">

    <TextView
        android:id="@+id/tvQuote"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:textAlignment="center"
        android:textSize="18sp"
        android:textStyle="bold"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        tools:text="@string/quote" />

    <TextView
        android:id="@+id/tvAuthor"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:textAlignment="center"
        android:textSize="14sp"
        android:textStyle="italic"
        app:layout_constraintTop_toBottomOf="@+id/tvQuote"
        tools:text="@string/author" />

    <ProgressBar
        android:id="@+id/progressBar"
        style="?android:attr/progressBarStyle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <Button
        android:id="@+id/btnAllQuotes"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_margin="16dp"
        android:text="@string/show_list_quotes"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

### item_quote.xml

This file defines how each quote item looks in the list:

```xml
<?xml version="1.0" encoding="utf-8"?>
<TextView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/tvItem"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:gravity="center_vertical"
    android:minHeight="?android:attr/listPreferredItemHeightSmall"
    android:paddingStart="?android:attr/listPreferredItemPaddingStart"
    android:paddingEnd="?android:attr/listPreferredItemPaddingEnd"
    android:textAppearance="?android:attr/textAppearanceListItemSmall"
    tools:text="Quote" />
```

### activity_list_quotes.xml

This file sets up the layout for listing quotes:

```xml
<?xml version="1.0" encoding="utf-8"?>
<androidx.constraintlayout.widget.ConstraintLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/main"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context=".ListQuotesActivity">

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/listQuotes"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_constraintBottom_toTopOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />

    <ProgressBar
        android:id="@+id/progressBar"
        style="?android:attr/progressBarStyle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintTop_toTopOf="parent" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

## Summary of Key Concepts

1. **Networking with LoopJ**: A library that simplifies making HTTP requests in Android.
  
2. **JSON Parsing**: Using `JSONObject` and `JSONArray` to extract data from JSON responses.
  
3. **View Binding**: Allows for easier and safer access to UI components.
  
4. **RecyclerView**: A powerful widget for displaying a large list of items efficiently.
  
5. **Error Handling**: Essential for providing user feedback during network requests.

## Conclusion

In this tutorial, you built the **MyQuote** app, demonstrating how to use LoopJ for networking and JSON parsing in Android. This guide gives you a solid foundation for further exploration in Android development. Keep this document as a reference whenever you need it. If you have questions, feel free to ask!


#### Reference:
1. Dicoding Indonesia
2. Explained by GPT