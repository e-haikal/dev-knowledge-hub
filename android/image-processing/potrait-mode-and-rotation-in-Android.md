### **Portrait Mode & Rotation in Android**

#### **What Happens When You Take a Picture in Portrait Mode:**

- **Portrait mode** = Phone held **upright** (vertical).
- **Camera automatically rotates the image** 90 degrees **clockwise** from **landscape orientation**.
  - **Landscape = horizontal (wider than tall)**
  - **Portrait = vertical (taller than wide)**

#### **Key Points:**

- **Image appears upright to you** but **internally** it’s rotated 90 degrees (landscape).
- **`image.imageInfo.rotationDegrees`** = **90** (because the phone rotated the image 90 degrees).
- The camera **stores this rotation info** so the system can adjust it when displaying the image.
  
#### **Why Do You Need to Rotate?**
- **Models like TensorFlow Lite** expect **landscape images** (horizontal).
- If the image is in **portrait mode**, it needs to be **rotated back to landscape**.

#### **How to Rotate It Back?**
- **`Rot90Op(-image.imageInfo.rotationDegrees / 90)`**:
  - **`rotationDegrees = 90`**.
  - **`90 / 90 = 1`** → This means rotate **1 turn** (90 degrees).
  - **`-1`** → Rotate **counterclockwise** by **90 degrees** to correct portrait image to landscape.

#### **Summary:**

- **Portrait Mode → Camera rotates image 90° clockwise**.
- **Rotation Info**: `rotationDegrees = 90` (portrait).
- **To fix**: Apply **counterclockwise 90° rotation** (`Rot90Op(-1)`).
- **Result**: Image becomes **landscape**.

---

### **Quick Memory Tip:**
- **Portrait → Clockwise 90° rotation** by phone.
- **Fix with counterclockwise 90° rotation** to turn it back to landscape.

---

#### Reference:
1. Explained by GPT