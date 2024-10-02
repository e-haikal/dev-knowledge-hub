### Summary of View and ViewGroup

In Android app development, all UI elements are built using two main components: **View** and **ViewGroup**.

- **View**: The basic component that appears on the screen and allows user interaction. Common types of Views include `TextView`, `Button`, `ImageView`, `RadioButton`, and `Checkbox`.

- **ViewGroup**: A special subclass of View that holds and arranges other Views. Common types of ViewGroup include:

  - **LinearLayout**: Arranges components either horizontally or vertically.
  - **RelativeLayout**: Positions components flexibly relative to each other.
  - **FrameLayout**: Allows components to overlap.
  - **TableLayout**: Organizes components into rows and columns.
  - **ConstraintLayout**: Recommended for complex layouts, allowing for efficient view hierarchy creation with features such as:
    - **Relative Positioning**: Positioning components based on others.
    - **Center Positioning & Bias**: Aligning with percentage-based offsets.
    - **Baseline Alignment**: Aligning text across components.
    - **Guideline**: Invisible helper lines.
    - **Barrier**: Positions components based on the size of other components.
    - **Chain**: Arranges components linearly with various distribution styles (Spread, Spread Inside, Weighted, Packed).

- **ScrollView**: Allows for vertical or horizontal scrolling of its content, but can only contain one ViewGroup.

### Practical Tips

- **Adding Images**: Copy images into the `drawable` folder (not `drawable-v24`) to support all Android versions. File names should use lowercase letters and underscores.

- **Changing App Icons**: Right-click the `res` folder → New → Image Asset, select **Launcher Icons**, and save in the `mipmap` folder for various densities.

- **Layout Inspector**: A tool for analyzing the hierarchy and properties of a layout.

- **Using Start and End for Margins**: This practice supports both Left to Right (LTR) and Right to Left (RTL) languages.


---
#### Reference:
1. Dicoding Indonesia
2. Explained by GPT
