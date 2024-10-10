# Comprehensive Tutorial: Creating a Tab Layout with ViewPager2 in Android

In this tutorial, we will build a simple Android app that features a tab layout using `ViewPager2`. This app will allow users to switch between two screens: "Home" and "Profile". Each step will be explained to help you understand the purpose behind it, making it easy for beginners to follow along.

## Table of Contents

1. [Initial Setup](#initial-setup)
2. [Adding Dependencies](#adding-dependencies)
3. [Creating the Project Structure](#creating-the-project-structure)
4. [Implementing the Main Activity](#implementing-the-main-activity)
5. [Creating Fragments](#creating-fragments)
6. [Creating the Adapter](#creating-the-adapter)
7. [Designing the Layouts](#designing-the-layouts)
8. [Running the App](#running-the-app)
9. [Summary of Key Concepts](#summary-of-key-concepts)

## Initial Setup

### Step 1: Create a New Project

1. **Open Android Studio**.
2. Select **New Project**.
3. Choose **Empty Activity**.
4. Name your project (e.g., `MyTabLayout`).
5. Set **Language** to **Kotlin**.
6. Click **Finish**.

*Why?* This creates a basic framework for our app, providing essential components like the main activity and layout files.

### Step 2: Set Up the Minimum SDK

Open `build.gradle` and set the `minSdkVersion` to at least 21 (Android 5.0 Lollipop):

```groovy
android {
    compileSdk = 34

    defaultConfig {
        applicationId = "com.siaptekno.mytablayout"
        minSdk = 21 // Minimum version required
        targetSdk = 34
        versionCode = 1
        versionName = "1.0"
    }
}
```

*Why?* This ensures that your app can run on a wider range of devices while using modern Android features.

## Adding Dependencies

### Step 3: Include Required Libraries

To use `ViewPager2` and `TabLayout`, you need to add some libraries. Open the `build.gradle (Module: app)` file and add these dependencies:

```groovy
dependencies {
    implementation 'androidx.appcompat:appcompat:1.7.0' // For basic UI components
    implementation 'androidx.core:core-ktx:1.12.0' // Kotlin extensions
    implementation 'androidx.fragment:fragment-ktx:1.6.0' // Fragment support
    implementation 'androidx.viewpager2:viewpager2:1.0.0' // ViewPager2 for swipe navigation
    implementation 'com.google.android.material:material:1.10.0' // Material components
}
```

After adding these lines, click **Sync Now** to download the libraries.

*Why?* These libraries provide the necessary components to create a modern user interface with tabs and swiping functionality.

## Creating the Project Structure

### Step 4: Create the Required Packages

In the `app/src/main/java/com/siaptekno/mytablayout` directory, create these packages:
- `ui` (for the main activity)
- `adapter` (for the adapter)
- `fragment` (for the fragments)

*Why?* Organizing your code into packages helps maintain clarity and makes it easier to manage as the project grows.

## Implementing the Main Activity

### Step 5: Create the Main Activity

Open `MainActivity.kt` in the `ui` package and add the following code:

```kotlin
package com.siaptekno.mytablayout.ui

import android.os.Bundle
import androidx.annotation.StringRes
import androidx.appcompat.app.AppCompatActivity
import androidx.viewpager2.widget.ViewPager2
import com.google.android.material.tabs.TabLayout
import com.google.android.material.tabs.TabLayoutMediator
import com.siaptekno.mytablayout.R
import com.siaptekno.mytablayout.adapter.SectionPagerAdapter

class MainActivity : AppCompatActivity() {

    // Array of tab titles
    companion object {
        @StringRes
        private val TAB_TITLES = intArrayOf(
            R.string.tab_text_home,  // Home tab title
            R.string.tab_text_profile // Profile tab title
        )
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main) // Set the layout for this activity

        // Find ViewPager2 in the layout
        val viewPager: ViewPager2 = findViewById(R.id.view_pager)
        viewPager.adapter = SectionPagerAdapter(this) // Set the adapter for the ViewPager2

        // Find TabLayout in the layout
        val tabs: TabLayout = findViewById(R.id.tabs)
        // Connect TabLayout with ViewPager2
        TabLayoutMediator(tabs, viewPager) { tab, position ->
            tab.text = resources.getString(TAB_TITLES[position]) // Set the tab title
        }.attach() // Attach the mediator
    }
}
```

### Explanation of Code
- **ViewPager2**: This allows users to swipe between different fragments, similar to flipping through a book.
- **TabLayoutMediator**: This connects the tabs to the ViewPager2, managing the titles for each tab.

*Why?* This code sets up the main activity, enabling tab navigation through the ViewPager2.

## Creating Fragments

### Step 6: Create HomeFragment

Create `HomeFragment.kt` in the `fragment` package:

```kotlin
package com.siaptekno.mytablayout.fragment

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.fragment.app.Fragment
import com.siaptekno.mytablayout.R

class HomeFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_home, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        // Find the TextView and set the text for the Home tab
        val tvLabel: TextView = view.findViewById(R.id.section_label)
        tvLabel.text = getString(R.string.content_tab_home) // Set content text
    }
}
```

*Why?* This fragment represents the "Home" section of your app, displaying relevant content when the user selects this tab.

### Step 7: Create ProfileFragment

Create `ProfileFragment.kt` in the `fragment` package:

```kotlin
package com.siaptekno.mytablayout.fragment

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import android.widget.TextView
import androidx.fragment.app.Fragment
import com.siaptekno.mytablayout.R

class ProfileFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View? {
        // Inflate the layout for this fragment
        return inflater.inflate(R.layout.fragment_profile, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)

        // Find the TextView and set the text for the Profile tab
        val tvLabel: TextView = view.findViewById(R.id.section_label)
        tvLabel.text = getString(R.string.content_tab_profile) // Set content text
    }
}
```

*Why?* Similar to `HomeFragment`, this fragment displays the "Profile" section, providing a separate area for user profile information.

## Creating the Adapter

### Step 8: Create SectionPagerAdapter

Create `SectionPagerAdapter.kt` in the `adapter` package:

```kotlin
package com.siaptekno.mytablayout.adapter

import androidx.appcompat.app.AppCompatActivity
import androidx.fragment.app.Fragment
import androidx.viewpager2.adapter.FragmentStateAdapter
import com.siaptekno.mytablayout.fragment.HomeFragment
import com.siaptekno.mytablayout.fragment.ProfileFragment

class SectionPagerAdapter(activity: AppCompatActivity) : FragmentStateAdapter(activity) {

    override fun getItemCount(): Int {
        return 2 // Number of tabs (Home and Profile)
    }

    override fun createFragment(position: Int): Fragment {
        // Create the fragment based on the tab position
        return when (position) {
            0 -> HomeFragment() // First tab is Home
            1 -> ProfileFragment() // Second tab is Profile
            else -> HomeFragment() // Default to Home
        }
    }
}
```

### Explanation of Code
- **FragmentStateAdapter**: This class efficiently manages fragments, allowing for smooth transitions and memory usage.
- **getItemCount()**: Returns the number of tabs (2 in this case).
- **createFragment()**: Instantiates the appropriate fragment based on the tab position.

*Why?* The adapter connects the fragments to the ViewPager2, controlling which fragment is displayed as the user navigates through the tabs.

## Designing the Layouts

### Step 9: Create the Main Layout

Create `activity_main.xml` in the `res/layout` directory:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res

/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <com.google.android.material.tabs.TabLayout
        android:id="@+id/tabs"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:background="?attr/colorPrimary"
        app:tabTextColor="@android:color/white"/>

    <androidx.viewpager2.widget.ViewPager2
        android:id="@+id/view_pager"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
</LinearLayout>
```

*Why?* This layout defines the structure for the main activity, including the `TabLayout` for tabs and `ViewPager2` for the fragments.

### Step 10: Create Fragment Layouts

**Home Fragment Layout (`fragment_home.xml`)**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/section_label"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="@string/content_tab_home" />
</FrameLayout>
```

**Profile Fragment Layout (`fragment_profile.xml`)**:

```xml
<?xml version="1.0" encoding="utf-8"?>
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <TextView
        android:id="@+id/section_label"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_gravity="center"
        android:text="@string/content_tab_profile" />
</FrameLayout>
```

### Explanation of Code
- **FrameLayout**: A simple layout that allows for a single child view (like the `TextView`).
- **TextView**: Displays content centered on the screen.

*Why?* These layouts define the user interface for each fragment, specifying how the content is displayed.

### Step 11: Create String Resources

Open `res/values/strings.xml` and add the following strings:

```xml
<resources>
    <string name="app_name">My Tab Layout</string>
    <string name="tab_text_home">Home</string>
    <string name="tab_text_profile">Profile</string>
    <string name="content_tab_home">Welcome to the Home Tab</string>
    <string name="content_tab_profile">Welcome to the Profile Tab</string>
</resources>
```

*Why?* This provides user-friendly text for the app, making it easier to manage and localize if necessary.

## Running the App

### Step 12: Build and Run

1. Connect your emulator or Android device.
2. Click the **Run** button in Android Studio.
3. The app will launch with two tabs: "Home" and "Profile". You can swipe between them to see different content.

*Why?* Running the app allows you to test the functionality and ensure everything works as intended.

## Summary of Key Concepts

- **ViewPager2**: Facilitates swipe navigation between different fragments.
- **TabLayout**: Displays tabs for easy access to different sections.
- **Fragment**: A modular UI component that can be reused across activities.
- **Adapter**: Connects the fragments to the ViewPager2, managing the displayed content based on user interaction.

Congratulations! You've successfully created a tabbed layout using `ViewPager2` in your Android application. You can now expand on this foundation to add more features and complexity. Happy coding!