
#### Date: November 2, 2024]
#### Issue Title: Image Upload Failed with "File is Not Allowed" Error

---

**Description:**
- Attempted to upload an image to the API endpoint, but received an error message: "File is not allowed." The issue occurred on both a Pixel 7a (API 31) emulator and other devices, causing image upload attempts to fail.

**Environment:**
- **Device:** AVD Pixel 7a
- **OS Version:** Android 12 (API 31)
- **Development Tools:** Android Studio [Specify version]
- **Libraries/Dependencies:** Retrofit, OkHttp for API requests

**Steps to Reproduce:**
1. Open the app and select an image from the gallery.
2. Attempt to upload the selected image.
3. Observe the "File is not allowed" error message.

**Expected Behavior:**
- The image should upload successfully, and the server should return a response indicating successful processing.

**Actual Behavior:**
- The API responded with "File is not allowed," and the upload attempt failed.

**Root Cause:**
- The form-data key in the multipart upload request was set as `"file"` instead of the expected `"photo"` key as specified in the API documentation.

**Resolution Steps:**
1. Modified the form-data key from `"file"` to `"photo"` in the `multipartBody` setup:
   ```kotlin
   val multipartBody = MultipartBody.Part.createFormData(
       "photo",
       imageFile.name,
       requestImageFile
   )
   ```
2. Verified that the API now accepts the upload with the correct key.

**Lessons Learned:**
- Carefully read API documentation for form-data requirements, as the server may expect specific keys. Mismatched keys can prevent successful request processing.

**References:**
- [Dicoding API Documentation](https://classification-api.dicoding.dev/)