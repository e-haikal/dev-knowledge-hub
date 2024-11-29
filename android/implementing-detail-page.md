# Building an Event Detail Page in Android Using Kotlin and Retrofit

This guide walks you through creating a detailed event page in an Android application using Kotlin and Retrofit. You will learn how to set up your API configuration, create an API service, implement the ViewModel, and build the UI to display event details. We will also ensure that your application can navigate to this detail page from an upcoming events list.

## Table of Contents
1. [API Configuration](#api-configuration)
2. [API Service Definition](#api-service-definition)
3. [Response Data Models](#response-data-models)
4. [Detail Event ViewModel](#detail-event-viewmodel)
5. [Detail Event Activity](#detail-event-activity)
6. [Upcoming Events Adapter](#upcoming-events-adapter)
7. [Activity Layout XML](#activity-layout-xml)
8. [Key Points](#key-points)

## 1. API Configuration

The `ApiConfig.kt` file sets up the Retrofit instance for making network requests. It uses an OkHttp client for HTTP requests and includes a logging interceptor for debugging.

```kotlin
package com.siaptekno.dicodingevent.data.retrofit

import okhttp3.OkHttpClient
import okhttp3.logging.HttpLoggingInterceptor
import retrofit2.Retrofit
import retrofit2.converter.gson.GsonConverterFactory

// ApiConfig class to configure API
class ApiConfig {
    companion object {
        // Create and return an instance of ApiService
        fun getApiService(): ApiService {
            // Logging interceptor to monitor HTTP requests and responses
            val loggingInterceptor = HttpLoggingInterceptor().setLevel(HttpLoggingInterceptor.Level.BODY)

            // Build the OkHttpClient
            val client = OkHttpClient.Builder()
                .addInterceptor(loggingInterceptor) // Add logging interceptor
                .build()

            // Build the Retrofit instance
            val retrofit = Retrofit.Builder()
                .baseUrl("https://event-api.dicoding.dev/") // Set base URL
                .addConverterFactory(GsonConverterFactory.create()) // JSON converter
                .client(client) // Set OkHttpClient
                .build()

            // Return ApiService instance
            return retrofit.create(ApiService::class.java)
        }
    }
}
```

### Key Points
- **OkHttpClient**: Handles HTTP requests.
- **HttpLoggingInterceptor**: Useful for debugging API calls.
- **Retrofit**: Facilitates API interaction.

## 2. API Service Definition

The `ApiService.kt` file defines the API endpoints used in the application.

```kotlin
package com.siaptekno.dicodingevent.data.retrofit

import com.siaptekno.dicodingevent.data.response.EventDetailResponse
import com.siaptekno.dicodingevent.data.response.EventResponse
import com.siaptekno.dicodingevent.data.response.ListEventsItem
import retrofit2.Call
import retrofit2.http.*

// Interface for defining API calls
interface ApiService {
    // Fetch a list of events with query parameters
    @GET("events") 
    fun getEvents(
        @Query("active") active: Int, // Filter active events
        @Query("limit") limit: Int = 40 // Limit the number of events returned
    ): Call<EventResponse> // API response type

    // Fetch event details by ID
    @GET("events/{id}")
    fun getEventById(
        @Path("id") eventId: Int // Path parameter for event ID
    ): Call<EventDetailResponse> // Single event response type
}
```

### Key Points
- **GET Requests**: Used for fetching data.
- **Path and Query Parameters**: Enable dynamic requests.

## 3. Response Data Models

The `EventDetailResponse.kt` file defines the data model for parsing the API response for event details.

```kotlin
package com.siaptekno.dicodingevent.data.response

import com.google.gson.annotations.SerializedName

// Response model for detailed event data
data class EventDetailResponse(
    @field:SerializedName("error") val error: Boolean,
    @field:SerializedName("message") val message: String,
    @field:SerializedName("event") val event: Event // Event data object
)

// Model for individual event details
data class Event(
    @field:SerializedName("summary") val summary: String,
    @field:SerializedName("mediaCover") val mediaCover: String,
    @field:SerializedName("registrants") val registrants: Int,
    @field:SerializedName("imageLogo") val imageLogo: String,
    @field:SerializedName("link") val link: String,
    @field:SerializedName("description") val description: String,
    @field:SerializedName("ownerName") val ownerName: String,
    @field:SerializedName("cityName") val cityName: String,
    @field:SerializedName("quota") val quota: Int,
    @field:SerializedName("name") val name: String,
    @field:SerializedName("id") val id: Int,
    @field:SerializedName("beginTime") val beginTime: String,
    @field:SerializedName("endTime") val endTime: String,
    @field:SerializedName("category") val category: String
)
```

### Key Points
- **Gson Annotations**: Maps JSON fields to Kotlin properties.
- **Data Models**: Structure data for easy access.

## 4. Detail Event ViewModel

The `DetailEventViewModel.kt` file handles data fetching and UI state management for the detail event screen.

```kotlin
package com.siaptekno.dicodingevent.ui.detail

import android.util.Log
import androidx.lifecycle.LiveData
import androidx.lifecycle.MutableLiveData
import androidx.lifecycle.ViewModel
import com.siaptekno.dicodingevent.data.response.Event
import com.siaptekno.dicodingevent.data.response.EventDetailResponse
import com.siaptekno.dicodingevent.data.retrofit.ApiConfig
import retrofit2.Call
import retrofit2.Callback
import retrofit2.Response

// ViewModel for managing event details
class DetailEventViewModel : ViewModel() {
    private val _detailEvents = MutableLiveData<Event>() // LiveData for event details
    val detailEvents: LiveData<Event> get() = _detailEvents // Expose immutable LiveData

    private val _isLoading = MutableLiveData<Boolean>() // LiveData for loading state
    val isLoading: LiveData<Boolean> get() = _isLoading // Expose immutable loading state

    // Function to fetch event details
    fun fetchDetailEvent(eventId: Int) {
        _isLoading.value = true // Set loading state
        val apiService = ApiConfig.getApiService() // Get ApiService instance
        apiService.getEventById(eventId).enqueue(object : Callback<EventDetailResponse> {
            override fun onResponse(call: Call<EventDetailResponse>, response: Response<EventDetailResponse>) {
                if (response.isSuccessful) {
                    _detailEvents.value = response.body()?.event // Update event details
                } else {
                    Log.e("DetailEventViewModel", "Error: ${response.message()}") // Log error
                }
                _isLoading.value = false // Reset loading state
            }

            override fun onFailure(call: Call<EventDetailResponse>, t: Throwable) {
                Log.e("DetailEventViewModel", "Failure: ${t.message}") // Log failure
                _isLoading.value = false // Reset loading state
            }
        })
    }
}
```

### Key Points
- **LiveData**: Observes changes in data to update the UI.
- **ViewModel**: Manages UI-related data in a lifecycle-conscious way.

## 5. Detail Event Activity

The `DetailEventActivity.kt` file is responsible for displaying the event details and handling user interactions.

```kotlin
package com.siaptekno.dicodingevent.ui.detail

import android.content.Intent
import android.net.Uri
import android.os.Bundle
import android.view.View
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.text.HtmlCompat
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import com.bumptech.glide.Glide
import com.siaptekno.dicodingevent.R
import com.siaptekno.dicodingevent.databinding.ActivityDetailEventBinding

class DetailEventActivity : AppCompatActivity(), View.OnClickListener {
    private lateinit var detailEventViewModel: DetailEventViewModel // ViewModel for event details
    private lateinit var binding: ActivityDetailEventBinding // View binding for the layout
    private var link: String? = null // Store event registration link

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityDetailEventBinding.inflate(layoutInflater)
        setContentView(binding.root)
        enableEdgeToEdge() // Enable edge-to-edge display

        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.activity_detail_event)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars()) // Handle insets
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }

        detailEventViewModel = DetailEventViewModel() // Initialize ViewModel
        val eventId = intent.getIntExtra("EXTRA_EVENT_ID", -1) // Retrieve event ID from intent

        if (eventId != -1) {
            detailEventViewModel.fetchDetailEvent(eventId) // Fetch event details
        }

        // Observe LiveData for event details
        detailEventViewModel.detailEvents.observe(this) { event ->
            event?.let {
                binding.tvDetailEventName.text = event.name // Set event name
                binding.tvDetailEventDesc.text = HtmlCompat.fromHtml(event.description, HtmlCompat.FROM_HTML_MODE_LEGACY) // Set event description
                binding.tvDetailEventLocation.text = getString(R.string.location, event.cityName) // Set event location
                binding.tvDetailEventOwner.text = getString(R.string.owner, event.ownerName) // Set event owner name
                binding.tvDetailEventQuota.text = getString(R.string.quota, event.quota.toString(), event.registrants.toString()) // Set quota information

                link = event.link // Store the registration link
                Glide.with(this) // Load event cover image
                    .load(event.mediaCover)
                    .into(binding.imgDetailEventCover)
                Glide.with(this) // Load event logo
                    .load(event.imageLogo)
                    .into(binding.imgDetailEventLogo)
            }
        }

        // Observe loading state
        detailEventViewModel.isLoading.observe(this) { isLoading ->
            binding.progressBar.visibility = if (isLoading) View.VISIBLE else View.GONE // Show or hide progress bar
        }

        binding.btnDetailEventRegister.setOnClickListener(this) // Set click listener for registration button
    }

    override fun onClick(v: View) {
        when (v.id) {
            R.id.btn_detail_event_register -> {
                link?.let {
                    // Open registration link in browser
                    val intent = Intent(Intent.ACTION_VIEW, Uri.parse(it))
                    startActivity(intent)
                }
            }
        }
    }
}
```

### Key Points
- **Glide**: Library for loading images efficiently.
- **HTML Rendering**: Allows rendering formatted text in TextViews.
- **Intents**: Used to open external links in a browser.

## 6. Upcoming Events Adapter

The `UpcomingEventsAdapter.kt` file is responsible for displaying the list of upcoming events and handling item clicks.

```kotlin
package com.siaptekno.dicodingevent.ui.upcoming

import android.content.Context
import android.content.Intent
import android.view.LayoutInflater
import android.view.ViewGroup
import androidx.recyclerview.widget.DiffUtil
import androidx.recyclerview.widget.ListAdapter
import androidx.recyclerview.widget.RecyclerView
import com.bumptech.glide.Glide
import com.siaptekno.dicodingevent.data.response.ListEventsItem
import com.siaptekno.dicodingevent.databinding.ItemUpcomingBinding
import com.siaptekno.dicodingevent.ui.detail.DetailEventActivity

// Adapter for displaying upcoming events in a RecyclerView
class UpcomingAdapter(private val context: Context) : ListAdapter<ListEventsItem, UpcomingAdapter.MyViewHolder>(DIFF_CALLBACK) {

    // ViewHolder to hold the views for each event item
    class MyViewHolder(val binding: ItemUpcomingBinding) : RecyclerView.ViewHolder(binding.root) {
        // Binds event data to the views in the layout
        fun bind(event: ListEventsItem?) {
            binding.tvUpcomingEventName.text = event?.name // Set event name

            // Load event image using Glide
            Glide.with(binding.ivUpcomingEventImage.context)
                .load(event?.mediaCover) // Load image URL
                .into(binding.ivUpcomingEventImage) // Display the image
        }
    }

    // Creates a new ViewHolder when needed
    override fun onCreateViewHolder(parent: ViewGroup, viewType: Int): MyViewHolder {
        // Inflate the item layout and create ViewHolder
        val binding = ItemUpcomingBinding.inflate(LayoutInflater.from(parent.context), parent, false)
        return MyViewHolder(binding)
    }

    // Binds the data to the ViewHolder at the specified position
    override fun onBindViewHolder(holder: MyViewHolder, position: Int) {
        val event = getItem(position) // Get the event for this position
        holder.bind(event) // Bind event data to the ViewHolder

        // Set up a click listener to navigate to event detail
        holder.itemView.setOnClickListener {
            val intent = Intent(context, DetailEventActivity::class.java).apply {
                putExtra("EXTRA_EVENT_ID", event.id) // Pass the event ID to the detail activity
            }
            context.startActivity(intent) // Start the detail activity
        }
    }

    // Companion object for handling item differences
    companion object {
        // DiffUtil callback to optimize list updates
        val DIFF_CALLBACK = object : DiffUtil.ItemCallback<ListEventsItem>() {
            // Check if two items are the same based on their IDs
            override fun areItemsTheSame(oldItem: ListEventsItem, newItem: ListEventsItem): Boolean {
                return oldItem.id == newItem.id // Compare event IDs
            }

            // Check if the contents of two items are the same
            override fun areContentsTheSame(oldItem: ListEventsItem, newItem: ListEventsItem): Boolean {
                return oldItem == newItem // Compare contents
            }
        }
    }
}
```

### Key Points
- **ViewHolder Pattern**: Improves performance by minimizing calls to `findViewById`.
- **Adapter**: Connects data to RecyclerView.

## 7. Activity Layout XML

The layout for the Detail Event Activity is defined in `activity_detail_event.xml`.

```xml
<?xml version="1.0" encoding="utf-8"?>
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/activity_detail_event"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@android:color/white">

    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical"
        android:padding="8dp">

        <ImageView
            android:id="@+id/ivDetailEventBanner"
            android:layout_width="match_parent"
            android:layout_height="200dp"
            android:scaleType="centerCrop"
            tools:src="@tools:sample/avatars" />

        <TextView
            android:id="@+id/tvDetailEventName"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            style="@style/Heading1"
            android:textAlignment="center"
            android:layout_marginTop="8dp"
            tools:text="Detail Event Name" />

        <TextView
            android:id="@+id/tvDataEventOwner"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            style="@style/SmallText"
            android:textAlignment="center"
            tools:text="Diselenggarakan oleh : "/>


        <LinearLayout
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:orientation="horizontal"
            android:layout_marginTop="8dp"
            android:weightSum="2">
            <TextView
                android:id="@+id/tvDataQuota"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:textAlignment="center"
                style="@style/BodyText"
                android:textStyle="bold"
                tools:text="Sisa Quota: Example" />

            <TextView
                android:id="@+id/tvDataBeginTime"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:layout_weight="1"
                android:textAlignment="center"
                android:textStyle="bold"
                style="@style/BodyText"
                tools:text="2024 10 20" />

        </LinearLayout>

        <TextView
            android:id="@+id/ivDescription"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            style="@style/Heading1"
            android:layout_marginTop="12dp"
            android:text="Infomasi: "/>

        <TextView
            android:id="@+id/tvDataDescription"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_marginTop="8dp"
            style="@style/BodyText"
            tools:text="Detail Description"/>

        <Button
            android:id="@+id/action_register"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_weight="1"
            android:layout_marginTop="8dp"
            android:text="Register"/>

        <!-- Loading Indicator -->
        <ProgressBar
            android:id="@+id/loadingIndicator"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:layout_gravity="center"
            android:visibility="gone"/> <!-- Initially hidden -->
    </LinearLayout>
</ScrollView>
```

### Key Points
- **Layout Components**: Define UI elements for displaying event details.
- **ProgressBar**: Indicates loading state.

## 8. Key Points

1. **API Configuration**: Properly set up Retrofit to manage API requests with clear logging for easier debugging.
2. **ViewModel**: Use ViewModel for managing UI-related data in a lifecycle-aware way, improving app performance and stability.
3. **LiveData**: Observing LiveData ensures UI components are updated automatically when the data changes.
4. **Image Loading**: Use Glide for efficient image loading and caching to enhance the user experience.
5. **Separation of Concerns**: Maintain clean architecture by separating data retrieval, UI handling, and API calls into distinct classes and files.

By following this guide, you should now have a fully functional detail event page in your Android application, equipped with proper architecture and best practices in Kotlin development. Feel free to modify and expand upon this code to fit your specific needs!

#### Reference
1. Dicoding Indonesia
2. Explained by GPT