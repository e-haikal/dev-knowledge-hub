#### Date: November 10, 2024
#### Issue Title: Cropping Cancelation Not Updating Image in ResultActivity

---

**Description:**
- After selecting an image (Image B) and initiating the cropping process, the user decides to cancel the crop. When the user returns to the `ResultActivity`, it still shows the previous image (Image A) instead of the selected Image B, even though the cropping was canceled. This results in incorrect display of the image in the ResultActivity.

**Environment:**
- **Device:** On AVD & Physical Device
- **OS Version:** Android 12
- **Development Tools:** Android Studio Koala
- **Libraries/Dependencies:**
  - UCrop library (for cropping images)
  - ViewModel (for managing image URI)

**Steps to Reproduce:**
1. Open `ResultActivity` which initially shows Image A.
2. Pick an image (Image B) and initiate the cropping process.
3. Cancel the cropping process.
4. Return to the `ResultActivity`.
5. Image A is still displayed instead of Image B.

**Expected Behavior:**
- After canceling the cropping process, the selected image (Image B) should still be displayed in `ResultActivity`, even if cropping was not applied.

**Actual Behavior:**
- After canceling cropping, Image A is still shown in `ResultActivity` instead of Image B, even though Image B was selected prior to cropping.

**Root Cause:**
- The issue occurs because the `ResultActivity` does not correctly handle the case where the user cancels cropping. The image URI is not updated after cancellation, so the activity defaults to the previously set image URI (Image A).

**Resolution Steps:**
1. In the `AnalyzeFragment`, after detecting the `RESULT_CANCELED` status from the cropping activity, the `currentImageUri` (which holds the selected image URI) should be used to update the preview image.
   - Code added to check `Activity.RESULT_CANCELED` and use the selected image URI:
     ```kotlin
     // If cropping was canceled, just use the selected image URI
     viewModel.currentImageUri?.let {
         showImage()  // Show the selected image
     }
     ```

    - Here's the full of code
        ```kotlin
        package com.dicoding.asclepius.view.analyze
        import android.app.Activity
        import android.app.Activity.RESULT_OK
        import android.content.Intent
        import android.net.Uri
        import android.os.Bundle
        import android.util.Log
        import androidx.fragment.app.Fragment
        import android.view.LayoutInflater
        import android.view.View
        import android.view.ViewGroup
        import android.widget.Toast
        import androidx.activity.result.PickVisualMediaRequest
        import androidx.activity.result.contract.ActivityResultContracts
        import androidx.fragment.app.viewModels
        import com.dicoding.asclepius.R
        import com.dicoding.asclepius.databinding.FragmentAnalyzeBinding
        import com.dicoding.asclepius.helper.ImageClassifierHelper
        import com.dicoding.asclepius.view.ResultActivity
        import com.yalantis.ucrop.UCrop
        import org.tensorflow.lite.task.vision.classifier.Classifications
        import java.io.File

        class AnalyzeFragment : Fragment() {

            private var _binding : FragmentAnalyzeBinding? = null  // View binding for the layout
            private val binding get() = _binding!!

            private val viewModel: AnalyzeViewModel by viewModels()  // ViewModel for managing image URI

            override fun onCreateView(
                inflater: LayoutInflater, container: ViewGroup?,
                savedInstanceState: Bundle?
            ): View? {
                _binding = FragmentAnalyzeBinding.inflate(inflater, container, false)
                return binding.root
            }

            override fun onViewCreated(view: View, savedInstanceState: Bundle?) {
                super.onViewCreated(view, savedInstanceState)

                activity?.title = "Analyze your Skin"

                showImage()  // Displays the image if one is already selected

                // Opens the gallery to select an image
                binding.galleryButton.setOnClickListener {
                    startGallery()
                }

                // Starts the analysis process on the selected image
                binding.analyzeButton.setOnClickListener {
                    analyzeImage()
                }
            }

            // Launches the gallery to pick an image
            private fun startGallery() {
                launcherGallery.launch(PickVisualMediaRequest(ActivityResultContracts.PickVisualMedia.ImageOnly))
            }

            // Handles the result of the image selection from the gallery
            private val launcherGallery = registerForActivityResult(
                ActivityResultContracts.PickVisualMedia()
            ) { uri: Uri? ->
                if (uri != null) {
                    viewModel.currentImageUri = uri  // Saves the selected image URI in the ViewModel
                    startCrop(uri)  // Starts the cropping process
                } else {
                    Log.d("Photo Picker", getString(R.string.no_media_selected))
                    showToast(getString(R.string.no_media_selected))  // Shows a message if no image was selected
                }
            }

            // Displays the selected or cropped image in the preview
            private fun showImage() {
                viewModel.currentImageUri?.let {
                    Log.d("Image URI", "showImage: $it")
                    binding.previewImageView.setImageURI(it)  // Sets the preview image view to display the URI
                }
            }

            // Analyzes the selected image using the ImageClassifierHelper
            private fun analyzeImage() {
                viewModel.currentImageUri?.let { uri ->
                    val imageClassifierHelper = ImageClassifierHelper(
                        context = requireContext(),
                        classifierListener = object : ImageClassifierHelper.ClassifierListener {
                            // Displays an error if classification fails
                            override fun onError(error: String) {
                                showToast("Error: $error")
                            }

                            // Processes the classification result and displays it
                            override fun onResult(results: List<Classifications>?, inferenceTime: Long) {
                                val resultText = results?.joinToString("\n") { classification ->
                                    classification.categories.joinToString(", ") {
                                        "${it.label} ${"%.2f".format(it.score * 100)}%"
                                    }
                                }
                                resultText?.let {
                                    moveToResult(uri, it)  // Moves to ResultActivity with the analysis result
                                } ?: showToast(getString(R.string.tidak_ada_hasil_yang_ditemukan))
                            }
                        }
                    )

                    // Begins the image classification process
                    imageClassifierHelper.classifyStaticImage(uri)
                } ?: run {
                    showToast(getString(R.string.pilih_gambar_terlebih_dahulu))  // Shows a message if no image was selected
                }
            }

            // Shows a toast message with the given string
            private fun showToast(message: String) {
                Toast.makeText(requireContext(), message, Toast.LENGTH_SHORT).show()
            }

            // Starts ResultActivity and passes the image URI and analysis result
            private fun moveToResult(imageUri: Uri, resultText: String) {
                val intent = Intent(requireContext(), ResultActivity::class.java).apply {
                    putExtra("IMAGE_URI", imageUri)
                    putExtra("RESULT_TEXT", resultText)
                }
                startActivity(intent)
            }

            // Handles the result of the cropping process
            private val cropActivityResultLauncher = registerForActivityResult(
                ActivityResultContracts.StartActivityForResult()
            ) { result ->
                if (result.resultCode == RESULT_OK) {
                    val data = result.data
                    val resultUri = data?.let { UCrop.getOutput(it) }  // Gets the cropped image URI
                    if (resultUri != null) {
                        viewModel.currentImageUri = resultUri  // Updates the image URI with the cropped image
                        showImage()
                    } else {
                        showToast(getString(R.string.error_no_image_returned_after_cropping))
                    }
                } else {
                    if (result.resultCode == Activity.RESULT_CANCELED) {
                        Log.d("uCrop", "Crop cancelled by user")
                        // If cropping was canceled, just use the selected image URI
                        viewModel.currentImageUri?.let {
                            showImage()  // Show the selected image
                        }
                    } else {
                        val cropError = UCrop.getError(result.data!!)
                        cropError?.let {
                            showToast("Crop failed: ${it.message}")  // Shows a message if cropping failed
                        }
                    }
                }
            }

            // Starts the cropping activity using uCrop library
            private fun startCrop(uri: Uri) {
                val destinationUri = Uri.fromFile(File(requireContext().cacheDir, "cropped_${System.currentTimeMillis()}.jpg"))
                val uCrop = UCrop.of(uri, destinationUri)
                uCrop.withAspectRatio(1f, 1f)  // Sets a square crop aspect ratio
                uCrop.withMaxResultSize(1000, 1000)  // Limits the maximum result size

                val intent = uCrop.getIntent(requireContext())
                cropActivityResultLauncher.launch(intent)
            }

            // Cleans up the binding when the fragment is destroyed
            override fun onDestroy() {
                super.onDestroy()
                _binding = null
            }
        }

        ```

     









**Lessons Learned:**
- Ensure that the correct image URI is always passed and displayed in the UI, even in edge cases such as user cancellations. It's important to handle all user interaction cases (like canceling cropping) to maintain a consistent user experience.
- Using `ViewModel` to store and manage the image URI allows for smoother transitions and ensures that the selected image is always available when needed.

**References:**
- [UCrop Documentation](https://github.com/Yalantis/uCrop)
- [Android Activity Result API](https://developer.android.com/training/basics/intents/result)
