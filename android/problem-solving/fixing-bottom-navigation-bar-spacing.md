# Fixing Bottom Navigation Bar Spacing in Edge-to-Edge Mode on Android

### Introduction
Enabling edge-to-edge mode in an Android application allows for a full-screen experience, utilizing every part of the display, including areas typically reserved for system bars. However, this can lead to overlapping issues with the bottom navigation bar. Proper padding adjustments are essential to maintain a clean and usable interface.

### Problem
When edge-to-edge mode is activated, the bottom navigation bar may overlap with the system navigation bar, creating accessibility issues and a cluttered user experience.

### Solution Steps

1. **Enable Edge-to-Edge Mode**
   - To allow your app to use the full screen, call `enableEdgeToEdge()` in your `Activity`:
   ```kotlin
   enableEdgeToEdge()
   ```

2. **Set Up Window Insets Listener**
   - Implement `ViewCompat.setOnApplyWindowInsetsListener()` to dynamically adjust padding based on the system bar insets:
   ```kotlin
   ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.activity_main)) { v, insets ->
       // Logic to adjust padding
   }
   ```

3. **Adjust Padding for Main View and Bottom Navigation Bar**
   - **Main View**: Apply padding to ensure the content is not obscured by the status bar.
   - **Bottom Navigation Bar**: Set the bottom padding to match the height of the system navigation bar to prevent overlap.

#### Example Code
Here’s how to implement these steps effectively:

```kotlin
package com.siaptekno.dicodingevent

import android.os.Bundle
import androidx.activity.enableEdgeToEdge
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import com.google.android.material.bottomnavigation.BottomNavigationView

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        enableEdgeToEdge() // Allow full-screen mode
        setContentView(R.layout.activity_main)

        val bottomNavigationView = findViewById<BottomNavigationView>(R.id.nav_view)

        // Adjust layout for system bars
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.activity_main)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            
            // Apply padding to the main view
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, 0)

            // Set bottom padding for the navigation bar
            bottomNavigationView.setPadding(0, 0, 0, systemBars.bottom)

            insets // Return insets for further processing
        }
    }
}
```

### How Each Padding Adjustment Works

#### 1. **Main View Padding**
- **Purpose**: Ensures that the main content is visible and not obscured by the status bar or screen edges.
- **Code**: 
   ```kotlin
   v.setPadding(systemBars.left, systemBars.top, systemBars.right, 0)
   ```
- **Explanation**:
  - `systemBars.left`: Adds padding on the left to avoid clipping.
  - `systemBars.top`: Keeps content below the status bar.
  - `systemBars.right`: Adds padding on the right.
  - `0`: No padding at the bottom since the bottom navigation bar will handle its spacing.

#### 2. **Bottom Navigation Bar Padding**
- **Purpose**: Prevents the bottom navigation bar from overlapping with the system navigation bar.
- **Code**: 
   ```kotlin
   bottomNavigationView.setPadding(0, 0, 0, systemBars.bottom)
   ```
- **Explanation**:
  - `0`: No padding on the left.
  - `0`: No padding on the top.
  - `0`: No padding on the right.
  - `systemBars.bottom`: Adds padding at the bottom to keep the navigation bar above the system navigation buttons.

### Summary
By implementing these padding adjustments, you can ensure that your app's bottom navigation bar remains functional and visually distinct in edge-to-edge mode. Proper padding enhances the user experience and maintains accessibility, preventing overlap and creating a clean interface.


#### Reference:
1. Explained by GPT