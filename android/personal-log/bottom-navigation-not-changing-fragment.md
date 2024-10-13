#### Date: October 13, 2024
#### Issue Title: Bottom Navigation Not Changing Fragments

---

**Description:**
- The bottom navigation in my Android app (Dicoding Event) was not switching between fragments when selecting the Upcoming and Finished options. Only the Home fragment was functioning correctly.

**Environment:**
- **Device:** Emulator
- **OS Version:** Android 11
- **Development Tools:** Android Studio [Insert Version]
- **Libraries/Dependencies:** AndroidX Navigation, Material Components

**Steps to Reproduce:**
1. Launch the app on an emulator.
2. Click on the Upcoming button in the bottom navigation.
3. Click on the Finished button in the bottom navigation.

**Expected Behavior:**
- The app should switch to the Upcoming fragment when the Upcoming button is clicked and to the Finished fragment when the Finished button is clicked.

**Actual Behavior:**
- Clicking the Upcoming and Finished buttons did not change the displayed fragment, and no logs were generated for those actions.

**Root Cause:**
- The IDs for the menu items in `bottom_nav_menu.xml` did not match the IDs defined in the navigation graph (`mobile_navigation.xml`), causing the navigation to fail.

**Resolution Steps:**
1. Do logging on each button to see if it's work or not.
```kotlin
navView.setOnNavigationItemSelectedListener { item ->
    val navController = findNavController(R.id.nav_host_fragment_activity_main)
    when (item.itemId) {
        R.id.navigation_home -> {
            navController.navigate(R.id.navigation_home)
            Log.d("Navigation", "Navigated to HomeFragment")
            true
        }
        R.id.navigation_upcoming -> {
            navController.navigate(R.id.navigation_upcoming)
            Log.d("Navigation", "Navigated to UpcomingFragment")
            true
        }
        R.id.navigation_finished -> {
            navController.navigate(R.id.navigation_finished)
            Log.d("Navigation", "Navigated to FinishedFragment")
            true
        }
        else -> false
    }
}

```

2. Updated the IDs in `bottom_nav_menu.xml` to match the IDs used in `mobile_navigation.xml`.

**Lessons Learned:**
- Always ensure consistency between menu item IDs and navigation graph IDs to avoid issues with fragment navigation.

**References:**
- [Android Navigation Documentation](https://developer.android.com/guide/navigation)

---
#### Reference:
1. Explained by GPT