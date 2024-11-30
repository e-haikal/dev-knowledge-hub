
## **How to Integrate a Toolbar as the ActionBar and Show/Hide It Dynamically in Android Using Kotlin**

### Step-by-Step Guide to Use a `Toolbar` as Your App's ActionBar

#### 1. **Define the Theme in `styles.xml`**

First, ensure that your app theme is set to a `NoActionBar` theme. This removes the default `ActionBar` provided by the Android system, allowing you to use a custom `Toolbar`.

```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base theme without ActionBar -->
    <style name="Theme.TahsinMQI.NoActionBar" parent="Theme.Material3.DayNight.NoActionBar">
        <!-- Customize your theme properties here -->
        <item name="colorPrimary">@color/greenish</item>
        <item name="colorPrimaryDark">@color/greenish_dark</item>
        <item name="colorAccent">@color/greenish_accent</item>
    </style>
</resources>
```

**Explanation:**
- `Theme.Material3.DayNight.NoActionBar` is a theme that removes the built-in `ActionBar`.
- Customize `colorPrimary`, `colorPrimaryDark`, and `colorAccent` as needed for your app.

#### 2. **Apply the Theme in `AndroidManifest.xml`**

Make sure to apply the theme to your `MainActivity` or the entire app in the `AndroidManifest.xml`:

```xml
<application
    android:theme="@style/Theme.TahsinMQI.NoActionBar">
    <activity android:name=".MainActivity"
              android:theme="@style/Theme.TahsinMQI.NoActionBar">
        <!-- Other attributes -->
    </activity>
</application>
```

**Explanation:**
- The `android:theme` attribute applies your `NoActionBar` theme to the specified activity or the entire app.

#### 3. **Set Up the Toolbar in Your `MainActivity`**

In your `MainActivity`, configure the `Toolbar` to act as the `ActionBar`:

```kotlin
package com.siaptekno.tahsinmqi

import android.os.Bundle
import androidx.appcompat.app.AppCompatActivity
import androidx.appcompat.widget.Toolbar
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import androidx.navigation.findNavController
import androidx.navigation.ui.AppBarConfiguration
import androidx.navigation.ui.setupActionBarWithNavController
import androidx.navigation.ui.setupWithNavController
import com.google.android.material.bottomnavigation.BottomNavigationView
import com.siaptekno.tahsinmqi.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity() {
    private lateinit var binding: ActivityMainBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(binding.root)

        // Set up the Toolbar as the ActionBar
        val toolbar: Toolbar = findViewById(R.id.toolbar)
        setSupportActionBar(toolbar)

        // Set up navigation and app bar configuration
        val navView: BottomNavigationView = binding.navView
        val navController = findNavController(R.id.nav_host_fragment_activity_temp_bottom_nav)
        val appBarConfiguration = AppBarConfiguration(
            setOf(R.id.navigation_home, R.id.navigation_alquran)
        )
        setupActionBarWithNavController(navController, appBarConfiguration)
        navView.setupWithNavController(navController)

        // Add a listener to show/hide the Toolbar based on the current destination
        navController.addOnDestinationChangedListener { _, destination, _ ->
            if (destination.id == R.id.navigation_home || destination.id == R.id.navigation_alquran) {
                supportActionBar?.hide()  // Hide the Toolbar for these destinations
            } else {
                supportActionBar?.show()  // Show the Toolbar for other destinations
            }
        }
    }
}
```

**Explanation:**
- `setSupportActionBar(toolbar)` sets the custom `Toolbar` as the `ActionBar` for the activity.
- The `navController` and `AppBarConfiguration` are used to synchronize the navigation with the `Toolbar`.

#### 4. **Hide/Show Toolbar Dynamically for Specific Fragments**

If you need to show or hide the `Toolbar` for specific fragments, add logic in the fragment itself. 

**Example for a Fragment (`HomeFragment`) to Hide the Toolbar:**

```kotlin
package com.siaptekno.tahsinmqi.ui.home

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import com.siaptekno.tahsinmqi.R

class HomeFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        // Hide the Toolbar when this fragment is displayed
        (activity as? AppCompatActivity)?.supportActionBar?.hide()
        return inflater.inflate(R.layout.fragment_home, container, false)
    }

    override fun onDestroyView() {
        super.onDestroyView()
        // Ensure the Toolbar is shown when the fragment is destroyed
        (activity as? AppCompatActivity)?.supportActionBar?.show()
    }
}
```

**Explanation:**
- This ensures that when the `HomeFragment` is shown, the `Toolbar` is hidden.
- When the fragment is destroyed, the `Toolbar` is shown again.

**Example for a Fragment (`MaterialFragment`) to Show the Toolbar:**

```kotlin
package com.siaptekno.tahsinmqi.ui.material

import android.os.Bundle
import android.view.LayoutInflater
import android.view.View
import android.view.ViewGroup
import androidx.fragment.app.Fragment
import com.siaptekno.tahsinmqi.R

class MaterialFragment : Fragment() {

    override fun onCreateView(
        inflater: LayoutInflater, container: ViewGroup?,
        savedInstanceState: Bundle?
    ): View {
        // Show the Toolbar when this fragment is displayed
        (activity as? AppCompatActivity)?.supportActionBar?.show()
        return inflater.inflate(R.layout.fragment_material, container, false)
    }
}
```

**Explanation:**
- This code ensures that when the `MaterialFragment` is displayed, the `Toolbar` is shown.

### Final Thoughts
- **Using a `NoActionBar` theme**: Ensures no default `ActionBar` appears, allowing your custom `Toolbar` to take its place.
- **Flexibility**: A `Toolbar` provides more customization options for titles, icons, and other UI elements compared to the built-in `ActionBar`.
- **Control**: You can show or hide the `Toolbar` as needed, making it adaptable for different fragments or app states.

By following these steps, you will have a fully functional `Toolbar` that acts as the app's `ActionBar`, allowing you to show or hide it dynamically based on the fragment or navigation destination.

#### Reference:
1. Explained by GPT