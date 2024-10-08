# Learning Fragments in an Activity

## Objective

In this tutorial, you'll learn about **Fragments** and their lifecycle, enabling you to create modular and reusable UI components. By the end, you'll have a basic app that can switch views without changing the entire activity.

---

## Step-by-Step Guide

### 1. Create a New Project

Open **Android Studio** and create a new project with these settings:

- **Project Name:** MyFlexibleFragment
- **Target SDK:** Phone and Tablet, API level 24
- **Activity Type:** Empty Views Activity
- **Activity Name:** MainActivity
- **Use AndroidX artifacts:** True
- **Language:** Kotlin / Java

### 2. Set Up the Main Layout (`activity_main.xml`)

Edit `activity_main.xml` to include a `FrameLayout`. This acts as a container for your Fragment.

```xml
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/frame_container"
    android:layout_width="match_parent"
    android:layout_height="match_parent"/>
```

**Explanation:**  
- **FrameLayout**: This layout can hold one or more views on top of each other, making it ideal for displaying Fragments.

### 3. Create the HomeFragment

1. Right-click on your project package.
2. Select **New** → **Fragment** → **Fragment (Blank)**.
3. Name it **HomeFragment** and click **Finish**.

### 4. Design the Fragment Layout (`fragment_home.xml`)

Open `fragment_home.xml` and add a `TextView` and a `Button`:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/hello_home_fragment" />

    <Button
        android:id="@+id/btn_category"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/to_category" />
</LinearLayout>
```

**Explanation:**
- **LinearLayout**: This organizes its children in a vertical column.
- **TextView**: Displays text on the screen.
- **Button**: Provides an interactive element that users can click.

### 5. Add String Resources

Go to `res/values/strings.xml` and add these strings:

```xml
<resources>
    <string name="app_name">MyFlexibleFragment</string>
    <string name="hello_home_fragment">Hello, this is the Home Fragment</string>
    <string name="to_category">Go to Category Fragment</string>
</resources>
```

**Explanation:**  
- These string resources are used for displaying text in your UI. Using resources allows for easy localization and changes.

### 6. Implement the HomeFragment Code

Edit the `HomeFragment` class as follows:

```kotlin
class HomeFragment : Fragment(), View.OnClickListener {

    override fun onCreateView(inflater: LayoutInflater, container: ViewGroup?,
                              savedInstanceState: Bundle?): View? {
        // Inflate the layout for this fragment from XML
        return inflater.inflate(R.layout.fragment_home, container, false)
    }

    override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
        super.onViewCreated(view, savedInstanceState)
        // Find the button and set a click listener
        val btnCategory: Button = view.findViewById(R.id.btn_category)
        btnCategory.setOnClickListener(this)    
    }

    override fun onClick(v: View) {
        // Logic for what happens when the button is clicked will go here
    }
}
```

**Code Explanation:**
- **onCreateView**: This method is called to create the view for the Fragment. It uses `inflate()` to convert XML layout into a View object.
- **onViewCreated**: This method is called after the view is created. Here, you initialize UI components and set up listeners.
- **onClick**: This method will handle button click events.

### 7. Add the Fragment to MainActivity

In your `MainActivity`, include the following code to add the `HomeFragment`:

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)

    // Get the FragmentManager to manage Fragments
    val fragmentManager = supportFragmentManager
    // Create an instance of HomeFragment
    val homeFragment = HomeFragment()
    // Check if the Fragment is already added
    val fragment = fragmentManager.findFragmentByTag(HomeFragment::class.java.simpleName)

    // If the Fragment isn't already added, add it
    if (fragment !is HomeFragment) {
        fragmentManager
            .beginTransaction() // Start the transaction
            .add(R.id.frame_container, homeFragment, HomeFragment::class.java.simpleName) // Add the Fragment to the container
            .commit() // Commit the transaction
    }
}
```

**Code Explanation:**
- **supportFragmentManager**: This allows you to manage fragments within the activity.
- **findFragmentByTag**: Checks if the Fragment is already added, preventing duplicate instances.
- **beginTransaction()**: Starts a new transaction to modify fragments.
- **add()**: Adds the specified Fragment to the container (`frame_container`).
- **commit()**: Completes the transaction, making the changes visible.

### 8. Run Your Application

Now, run your app. You should see the `TextView` and `Button` from the `HomeFragment`.

---

## Summary of Key Concepts

- **FrameLayout**: Acts as a container for Fragments.
- **Fragment Lifecycle**: Understand methods like `onCreateView` and `onViewCreated` for initializing UI.
- **FragmentManager**: Manages Fragment transactions (add, replace, remove).

---

## Next Steps

Now that you've set up your first Fragment, you can create additional Fragments and learn how to switch between them dynamically. Happy coding!

---
#### Reference:
1. Dicoding Indonesia
2. Explained by GPT