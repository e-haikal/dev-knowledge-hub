# Two Approaches to Using `Intent` in Kotlin Android for Multiple Activities: Direct Declaration vs. Method Overriding

In Android development, you often need to move between multiple activities. There are two common approaches to achieving this using `Intent`: direct declaration inside an `onClickListener` or handling it by overriding methods. 

---

## 1. Direct Declaration of `Intent` for Multiple Activities

With direct declaration, each `Intent` is declared inside the respective `onClickListener` for each button. This method is simple to implement, but can become cumbersome if you handle many buttons.

### Example: Direct Declaration for Two Activities

```kotlin
package com.example.myintentapp

import android.content.Intent
import android.os.Bundle
import android.widget.Button
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity() {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Find and set click listeners for both buttons
        val moveActivity1Button = findViewById<Button>(R.id.moveActivity1Button)
        val moveActivity2Button = findViewById<Button>(R.id.moveActivity2Button)
        
        // Directly declare intent to move to the first activity
        moveActivity1Button.setOnClickListener {
            val intent = Intent(this, FirstActivity::class.java)
            startActivity(intent)
        }

        // Directly declare intent to move to the second activity
        moveActivity2Button.setOnClickListener {
            val intent = Intent(this, SecondActivity::class.java)
            startActivity(intent)
        }
    }
}
```

### Key Points:
- Each button has its own `onClickListener` and directly declares the `Intent`.
- Simple for a small number of buttons, but harder to manage as your app grows.
- Code for handling each button is placed inside the `onCreate()` function.

---

## 2. Using Method Overriding to Handle Multiple Activities

By overriding the `onClick()` method, you can centralize the logic to handle clicks from multiple buttons. This approach is more scalable and clean when dealing with multiple UI elements.

### Example: Method Overriding for Two Activities

```kotlin
package com.example.myintentapp

import android.content.Intent
import android.os.Bundle
import android.view.View
import android.widget.Button
import androidx.appcompat.app.AppCompatActivity

class MainActivity : AppCompatActivity(), View.OnClickListener {
    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Set listeners for both buttons
        val moveActivity1Button = findViewById<Button>(R.id.moveActivity1Button)
        val moveActivity2Button = findViewById<Button>(R.id.moveActivity2Button)

        // Set this class as the click listener for both buttons
        moveActivity1Button.setOnClickListener(this)
        moveActivity2Button.setOnClickListener(this)
    }

    // Override the onClick method to handle multiple buttons
    override fun onClick(v: View?) {
        when (v?.id) {
            R.id.moveActivity1Button -> {
                val intent = Intent(this, FirstActivity::class.java)
                startActivity(intent)
            }
            R.id.moveActivity2Button -> {
                val intent = Intent(this, SecondActivity::class.java)
                startActivity(intent)
            }
        }
    }
}
```

### Key Points:
- The `onClick()` method is overridden to handle multiple buttons.
- Uses a `when` statement to manage different buttons in a centralized way.
- More scalable and organized for larger projects.

---

## Key Differences

- **Direct Declaration**: For each button, the `Intent` is declared separately in its own `onClickListener`. This approach is ideal for small apps with only a few buttons, but it can clutter the `onCreate()` method as more buttons are added.
  
- **Method Overriding**: Centralizes the logic for multiple buttons in the `onClick()` method using a `when` statement. This approach is more suitable for apps with many UI elements, as it helps maintain clean and organized code.

---

## Conclusion

- Use **Direct Declaration** when your app has only a few buttons or activities to manage.
- Use **Method Overriding** when your app grows in complexity, and you need a more maintainable and scalable solution.
