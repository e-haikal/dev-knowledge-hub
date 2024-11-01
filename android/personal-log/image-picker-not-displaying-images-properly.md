#### Date: November 2, 2024
#### Issue Title: Image Picker Not Displaying Images Properly

---

**Description:**
- While testing the image picker on different Android Virtual Devices (AVDs), images were not displaying properly in some cases. On a Pixel 3a (API 34) emulator, no images appeared in the picker, and navigation to other folders was restricted. This prevented any image selection for upload.

**Environment:**
- **Device:** AVD Pixel 3a
- **OS Version:** Android 13 (API 34)
- **Development Tools:** Android Studio Koala 2024.1.2 Patch 1
- **Libraries/Dependencies:** -

**Steps to Reproduce:**
1. Open the app on a Pixel 3a (API 34) emulator.
2. Tap the button to open the image picker.
3. Observe that no images appear in the picker, and folder navigation is restricted.

**Expected Behavior:**
- The image picker should display available images and allow navigation across folders to select an image.

**Actual Behavior:**
- The image picker was empty, and navigation between folders was restricted, making it impossible to select any image.

**Root Cause:**
- The app lacked the necessary permissions for accessing images in external storage, especially required for Android 13 and above.

**Resolution Steps:**
1. Added the following permissions to the Android Manifest:
   ```xml
   <uses-permission android:name="android.permission.READ_EXTERNAL_STORAGE" />
   <uses-permission android:name="android.permission.READ_MEDIA_IMAGES" />
   <uses-permission android:name="android.permission.WRITE_EXTERNAL_STORAGE" android:maxSdkVersion="28"/>
   ```
2. Verified that the image picker now shows images and allows folder navigation.

**Lessons Learned:**
- Permissions for accessing media have changed over Android versions. For Android 13+, the `READ_MEDIA_IMAGES` permission is necessary to access images in external storage, while earlier versions use `READ_EXTERNAL_STORAGE`.

**References:**
- [Android Media Permissions Documentation](https://developer.android.com/training/data-storage)  