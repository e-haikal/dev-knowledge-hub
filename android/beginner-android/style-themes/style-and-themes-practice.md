# Applying Styles and Themes in Android: A Beginner’s Guide

## Objective

In this codelab, we will learn how to apply styles and themes to an Android application. The focus will be on utilizing styles for consistency and themes for a cohesive design. By the end of this tutorial, you’ll have a clearer understanding of how to enhance your app’s appearance.

### Key Learning Outcomes:
- How to utilize styles in your application.
- How to use themes to manage the overall look and feel.
- You will see a visual result of these changes.

---

## Practice Flow

### Steps Overview

1. Change colors for light and night themes.
2. Create a custom style.
3. Implement the new custom styles.

### Getting Started

1. **Reopen your previous project** (`MyViewAndViewGroup`) or download it from the **View and ViewGroup Exercises**. Familiarize yourself with the `activity_main.xml` file and its components.

### Step 1: Change Default Colors

Open the `colors.xml` file located in `res → values → colors.xml`. Replace the existing values with the following:

```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <color name="black">#FF000000</color>
    <color name="white">#FFFFFFFF</color>
    <color name="gray">#607D8B</color>
    <color name="gray_light">#B0BEC5</color>
    <color name="gray_dark">#455A64</color>
    <color name="orange">#FF5722</color>
    <color name="orange_light">#FFAB91</color>
    <color name="orange_dark">#E64A19</color>
    <color name="colorSubtitle">#757575</color>
</resources>
```

### Step 2: Update Themes

Next, adjust the application theme by opening the `themes.xml` file located in `res → values → themes.xml`. Remove `.NoActionBar` from `Theme.Material3.DayNight` to display the default Action Bar:

```xml
<resources xmlns:tools="http://schemas.android.com/tools">
    <style name="Base.Theme.MyViewAndViewGroup" parent="Theme.Material3.DayNight">
        <item name="colorPrimary">@color/gray</item>
        <item name="colorOnPrimary">@color/white</item>
        <item name="colorSecondary">@color/orange_light</item>
        <item name="colorOnSecondary">@color/black</item>
    </style>

    <style name="Theme.MyViewAndViewGroup" parent="Base.Theme.MyViewAndViewGroup" />
</resources>
```

**Note**: Don’t forget to update the `night` version of your `themes.xml` file similarly.

### Step 3: Create Text Styles

Open the `themes.xml` file again and add styles for the text. This way, you can maintain a consistent look without writing lengthy style definitions for each TextView.

```xml
<style name="TextContent">
    <item name="android:layout_width">wrap_content</item>
    <item name="android:layout_height">wrap_content</item>
</style>

<style name="TextContent.HeadlineMedium">
    <item name="android:layout_marginLeft">16dp</item>
    <item name="android:layout_marginRight">16dp</item>
    <item name="android:textAppearance">@style/TextAppearance.Material3.HeadlineMedium</item>
</style>

<style name="TextContent.BodyMedium.Gray">
    <item name="android:textColor">@color/colorSubtitle</item>
</style>
```

**Note**: Ensure you mirror these styles in the `night` theme as well.

### Step 4: Apply Styles in `activity_main.xml`

Now, implement the styles you just created in the `activity_main.xml` file to streamline your TextViews. Here's an example of how to apply styles:

```xml
<TextView
    style="@style/TextContent.BodyMedium.Gray"
    android:layout_weight="1"
    android:text="@string/content_specs_battery" />
```

### Step 5: Update Button Style

To enhance the appearance of the button, add a style for it in the `themes.xml`:

```xml
<style name="ButtonGeneral">
    <item name="android:layout_width">match_parent</item>
    <item name="android:layout_height">wrap_content</item>
    <item name="android:layout_marginRight">16dp</item>
    <item name="android:layout_marginLeft">16dp</item>
    <item name="android:layout_marginBottom">16dp</item>
</style>

<style name="ButtonGeneral.SecondaryVariant">
    <item name="android:backgroundTint">@color/orange</item>
    <item name="android:textColor">@color/white</item>
    <item name="android:textStyle">bold</item>
</style>
```

In the `activity_main.xml`, change the button definition:

```xml
<Button
    style="@style/ButtonGeneral.SecondaryVariant"
    android:text="@string/buy" />
```

### Step 6: Run Your Application

Run your application to see the changes. It should reflect the new styles and themes applied.

---

## Key Concepts Highlighted

- **Themes**: Control the overall look of your application (e.g., colors, fonts).
- **Styles**: Define the appearance of specific views (e.g., TextView styles).
- **Reusable Patterns**: Styles allow you to maintain consistency and reduce code duplication.

## Safety and Compatibility Notes

- Ensure your app is compatible with different Android versions by testing themes and styles on various devices.
- Always check how your app looks in both light and dark modes.

## Conclusion

Congratulations! You've successfully applied styles and themes in your Android application. This not only improves the aesthetic quality of your app but also enhances user experience. 

For further exploration, consider diving deeper into Material Design components, which provide a plethora of design options for creating modern and user-friendly applications.

### Resources

- [Material Design](https://material.io/design)
- [Material Theme Builder](https://material.io/resources/theme-builder)

---
#### Reference:
1. Dicoding Indonesia
2. Explained by GPT