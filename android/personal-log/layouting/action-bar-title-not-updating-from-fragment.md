### **Date:** October 12, 2024  
### **Issue Title:** Action Bar Title Not Updating from Fragment

---

**Description:**
- While working on `AnalyzeFragment`, I encountered an issue where the title in the Action Bar wasn't being updated correctly. Despite using `activity?.actionBar?.title = "Analyze Image"` in the fragment, the title was still showing the default app name from the `AndroidManifest.xml`.

**Environment:**
- **Device:** Emulator  
- **OS Version:** Android 12  
- **Development Tools:** Android Studio Arctic Fox  
- **Libraries/Dependencies:** None specific to this issue

**Steps to Reproduce:**
1. Create a `Fragment` and try to update the action bar's title by calling `activity?.actionBar?.title = "Analyze Image"`.
2. Observe that the action bar still shows the default title (the app name) rather than the updated title.

**Expected Behavior:**
- The Action Bar title should reflect the string `"Analyze Image"` when the `AnalyzeFragment` is displayed.

**Actual Behavior:**
- The Action Bar title remains as the default app name set in the `AndroidManifest.xml` instead of `"Analyze Image"`.

**Root Cause:**
- The issue occurred because fragments do not directly control the action bar title. Instead, the activity hosting the fragment should handle this, and the title should be set explicitly when the fragment is displayed.

**Resolution Steps:**
1. In the `AnalyzeFragment`, use `activity?.title = "Analyze Image"` instead of `activity?.actionBar?.title = "Analyze Image"`.  
   This ensures the activity’s title is updated directly, which is reflected in the action bar.
   
2. Ensure that the `activity?.title` is set within the `onViewCreated()` method of the fragment to ensure it updates when the fragment is shown.
   
   Example:
   ```kotlin
   activity?.title = "Analyze Image"
   ```

**Lessons Learned:**
- When dealing with the action bar title from a fragment, it's safer to update the `activity?.title` instead of directly modifying the action bar via `actionBar?.title`.
- Fragment lifecycle should be considered when making UI changes that affect the parent activity's components (e.g., the action bar).

**References:**
- [Android Fragments: Communicating with the Activity](https://developer.android.com/guide/fragments/communicate)
