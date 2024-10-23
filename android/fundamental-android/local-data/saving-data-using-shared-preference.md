# Understanding SharedPreferences in Android: A Complete Guide

SharedPreferences is a lightweight framework in Android for storing user preferences and application settings. It allows you to save key-value pairs, making it ideal for storing simple data like user preferences.

### Basic Concepts of SharedPreferences

- **Key-Value Pairs**: Data is stored as key-value pairs, where each key is unique.
- **Modes**: You can access SharedPreferences in different modes, typically `MODE_PRIVATE`, which allows only your application to access the preferences.
- **Synchronous and Asynchronous Operations**: Changes to preferences can be committed synchronously or asynchronously.

## Complete Code Example

### 1. User Model

**Purpose**: Create a data model to represent user data.

```kotlin
// UserModel.kt
package com.siaptekno.mysharedpreferences

import android.os.Parcelable
import kotlinx.parcelize.Parcelize

@Parcelize
data class UserModel (
    var name: String? = null, // User's name
    var email: String? = null, // User's email
    var age: Int = 0, // User's age
    var phoneNumber: String? = null, // User's phone number
    var isLove: Boolean = false // User's preference for Manchester United
): Parcelable
```

**Key Points**:
- Use `@Parcelize` for easy passing of the model between activities.
- Store all relevant user fields for easy access.

### 2. SharedPreferences Wrapper

**Purpose**: Create a class to handle the saving and retrieving of user data using SharedPreferences.

```kotlin
// UserPreference.kt
package com.siaptekno.mysharedpreferences

import android.content.Context

internal class UserPreference(context: Context) {
    companion object {
        private const val PREFS_NAME = "user_pref" // Name of the preferences file
        private const val NAME = "name" // Key for the name
        private const val EMAIL = "email" // Key for the email
        private const val AGE = "age" // Key for the age
        private const val PHONE_NUMBER = "phone" // Key for the phone number
        private const val LOVE_MU = "islove" // Key for love for Manchester United
    }

    // Accessing SharedPreferences
    private val preferences = context.getSharedPreferences(PREFS_NAME, Context.MODE_PRIVATE)

    // Function to save user data
    fun setUser(value: UserModel) {
        val editor = preferences.edit() // Start editing preferences
        editor.putString(NAME, value.name) // Save name
        editor.putString(EMAIL, value.email) // Save email
        editor.putInt(AGE, value.age) // Save age
        editor.putString(PHONE_NUMBER, value.phoneNumber) // Save phone number
        editor.putBoolean(LOVE_MU, value.isLove) // Save love preference
        editor.apply() // Commit changes asynchronously
    }

    // Function to retrieve user data
    fun getUser(): UserModel {
        val model = UserModel() // Create a new UserModel instance
        model.name = preferences.getString(NAME, "") // Retrieve name
        model.email = preferences.getString(EMAIL, "") // Retrieve email
        model.age = preferences.getInt(AGE, 0) // Retrieve age
        model.phoneNumber = preferences.getString(PHONE_NUMBER, "") // Retrieve phone number
        model.isLove = preferences.getBoolean(LOVE_MU, false) // Retrieve love preference
        return model // Return the populated UserModel
    }
}
```

**Key Points**:
- Encapsulate SharedPreferences access in a dedicated class for cleaner code.
- Use `apply()` for asynchronous changes to preferences.

### 3. Main Activity

**Purpose**: Display user information and provide a way to edit the profile.

```kotlin
// MainActivity.kt
package com.siaptekno.mysharedpreferences

import android.content.Intent
import android.os.Bundle
import android.view.View
import androidx.activity.result.ActivityResult
import androidx.activity.result.contract.ActivityResultContracts
import androidx.activity.result.registerForActivityResult
import androidx.appcompat.app.AppCompatActivity
import com.siaptekno.mysharedpreferences.databinding.ActivityMainBinding

class MainActivity : AppCompatActivity(), View.OnClickListener {
    private lateinit var mUserPreference: UserPreference
    private var isPreferenceEmpty = false
    private lateinit var userModel: UserModel
    private lateinit var binding: ActivityMainBinding

    private val resultLauncher = registerForActivityResult(
        ActivityResultContracts.StartActivityForResult()
    ) { result: ActivityResult ->
        if (result.data != null && result.resultCode == FormUserPreferenceActivity.RESULT_CODE) {
            userModel = result.data?.getParcelableExtra<UserModel>(FormUserPreferenceActivity.EXTRA_RESULT) as UserModel
            populateView(userModel)
            checkForm(userModel)
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityMainBinding.inflate(layoutInflater)
        setContentView(binding.root)

        mUserPreference = UserPreference(this) // Initialize UserPreference
        showExistingPreference() // Load existing preferences

        binding.btnSave.setOnClickListener(this) // Set button click listener
    }

    private fun showExistingPreference() {
        userModel = mUserPreference.getUser() // Retrieve user data
        populateView(userModel) // Populate the UI with user data
        checkForm(userModel) // Check if the form needs to change
    }

    private fun populateView(userModel: UserModel) {
        binding.tvName.text = if (userModel.name.toString().isEmpty()) "Tidak Ada" else userModel.name // Show name
        binding.tvAge.text = if (userModel.age.toString().isEmpty()) "Tidak Ada" else userModel.age.toString() // Show age
        binding.tvIsLoveMu.text = if (userModel.isLove) "Ya" else "Tidak" // Show love preference
        binding.tvEmail.text = if (userModel.email.toString().isEmpty()) "Tidak Ada" else userModel.email // Show email
        binding.tvPhone.text = if (userModel.phoneNumber.toString().isEmpty()) "Tidak Ada" else userModel.phoneNumber // Show phone number
    }

    private fun checkForm(userModel: UserModel) {
        when {
            userModel.name.toString().isNotEmpty() -> {
                binding.btnSave.text = getString(R.string.change) // Change button text
                isPreferenceEmpty = false
            }
            else -> {
                binding.btnSave.text = getString(R.string.save) // Default button text
                isPreferenceEmpty = true
            }
        }
    }

    override fun onClick(view: View) {
        if (view.id == R.id.btn_save) {
            val intent = Intent(this@MainActivity, FormUserPreferenceActivity::class.java) // Start FormUserPreferenceActivity
            when {
                isPreferenceEmpty -> {
                    intent.putExtra(FormUserPreferenceActivity.EXTRA_TYPE_FORM, FormUserPreferenceActivity.TYPE_ADD) // Add new user
                }
                else -> {
                    intent.putExtra(FormUserPreferenceActivity.EXTRA_TYPE_FORM, FormUserPreferenceActivity.TYPE_EDIT) // Edit existing user
                    intent.putExtra("USER", userModel) // Pass current user data
                }
            }
            resultLauncher.launch(intent) // Launch the form activity
        }
    }
}
```

**Key Points**:
- Use `registerForActivityResult` for handling results from other activities.
- Populate the UI based on existing user preferences.

### 4. Form User Preference Activity

**Purpose**: Allow users to input or update their profile information.

```kotlin
// FormUserPreferenceActivity.kt
package com.siaptekno.mysharedpreferences

import android.content.Intent
import android.os.Bundle
import android.text.TextUtils
import android.view.MenuItem
import android.view.View
import android.widget.Toast
import androidx.appcompat.app.AppCompatActivity
import androidx.core.view.ViewCompat
import androidx.core.view.WindowInsetsCompat
import com.siaptekno.mysharedpreferences.databinding.ActivityFormUserPreferenceBinding

class FormUserPreferenceActivity : AppCompatActivity(), View.OnClickListener {

    companion object {
        const val EXTRA_TYPE_FORM = "extra_type_form"
        const val EXTRA_RESULT = "extra_result"
        const val RESULT_CODE = 101
        const val TYPE_ADD = 1
        const val TYPE_EDIT = 2
        private const val FIELD_REQUIRED = "Field tidak boleh kosong"
        private const val FIELD_DIGIT_ONLY = "Hanya boleh terisi numerik"
        private const val FIELD_IS_NOT_VALID = "Email tidak valid"
    }

    private lateinit var userModel: UserModel
    private lateinit var binding: ActivityFormUserPreferenceBinding

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        binding = ActivityFormUserPreferenceBinding.inflate(layoutInflater)
        setContentView(binding.root)

        binding.btnSave.setOnClickListener(this) // Set button click listener

        // Handle window insets for padding
        ViewCompat.setOnApplyWindowInsetsListener(findViewById(R.id.activity_form_user_preference)) { v, insets ->
            val systemBars = insets.getInsets(WindowInsetsCompat.Type.systemBars())
            v.setPadding(systemBars.left, systemBars.top, systemBars.right, systemBars.bottom)
            insets
        }

        userModel = intent.getParcelableExtra<UserModel>("USER") as UserModel // Get user data from intent
        val formType = intent.getIntExtra(EXTRA_TYPE_FORM, 0) // Get the form type

        var actionBarTitle = ""
       

 var btnTitle = ""
        when (formType) {
            TYPE_ADD -> {
                actionBarTitle = "Tambah Baru" // Add new user
                btnTitle = "Simpan" // Button title
            }
            TYPE_EDIT -> {
                actionBarTitle = "Ubah" // Edit existing user
                btnTitle = "Update" // Button title
                showPreferenceInForm() // Populate form with existing data
            }
        }
        supportActionBar?.title = actionBarTitle // Set action bar title
        supportActionBar?.setDisplayHomeAsUpEnabled(true) // Enable back button
        binding.btnSave.text = btnTitle // Set button title
    }

    override fun onClick(view: View) {
        if (view.id == R.id.btn_save) {
            val name = binding.edtName.text.toString().trim() // Get name input
            val email = binding.edtEmail.text.toString().trim() // Get email input
            val age = binding.edtAge.text.toString().trim() // Get age input
            val phoneNo = binding.edtPhone.text.toString().trim() // Get phone number input
            val isLoveMU = binding.rgLoveMu.checkedRadioButtonId == R.id.rb_yes // Get love preference

            // Validate input fields
            if (name.isEmpty()) {
                binding.edtName.error = FIELD_REQUIRED
                return
            }
            if (email.isEmpty()) {
                binding.edtEmail.error = FIELD_REQUIRED
                return
            }
            if (!isValidEmail(email)) {
                binding.edtEmail.error = FIELD_IS_NOT_VALID
                return
            }
            if (age.isEmpty()) {
                binding.edtAge.error = FIELD_REQUIRED
                return
            }
            if (phoneNo.isEmpty()) {
                binding.edtPhone.error = FIELD_REQUIRED
                return
            }
            if (!TextUtils.isDigitsOnly(phoneNo)) {
                binding.edtPhone.error = FIELD_DIGIT_ONLY
                return
            }

            saveUser(name, email, age, phoneNo, isLoveMU) // Save user data
            val resultIntent = Intent()
            resultIntent.putExtra(EXTRA_RESULT, userModel) // Pass the user model back
            setResult(RESULT_CODE, resultIntent) // Set result code
            finish() // Close activity
        }
    }

    private fun saveUser(name: String, email: String, age: String, phoneNo: String, isLoveMU: Boolean) {
        val userPreference = UserPreference(this) // Create instance of UserPreference
        userModel.name = name // Set the name
        userModel.email = email // Set the email
        userModel.age = Integer.parseInt(age) // Set the age
        userModel.phoneNumber = phoneNo // Set the phone number
        userModel.isLove = isLoveMU // Set love preference
        userPreference.setUser(userModel) // Save user data
        Toast.makeText(this, "Data tersimpan", Toast.LENGTH_SHORT).show() // Show confirmation
    }

    private fun isValidEmail(email: String): Boolean {
        return android.util.Patterns.EMAIL_ADDRESS.matcher(email).matches() // Validate email format
    }

    private fun showPreferenceInForm() {
        binding.edtName.setText(userModel.name) // Populate name field
        binding.edtEmail.setText(userModel.email) // Populate email field
        binding.edtAge.setText(userModel.age.toString()) // Populate age field
        binding.edtPhone.setText(userModel.phoneNumber) // Populate phone number field
        if (userModel.isLove) {
            binding.rbYes.isChecked = true // Set radio button for 'Yes'
        } else {
            binding.rbNo.isChecked = true // Set radio button for 'No'
        }
    }

    override fun onOptionsItemSelected(item: MenuItem): Boolean {
        if (item.itemId == android.R.id.home) {
            finish() // Handle back button
        }
        return super.onOptionsItemSelected(item)
    }
}
```

**Key Points**:
- Use intents to pass data between activities.
- Validate user input before saving.
- Populate form fields with existing data for editing.

### 5. Layout XML Files

**Purpose**: Define the UI for your activities.

**activity_main.xml**

```xml
<!-- Activity layout for MainActivity -->
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:id="@+id/tvName"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

    <TextView
        android:id="@+id/tvEmail"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

    <TextView
        android:id="@+id/tvAge"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

    <TextView
        android:id="@+id/tvPhone"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

    <TextView
        android:id="@+id/tvIsLoveMu"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

    <Button
        android:id="@+id/btn_save"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="@string/save"/>
</LinearLayout>
```

**Key Points**:
- Use appropriate UI elements like `TextView` and `Button` for displaying and interacting with data.
- Ensure the layout is user-friendly and accessible.

**activity_form_user_preference.xml**

```xml
<!-- Activity layout for FormUserPreferenceActivity -->
<LinearLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:padding="16dp">

    <EditText
        android:id="@+id/edtName"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Name"/>

    <EditText
        android:id="@+id/edtEmail"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Email"/>

    <EditText
        android:id="@+id/edtAge"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Age"/>

    <EditText
        android:id="@+id/edtPhone"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:hint="Phone Number"/>

    <RadioGroup
        android:id="@+id/rgLoveMu"
        android:layout_width="match_parent"
        android:layout_height="wrap_content">
        <RadioButton
            android:id="@+id/rb_yes"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="I Love MU"/>
        <RadioButton
            android:id="@+id/rb_no"
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="I Don't Love MU"/>
    </RadioGroup>

    <Button
        android:id="@+id/btn_save"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Save"/>
</LinearLayout>
```

**Key Points**:
- Layout files should match the functionality of your activities.
- Use `EditText` for user input and `RadioGroup` for choices.

### Conclusion

By following this structured approach, you can implement SharedPreferences in your Android app effectively. This guide provides the necessary steps to manage user preferences, making your applications more user-friendly.

### Key Takeaways:
- **Data Modeling**: Use data classes to represent the information you need to store.
- **Encapsulation**: Wrap SharedPreferences in a separate class for better organization.
- **Activity Communication**: Use intents to pass data between activities.
- **Input Validation**: Always validate user input before processing or saving.
- **UI Design**: Design user-friendly layouts for input and display.

--- 
#### Reference
1. Dicoding Indonesia
2. Explained by GPT