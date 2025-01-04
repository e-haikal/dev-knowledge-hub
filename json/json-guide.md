### **What is JSON?**
- **JSON** stands for **JavaScript Object Notation**.
- It is a lightweight format for storing and exchanging data.
- Commonly used in web applications and APIs to transfer data between servers and clients.

---

### **Key Characteristics of JSON**
1. **Key-Value Pair Structure**
   - Data is stored as key-value pairs, like:
     ```json
     {
       "key": "value"
     }
     ```
2. **Readable**
   - Easy for both humans and machines to read.
3. **Hierarchical**
   - Supports nesting (a JSON inside another JSON).

---

### **Basic JSON Structure**
1. **Object**: Enclosed in `{}`.
   - Example:
     ```json
     {
       "name": "Alice",
       "age": 25
     }
     ```
2. **Array**: Enclosed in `[]`.
   - Example:
     ```json
     {
       "fruits": ["apple", "banana", "cherry"]
     }
     ```
3. **Nesting**: JSON can nest objects and arrays.
   - Example:
     ```json
     {
       "user": {
         "name": "Alice",
         "details": {
           "age": 25,
           "hobbies": ["reading", "coding"]
         }
       }
     }
     ```

---

### **Data Types in JSON**
1. String: Enclosed in double quotes.
   - `"name": "Alice"`
2. Number: Integers or decimals.
   - `"age": 25`
3. Boolean: True or false.
   - `"isStudent": false`
4. Null: Empty value.
   - `"middleName": null`
5. Array: List of values.
   - `"hobbies": ["reading", "coding"]`
6. Object: Key-value pairs inside `{}`.
   - `"address": { "city": "Yogyakarta", "zip": "55281" }`

---

### **How to Add Fields in JSON**
1. **Single Field**
   - Add a new key-value pair at the top level:
     ```json
     {
       "name": "Alice",
       "age": 25,
       "gender": "female"
     }
     ```
2. **Nested Field**
   - Add data inside an object:
     ```json
     {
       "user": {
         "name": "Alice",
         "details": {
           "age": 25,
           "city": "Yogyakarta"
         }
       }
     }
     ```

---

### **Connecting Fields**
1. **Nesting**
   - Objects within objects to represent relationships:
     ```json
     {
       "employee": {
         "id": 1,
         "personal": {
           "name": "John Doe",
           "age": 30
         }
       }
     }
     ```
2. **Using Arrays**
   - Store multiple related items:
     ```json
     {
       "team": [
         {
           "name": "Alice",
           "role": "Developer"
         },
         {
           "name": "Bob",
           "role": "Designer"
         }
       ]
     }
     ```

---

### **Working with JSON in Your App**

#### **1. Store JSON in a File**
- Save your JSON as a `.json` file:
  ```json
  {
    "id": 1,
    "title": "Makharijul Huruf",
    "description": "Tempat keluarnya huruf-huruf Hijaiyah"
  }
  ```

#### **2. Parse JSON**
- In Android Kotlin, use libraries like `Gson` or `Moshi` to parse JSON into objects.

#### **Example: Parsing JSON**
```kotlin
// Add Gson dependency in build.gradle
implementation("com.google.code.gson:gson:2.10.1")

// Example JSON string
val json = """
    {
      "id": 1,
      "title": "Makharijul Huruf",
      "description": "Tempat keluarnya huruf-huruf Hijaiyah"
    }
"""

// Define a data class
data class Material(val id: Int, val title: String, val description: String)

// Parse JSON string to Kotlin object
val gson = Gson()
val material = gson.fromJson(json, Material::class.java)

// Access the data
println("ID: ${material.id}, Title: ${material.title}")
```

#### **3. Convert Kotlin Object to JSON**
```kotlin
// Create a Kotlin object
val newMaterial = Material(2, "Hukum Mad", "Tanda Panjang dalam bacaan Al-Qur’an")

// Convert to JSON string
val materialJson = gson.toJson(newMaterial)
println(materialJson)
```

---

### **Key Tools for JSON in Android**
1. **Gson**: Easy-to-use library for parsing JSON.
2. **Moshi**: Similar to Gson but with better Kotlin support.
3. **Retrofit**: For working with APIs that return JSON data.

---

### **Practical Example: JSON in a File**

#### **JSON File (materials.json)**
```json
[
  {
    "id": 1,
    "title": "Tentang Tajwid",
    "description": "Makna dari ilmu tahsin dan tajwid"
  },
  {
    "id": 2,
    "title": "Makharijul Huruf",
    "description": "Tempat keluarnya huruf-huruf Hijaiyah"
  }
]
```

#### **Reading JSON File in Kotlin**
```kotlin
fun loadJSONFromAsset(context: Context, fileName: String): String? {
    return try {
        val inputStream = context.assets.open(fileName)
        val size = inputStream.available()
        val buffer = ByteArray(size)
        inputStream.read(buffer)
        inputStream.close()
        String(buffer, Charsets.UTF_8)
    } catch (ex: IOException) {
        ex.printStackTrace()
        null
    }
}

val jsonString = loadJSONFromAsset(context, "materials.json")
val materialsList: List<Material> = gson.fromJson(jsonString, Array<Material>::class.java).toList()

materialsList.forEach {
    println("ID: ${it.id}, Title: ${it.title}")
}
```

---

### **Summary**
1. **JSON Basics**:
   - Key-value pairs, nesting, and arrays.
2. **Tools**:
   - Use Gson/Moshi to handle JSON in Kotlin.
3. **Practical Steps**:
   - Parse JSON from strings or files.
   - Convert objects to JSON for API communication.


##### Reference
- GPT