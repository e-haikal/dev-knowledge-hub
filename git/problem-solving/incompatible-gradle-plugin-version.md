#### Date: June 24, 2025

#### Issue Title: Incompatible Android Gradle Plugin Version (AGP 8.7.2)

---

**Description:**

* The project failed to build due to the usage of an Android Gradle Plugin (AGP) version (8.7.2) that is not supported by the current version of Android Studio. The IDE flagged the incompatibility and recommended downgrading to a supported AGP version.

**Environment:**

* **Device:** Emulator
* **OS Version:** Android 13
* **Development Tools:** Android Studio Iguana (Patch 1)
* **Libraries/Dependencies:**

  * AGP 8.7.2 (initially)
  * Gradle 8.5

**Steps to Reproduce:**

1. Open the project in Android Studio
2. Attempt to sync Gradle/build the project

**Expected Behavior:**

* Project should sync and build successfully using the defined Gradle and AGP versions.

**Actual Behavior:**

* Android Studio displayed an error:
  *"The project is using an incompatible version (AGP 8.7.2) of the Android Gradle plugin. Latest supported version is AGP 8.6.1."*

  * Build and sync were blocked.

**Root Cause:**

* The Android Gradle Plugin version (8.7.2) was newer than what the current version of Android Studio supported.

**Resolution Steps:**

1. Opened the project-level `build.gradle.kts` file
2. Changed:

   ```kotlin
   id("com.android.application") version "8.7.2"
   ```

   to:

   ```kotlin
   id("com.android.application") version "8.6.1"
   ```
3. Synced the project again
4. Project built successfully without errors

**Lessons Learned:**

* Always check the compatibility between the Android Gradle Plugin (AGP) and the Android Studio version in use.
* Using the latest AGP isn't always better if your tools aren't fully updated.

**References:**

* [Android Gradle Plugin Release Notes](https://developer.android.com/studio/releases/gradle-plugin#updating-gradle)
* Error message and recommendation from Android Studio

