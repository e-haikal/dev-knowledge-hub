# Hide Toolbar/Action Bar for Specific Activity in Kotlin Android

When developing Android applications, there may be cases where you need to hide the toolbar or action bar for specific activities, such as a login or welcome screen. This guide will show you how to achieve this in your Kotlin Android project.

## Step-by-Step Tutorial

### 1. Define Themes in `styles.xml`

To hide the action bar for specific activities, create a theme that excludes the action bar. For example:

```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <!-- Base application theme. -->

    <!-- Base theme with Action Bar -->
    <style name="Base.Theme.WasteWise" parent="Theme.Material3.DayNight">
        <item name="colorPrimary">#4CAF50</item>
    </style>

    <!-- Base theme without Action Bar -->
    <style name="Theme.WasteWise.NoActionBar" parent="Theme.Material3.DayNight.NoActionBar">
        <item name="colorPrimary">#4CAF50</item>
    </style>

    <style name="Theme.WasteWise" parent="Base.Theme.WasteWise" />
</resources>
```

Here:
- `Theme.WasteWise` is the base theme for your application with an action bar.
- `Theme.WasteWise.NoActionBar` is a variant without the action bar.

### 2. Assign Themes to Specific Activities in `AndroidManifest.xml`

In your `AndroidManifest.xml`, you can assign the `Theme.WasteWise.NoActionBar` theme to activities where the action bar should be hidden. For example:

```xml
<manifest xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools">

    <application
        android:allowBackup="true"
        android:dataExtractionRules="@xml/data_extraction_rules"
        android:fullBackupContent="@xml/backup_rules"
        android:icon="@mipmap/ic_launcher"
        android:label="@string/app_name"
        android:roundIcon="@mipmap/ic_launcher_round"
        android:supportsRtl="true"
        android:theme="@style/Theme.WasteWise"
        tools:targetApi="31">

        <!-- Activity without Action Bar -->
        <activity
            android:name=".view.login.LoginActivity"
            android:theme="@style/Theme.WasteWise.NoActionBar"
            android:exported="false" />

        <activity
            android:name=".view.register.RegisterActivity"
            android:theme="@style/Theme.WasteWise.NoActionBar"
            android:exported="false" />

        <!-- Activity with Action Bar -->
        <activity
            android:name=".view.GitTesting"
            android:theme="@style/Theme.WasteWise"
            android:exported="false" />

        <!-- Main Activity without Action Bar -->
        <activity
            android:name=".MainActivity"
            android:theme="@style/Theme.WasteWise.NoActionBar"
            android:exported="true" />
    </application>

</manifest>
```

### 3. Verify Theme Application

Run your application and verify that the action bar is hidden for the specified activities. Ensure the toolbar is still visible for activities using the default `Theme.WasteWise`.

### Additional Notes
- You can customize each theme further by adding specific styles (e.g., color schemes) for individual activities.
- Using different themes for activities helps keep the design modular and maintainable.

With this setup, you can easily control which activities have a visible action bar and which do not, improving your application's UI consistency.

---
#### Reference:
1. Explained by GPT