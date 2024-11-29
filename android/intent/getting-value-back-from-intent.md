### **Practice Getting Value Back from Intent**

In this module, we will explore the relationship between `Activity` and `Intent` to receive return values. Often, when one `Activity` starts another, we expect a return value when the second `Activity` finishes and returns to the first one. 

For instance, suppose `Activity A` starts `Activity B` to execute some operation, and then `Activity B` sends back a result before closing using `finish()`. This is how one `Activity` returns a value to the calling `Activity`.

Let’s implement this concept step-by-step.

---

#### **Step 1: Create `MoveForResultActivity`**

**Create a new Empty Views Activity** and name it `MoveForResultActivity`.  
Now, update the layout (`activity_move_for_result.xml`) as follows:

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:layout_marginBottom="16dp"
        android:text="@string/choose_number" />
    
    <!-- RadioGroup with multiple choices -->
    <RadioGroup
        android:id="@+id/rg_number"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical">

        <!-- Each RadioButton represents a number -->
        <RadioButton
            android:id="@+id/rb_50"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/_50" />
        
        <RadioButton
            android:id="@+id/rb_100"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/_100" />
        
        <RadioButton
            android:id="@+id/rb_150"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/_150" />
        
        <RadioButton
            android:id="@+id/rb_200"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:text="@string/_200" />
    </RadioGroup>

    <!-- Button to trigger action and return the result -->
    <Button
        android:id="@+id/btn_choose"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/choose" />
</LinearLayout>
```

This layout contains a `RadioGroup` with four `RadioButton`s representing numbers, and a `Button` to trigger an action.

---

#### **Step 2: Implement Logic in `MoveForResultActivity`**

Now, introduce the components and implement logic in `MoveForResultActivity`.

```kotlin
class MoveForResultActivity : AppCompatActivity(), View.OnClickListener {

    // Late init variables to avoid null initialization
    private lateinit var btnChoose: Button
    private lateinit var rgNumber: RadioGroup

    companion object {
        const val EXTRA_SELECTED_VALUE = "extra_selected_value" // Key to pass data
        const val RESULT_CODE = 110 // Result code for ActivityResult
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_move_for_result)

        // Initialize the button and radio group
        btnChoose = findViewById(R.id.btn_choose)
        rgNumber = findViewById(R.id.rg_number)

        // Set click listener for the button
        btnChoose.setOnClickListener(this)
    }

    // Handle button click to send the selected result back
    override fun onClick(v: View?) {
        if (v?.id == R.id.btn_choose) {
            if (rgNumber.checkedRadioButtonId > 0) {
                var value = 0

                // Determine which RadioButton was selected
                when (rgNumber.checkedRadioButtonId) {
                    R.id.rb_50 -> value = 50
                    R.id.rb_100 -> value = 100
                    R.id.rb_150 -> value = 150
                    R.id.rb_200 -> value = 200
                }

                // Create an Intent to send data back
                val resultIntent = Intent()
                resultIntent.putExtra(EXTRA_SELECTED_VALUE, value)
                setResult(RESULT_CODE, resultIntent)

                // Close this Activity and return to the previous one
                finish()
            }
        }
    }
}
```

**Key Points Explained**:
- The `RadioGroup` is used to group `RadioButton`s, so the selected number can be retrieved.
- The selected value is passed back to the calling `Activity` via an `Intent` and `setResult()` method.
- `finish()` is called to close the `Activity` and return to the caller.

---

#### **Step 3: Modify `MainActivity`**

Add a `Button` and a `TextView` in `activity_main.xml` to trigger `MoveForResultActivity` and display the result.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <!-- Other buttons already existing -->

    <!-- Button to move for result -->
    <Button
        android:id="@+id/btn_move_for_result"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="@string/move_with_result" />

    <!-- TextView to show result from MoveForResultActivity -->
    <TextView
        android:id="@+id/tv_result"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:textSize="24sp"
        android:text="@string/result_from_activity"
        android:gravity="center" />
</LinearLayout>
```

Now update the `MainActivity` code to handle the result.

```kotlin
class MainActivity : AppCompatActivity(), View.OnClickListener {

    private lateinit var tvResult: TextView // To display result

    // Launcher to receive result from MoveForResultActivity
    private val resultLauncher = registerForActivityResult(
        ActivityResultContracts.StartActivityForResult()
    ) { result ->
        if (result.resultCode == MoveForResultActivity.RESULT_CODE && result.data != null) {
            // Retrieve the selected value from the returned Intent
            val selectedValue = result.data?.getIntExtra(MoveForResultActivity.EXTRA_SELECTED_VALUE, 0)
            // Display the selected value in the TextView
            tvResult.text = "Selected Number: $selectedValue"
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        setContentView(R.layout.activity_main)

        // Initialize the result TextView and set button listener
        tvResult = findViewById(R.id.tv_result)
        val btnMoveForResult: Button = findViewById(R.id.btn_move_for_result)
        btnMoveForResult.setOnClickListener(this)
    }

    override fun onClick(v: View?) {
        when (v?.id) {
            R.id.btn_move_for_result -> {
                // Intent to start MoveForResultActivity
                val moveForResultIntent = Intent(this@MainActivity, MoveForResultActivity::class.java)
                // Launch the activity to get a result
                resultLauncher.launch(moveForResultIntent)
            }
        }
    }
}
```

**Key Concepts Highlighted**:
- The `ActivityResultLauncher` is used to start the `MoveForResultActivity` and retrieve the result when the `Activity` finishes.
- The result is checked to ensure that the result is valid and not `null`, and then it is displayed.

---

#### **Step 4: Review Key Concepts and Safety Notes**

- **Best Practice**: Using `registerForActivityResult()` is the modern approach, replacing `startActivityForResult()`, which is deprecated.
- **Safety Note**: Always check if the `Intent` data is not `null` to avoid potential crashes.
- **Reusable Patterns**: The `ActivityResultLauncher` pattern can be reused for any situation where you need a result from another `Activity`, such as selecting a file or an image.

---

### **Conclusion**

By following these steps, you can now handle returning values between Activities using the `Intent` and `ActivityResultLauncher`. This is a common pattern in Android development, making your app more interactive and responsive to user input.

For further reading on **Intents** and **Activity Result Contracts**, check out the official Android documentation.

---

#### **Reference**:
1. Dicoding Indonesia: [Practice Getting Value Back from Intent
](https://www.dicoding.com/academies/51/tutorials/29100?from=1200)
2. Improved by GPT
