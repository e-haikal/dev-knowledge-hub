# Fixing Cut-Off Text in Android TextView

When displaying text in a TextView in Android, it's common to encounter issues where the last line of text appears cut off. This article outlines how to ensure that all lines of text are fully visible in your layouts.

## Common Causes

1. **Incorrect `layout_height`**: Using `0dp` without proper constraints can lead to cut-off text.
2. **Insufficient Space**: Not enough padding or margins can cause the last line to be clipped.

## Solutions

### 1. Change `layout_height` to `wrap_content`

Using `wrap_content` allows the TextView to resize based on its content, ensuring all lines are visible.

```xml
<TextView
    android:id="@+id/tv_item_description"
    android:layout_width="0dp"
    android:layout_height="wrap_content"  <!-- Allows TextView to expand based on content -->
    android:layout_marginTop="8dp"
    android:layout_marginBottom="8dp"
    android:ellipsize="end"
    android:maxLines="5"
    card_view:layout_constraintBottom_toBottomOf="@+id/img_item_photo"
    card_view:layout_constraintEnd_toEndOf="@+id/tv_item_name"
    card_view:layout_constraintStart_toStartOf="@+id/tv_item_name"
    card_view:layout_constraintTop_toBottomOf="@+id/tv_item_name"
    card_view:layout_constraintVertical_bias="0.0"  <!-- Centered vertically -->
    tools:text="@string/description" />
```

### 2. Adjust Vertical Bias

If you need to keep `0dp` for the height, increase the vertical bias to provide more room for the TextView.

```xml
<TextView
    android:id="@+id/tv_item_description"
    android:layout_width="0dp"
    android:layout_height="0dp"
    android:layout_marginTop="8dp"
    android:layout_marginBottom="8dp"
    android:ellipsize="end"
    android:maxLines="5"
    card_view:layout_constraintBottom_toBottomOf="@+id/img_item_photo"
    card_view:layout_constraintEnd_toEndOf="@+id/tv_item_name"
    card_view:layout_constraintStart_toStartOf="@+id/tv_item_name"
    card_view:layout_constraintTop_toBottomOf="@+id/tv_item_name"
    card_view:layout_constraintVertical_bias="0.5"  <!-- Adjusted to center text better -->
    tools:text="@string/description" />
```

### 3. Add Bottom Padding

Adding padding at the bottom of the TextView can help prevent text from being cut off.

```xml
<TextView
    android:id="@+id/tv_item_description"
    android:layout_width="0dp"
    android:layout_height="0dp"
    android:layout_marginTop="8dp"
    android:layout_marginBottom="8dp"
    android:paddingBottom="4dp"  <!-- Adds space inside the TextView -->
    android:ellipsize="end"
    android:maxLines="5"
    card_view:layout_constraintBottom_toBottomOf="@+id/img_item_photo"
    card_view:layout_constraintEnd_toEndOf="@+id/tv_item_name"
    card_view:layout_constraintStart_toStartOf="@+id/tv_item_name"
    card_view:layout_constraintTop_toBottomOf="@+id/tv_item_name"
    card_view:layout_constraintVertical_bias="0.0"  <!-- Keep the bias as needed -->
    tools:text="@string/description" />
```

## Conclusion

To ensure that all lines of text in a TextView are visible, adjust the `layout_height` to `wrap_content`, modify the vertical bias, or add padding. These adjustments can greatly enhance the readability of your app’s UI and provide a better user experience.

#### Reference:
1. Explained by GPT