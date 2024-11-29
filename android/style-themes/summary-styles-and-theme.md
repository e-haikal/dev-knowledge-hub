# Summary of Styles and Themes in Android

## Introduction
In Android development, **styles** and **themes** play a crucial role in defining how user interface components are displayed. Understanding their application can enhance your app's design and usability.

## What is a Style?
A **style** is a collection of properties that dictate the appearance of a View or ViewGroup. By centralizing styles, you can easily manage attributes used across multiple components. If changes are needed, you only have to modify one file.

### Defining Styles
Styles are defined in XML files located at `res → values → themes.xml`. Every style must be enclosed within the `<resources>` tag. Each style is created using the `<style>` tag, and individual properties are defined with the `<item>` tag.

**Example:**
```xml
<resources>
    <style name="MyButtonStyle">
        <item name="android:background">#FF0000</item> <!-- Red background -->
        <item name="android:textColor">#FFFFFF</item> <!-- White text -->
    </style>
</resources>
```

To apply a style in your XML layout, use the following attribute:
```xml
<Button
    style="@style/MyButtonStyle"
    android:text="Click Me"/>
```

## What is a Theme?
A **theme** is essentially a style applied to an entire Activity or Application, defined in the `AndroidManifest.xml` file. Themes can enhance visual consistency and improve user experience across your app.

### Benefits of Dark Theme
Implementing a **dark theme** can reduce energy consumption on mobile devices and enhance readability in low-light conditions.

### Material Design
**Material Design**, developed by Google, offers guidelines for creating cohesive user interfaces and experiences in Android applications. 

## Creating a Theme
To create a theme, use the `<style>` tag with a parent theme. For example:
```xml
<style name="MyAppTheme" parent="Theme.Material3.DayNight">
    <item name="colorPrimary">#6200EE</item>
    <item name="colorOnPrimary">#FFFFFF</item>
    <item name="colorSecondary">#03DAC5</item>
    <item name="colorOnSecondary">#000000</item>
</style>
```

### Customizable Attributes
Here are some customizable attributes commonly used in themes:
- **colorPrimary**: The main color of your application, displayed on the Action Bar and buttons.
- **colorOnPrimary**: The color of text/icons displayed over the primary color.
- **colorSecondary**: A secondary main color used in components like the Action Bar and EditText.
- **colorOnSecondary**: The color for text/icons displayed over the secondary color.

For better organization, it’s advisable to define all color variables in the `colors.xml` file.

## Key Concepts
- Styles and themes are essential for UI consistency and ease of modification.
- Dark themes improve readability and reduce battery usage.
- Utilizing Material Design principles can elevate the user experience.

## Safety and Compatibility Notes
- Ensure compatibility with different Android versions by testing themes and styles across devices.
- Use color resources to maintain accessibility standards, especially for colorblind users.

## Practical Applications
Incorporating styles and themes effectively can streamline your design process and create a more engaging user interface. Experiment with different themes and styles to find what works best for your application.

#### Reference:
1. Dicoding Indonesia
2. Explained by GPT