# Implementing the Glide Library in Android with Kotlin

## Objective
In this tutorial, you will learn how to implement the Glide library to efficiently load and display images of heroes from the internet in your Android application. Glide is a powerful image loading library that simplifies image handling, including caching and resizing, which helps prevent common memory-related issues like `OutOfMemory` errors. By the end of this tutorial, you'll have a functional app that displays hero images, as shown below:

## Tutorial Flow
We'll cover the following steps:
1. Adding the Glide library dependency in `build.gradle`.
2. Syncing the project to integrate the library.
3. Utilizing Glide to load images in your RecyclerView.

---

## Step-by-Step Implementation of the Glide Library

### Step 1: Prepare Your Project
Start by opening your **Hero List** project, or if you don’t have one, you can download the **RecyclerView Exercise** project.

### Step 2: Add the Glide Dependency
1. Visit the [Glide GitHub page](https://github.com/bumptech/glide) and copy the dependency code.
2. Open your `build.gradle.kts` (module: app) file and add the following code in the dependencies section:

   ```kotlin
   dependencies {
       ...
       // Kotlin DSL (new version)
       implementation("com.github.bumptech.glide:glide:4.16.0")
       kapt("com.github.bumptech.glide:compiler:4.16.0") // For annotation processing
   }
   ```

3. Click **Sync Now** to download the library.

### Note on Best Practices:
For Android Studio versions Iguana and above, you may receive warnings regarding hardcoded library versions. To fix this:
- Hover over the dependency, press `Alt + Enter` (Windows) or `Option + Enter` (Mac), and select **Replace with new library declaration**.
- Click **Sync Now** afterward.

### Step 3: Update Image Data
Modify the `Strings.xml` file to change the image data from `integer-array` to `string-array`, allowing you to store URLs for the hero images. Here’s the updated code:

```xml
<string-array name="data_photo">
    <item>https://upload.wikimedia.org/wikipedia/commons/8/87/Ahmad_Dahlan.jpg</item>
    <item>https://upload.wikimedia.org/wikipedia/commons/3/3f/Ahmad_Yani.jpg</item>
    <item>https://upload.wikimedia.org/wikipedia/commons/e/ed/Bung_Tomo.jpg</item>
    <item>https://upload.wikimedia.org/wikipedia/commons/b/be/Col_Gatot_Subroto%2C_Kenang-Kenangan_Pada_Panglima_Besar_Letnan_Djenderal_Soedirman%2C_p27.jpg</item>
    <item>https://upload.wikimedia.org/wikipedia/commons/3/3a/Ki_Hadjar_Dewantara_Mimbar_Umum_18_October_1949_p2.jpg</item>
    <item>https://upload.wikimedia.org/wikipedia/commons/thumb/3/3b/VP_Hatta.jpg/330px-VP_Hatta.jpg</item>
    <item>https://upload.wikimedia.org/wikipedia/commons/thumb/e/e7/Sudirman.jpg/486px-Sudirman.jpg</item>
    <item>https://upload.wikimedia.org/wikipedia/commons/thumb/0/01/Presiden_Sukarno.jpg/330px-Presiden_Sukarno.jpg</item>
    <item>https://upload.wikimedia.org/wikipedia/commons/a/a6/Supomo.jpg</item>
    <item>https://upload.wikimedia.org/wikipedia/commons/thumb/f/f7/TanMalaka_DariPendjara_ed3.jpg/330px-TanMalaka_DariPendjara_ed3.jpg</item>
</string-array>
```

### Step 4: Update the Hero Data Class
Since the image data type has changed from integer to string, update the `Hero` data class:

```kotlin
@Parcelize
data class Hero(
    var name: String,            // Hero's name
    var description: String,     // Hero's description
    var photo: String            // Hero's image URL
) : Parcelable
```

### Step 5: Update the getListHeroes Function
Next, modify the `getListHeroes` function in `MainActivity` to fetch data from the updated string-array:

```kotlin
private fun getListHeroes(): ArrayList<Hero> {
    val dataName = resources.getStringArray(R.array.data_name) // Fetch hero names
    val dataDescription = resources.getStringArray(R.array.data_description) // Fetch descriptions
    val dataPhoto = resources.getStringArray(R.array.data_photo) // Fetch image URLs
    val listHero = ArrayList<Hero>()

    for (i in dataName.indices) {
        val hero = Hero(dataName[i], dataDescription[i], dataPhoto[i]) // Create Hero object
        listHero.add(hero) // Add hero to the list
    }
    return listHero
}
```

### Step 6: Update the Adapter
Now, open the `ListHeroAdapter` and modify the image loading section. Use Glide to load images from the URLs:

```kotlin
override fun onBindViewHolder(holder: ListViewHolder, position: Int) {
    val (name, description, photo) = listHero[position] // Destructure hero data
    // Load image using Glide
    Glide.with(holder.itemView.context)
        .load(photo) // URL of the image
        .placeholder(R.drawable.placeholder) // Placeholder image while loading
        .error(R.drawable.error) // Error image if loading fails
        .into(holder.imgPhoto) // Target ImageView
    holder.tvName.text = name // Set hero name
    holder.tvDescription.text = description // Set hero description

    // Handle item click
    holder.itemView.setOnClickListener {
        onItemClickCallback.onItemClicked(listHero[holder.adapterPosition])
    }
}
```

### Step 7: Enable Internet Permission
To load images from the internet, you must enable internet permission in your `AndroidManifest.xml`:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    package="com.dicoding.picodiploma.myrecyclerview">

    <uses-permission android:name="android.permission.INTERNET" /> <!-- Internet permission -->

    <application
        ...
    </application>
</manifest>
```

### Step 8: Run Your Application
After completing all the steps, run your application. You should see the hero images displayed correctly in the RecyclerView.

![Output](dos:bc3fd7ebd96cb781ca07251e7d7b8b5420230630173227.png)

---

## Code Breakdown

### Understanding Glide
Glide is a versatile library for loading images efficiently in Android applications. Here's a basic code snippet to load an image from a URL:

```kotlin
Glide.with(holder.itemView.context)
    .load(photo) // Image URL
    .into(holder.imgPhoto) // ImageView to display the image
```

### Additional Glide Features
- **circleCrop()**: Crops the image into a circular shape.
- **transition()**: Adds a transition effect when the image loads.
- **thumbnail()**: Displays a smaller version of the image while the full image loads.
- **error()**: Displays a placeholder image if loading fails.

### Safety and Compatibility Notes
- **Permissions**: Always ensure you have the required permissions for internet access in your app.
- **Error Handling**: Implement error handling for network issues to enhance the user experience. Glide handles a lot of this for you, but be prepared for scenarios where images cannot be loaded.
- **Memory Management**: Glide optimizes image loading and caching, but make sure you are using appropriately sized images to maintain performance.

### Reusable Code Pattern
To maintain consistency and simplify image loading, you can create a utility function:

```kotlin
fun loadImage(context: Context, url: String, imageView: ImageView) {
    Glide.with(context)
        .load(url)
        .placeholder(R.drawable.placeholder) // Placeholder while loading
        .error(R.drawable.error) // Error image if loading fails
        .into(imageView) // Target ImageView
}
```

### Use Cases for Glide
- **Loading images from the web**: Perfect for displaying user profile pictures, product images, etc.
- **Displaying images from local storage**: Glide can also handle images stored on the device.
- **Handling animated GIFs**: Glide supports GIFs and can display them seamlessly.

---
#### Reference:
1. Dicoding Indonesia
2. Explained by GPT