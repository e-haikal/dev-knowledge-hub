# A Beginner's Guide to Handling Special Symbols in Android with Kotlin

Special symbols like ampersands (`&`), hashtags (`#`), and quotes can create challenges in Android application development. This guide will walk you through how to effectively handle these characters, especially in XML string resources and Kotlin code.

## Understanding Special Symbols

Special symbols serve different purposes in various contexts:
- **HTML**: Characters like `&` and `<` have specific meanings.
- **URLs**: Characters such as `?`, `&`, and `#` are used for query parameters.
- **JSON**: Certain characters need to be escaped for valid data structures.

Handling these characters properly is crucial for maintaining data integrity and providing a seamless user experience.

## Step-by-Step Guide to Handling Special Symbols

### Step 1: Escaping Special Symbols in XML

When adding string values in your `res/values/strings.xml`, some characters must be escaped to ensure that XML remains valid. The following characters require escaping:

| Symbol | Escaped Version |
|--------|-----------------|
| `&`    | `&amp;`        |
| `<`    | `&lt;`         |
| `>`    | `&gt;`         |
| `"`    | `&quot;`       |
| `'`    | `&apos;`       |

#### Example in `strings.xml`:

```xml
<resources>
    <string name="welcome_message">Hello &amp; welcome to our app!</string>
    <string name="html_example">Use &lt;tag&gt; to format text.</string>
    <string name="quote_example">He said, &quot;Hello!&quot;</string>
</resources>
```

### Step 2: Accessing Strings in Kotlin

You can access the strings defined in `strings.xml` using the `getString()` method in your Kotlin code.

#### Example in Your Activity:

```kotlin
class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        val welcomeTextView: TextView = findViewById(R.id.welcomeTextView)
        welcomeTextView.text = getString(R.string.welcome_message)
    }
}
```

### Step 3: Encoding Special Symbols for URLs

When working with URLs, special symbols need to be encoded. Use `URLEncoder` to format these characters properly.

#### Example:

```kotlin
import java.net.URLEncoder

val originalString = "John & Jane"
val encodedString = URLEncoder.encode(originalString, "UTF-8") // "John+%26+Jane"
```

### Step 4: HTML Encoding for Display

If you are displaying text in a WebView or want to ensure that special symbols render correctly, use HTML encoding.

#### Example:

```kotlin
import android.text.Html

val htmlString = "Hello & welcome!"
val encodedHtmlString = Html.escapeHtml(htmlString) // "Hello &amp; welcome!"
```

### Step 5: Validating User Input

When accepting user input, validate it to avoid issues with special symbols. Use regular expressions to ensure the input meets your criteria.

#### Example:

```kotlin
val regex = Regex("^[a-zA-Z0-9 &]*$") // Allows alphanumeric characters, spaces, and ampersands
val inputText = editText.text.toString()

if (inputText.isNotEmpty() && regex.matches(inputText)) {
    // Process valid input
} else {
    // Show error message
}
```

### Step 6: Testing Your Application

Always test your application with various inputs containing special symbols. This helps ensure that everything functions as expected.

## Conclusion

Handling special symbols in Android development is essential for data integrity and user experience. By following these steps—escaping characters in XML, encoding for URLs and HTML, validating input, and thorough testing—you can manage special symbols effectively in your Kotlin applications.

#### Reference:
1. Dicoding Indonesia
2. Explained by GPT