# **Tutorial: How to Download a PDF from Google Drive with Custom Layouts (Vertical and Landscape)**

This tutorial shows you how to download a PDF from Google Drive, either in the **default vertical format** (portrait) or in a **landscape format with a 16:9 ratio**. Follow the instructions in the corresponding sections for your preferred layout.

---

## **Version 1: Download PDF for Vertical (Default) Documents**

This version is for documents that are in the default **vertical (portrait)** format. Typically, this would apply to standard documents or presentations in a portrait layout.

### **Steps to Download Vertical PDF**

1. **Open the PDF File in Google Drive**
   - Navigate to the Google Drive link containing the document.
   - Open the document in your browser.

2. **Open Developer Tools in Google Chrome**
   - Right-click anywhere on the page and select **Inspect** or press `Ctrl + Shift + I` (Windows) or `Cmd + Option + I` (Mac) to open Developer Tools.
   - Go to the **Console** tab in Developer Tools.

3. **Insert the JavaScript Code for Vertical Format**

   Copy and paste the following script into the console. This will ensure the PDF is downloaded in the **default vertical format**.

   ```javascript
   let jspdf = document.createElement( "script" );
   jspdf.onload = function () {
     let pdf = new jsPDF('portrait', 'mm', [210, 297]); // Portrait format (A4)
     let elements = document.getElementsByTagName( "img" );
     
     for ( let i in elements) {
       let img = elements[i];
       if (!/^blob:/.test(img.src)) {
         continue;
       }
   
       let canvasElement = document.createElement( 'canvas' );
       let con = canvasElement.getContext( "2d" );
       canvasElement.width = img.width;
       canvasElement.height = img.height;
       con.drawImage(img, 0, 0,img.width, img.height);
       let imgData = canvasElement.toDataURL( "image/jpeg", 1.0);
       
       // Add the image to the PDF with correct position and size (A4 portrait size)
       pdf.addImage(imgData, 'JPEG', 0, 0, 210, 297); // 210x297mm A4 portrait size
       pdf.addPage();
     }
     
     pdf.save( "download.pdf" );
   };
   
   jspdf.src = 'https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.3.2/jspdf.min.js';
   document.body.appendChild(jspdf);
   ```

4. **Execute the Script**
   - Press `Enter` to execute the script. The script will capture the images from the document and save them in a PDF format with vertical orientation.
   - The PDF will automatically download to your device.

---

## **Version 2: Download PDF for Landscape (16:9) Documents**

This version is for documents that need to be in a **landscape layout** with a **16:9 aspect ratio**. This is often used for presentations, slides, or media that have a horizontal layout.

### **Steps to Download Landscape PDF**

1. **Open the PDF File in Google Drive**
   - Open the document you want to download in your browser.

2. **Open Developer Tools in Google Chrome**
   - Right-click anywhere on the page and select **Inspect** or press `Ctrl + Shift + I` (Windows) or `Cmd + Option + I` (Mac) to open Developer Tools.
   - Go to the **Console** tab in Developer Tools.

3. **Insert the JavaScript Code for Landscape Format**

   Copy and paste the following script into the console. This will ensure the PDF is downloaded in **landscape format** (16:9 ratio):

   ```javascript
   let jspdf = document.createElement( "script" );
   jspdf.onload = function () {
     let pdf = new jsPDF('landscape', 'mm', [297, 210]); // A4 landscape size
     let elements = document.getElementsByTagName( "img" );
     
     for ( let i in elements) {
       let img = elements[i];
       if (!/^blob:/.test(img.src)) {
         continue;
       }
   
       let canvasElement = document.createElement( 'canvas' );
       let con = canvasElement.getContext( "2d" );
       canvasElement.width = img.width;
       canvasElement.height = img.height;
       con.drawImage(img, 0, 0,img.width, img.height);
       let imgData = canvasElement.toDataURL( "image/jpeg", 1.0);
       
       // Add the image to the PDF with correct position and size (A4 landscape size)
       pdf.addImage(imgData, 'JPEG', 0, 0, 297, 210); // 297x210mm A4 landscape size (16:9 ratio)
       pdf.addPage();
     }
     
     pdf.save( "download.pdf" );
   };
   
   jspdf.src = 'https://cdnjs.cloudflare.com/ajax/libs/jspdf/1.3.2/jspdf.min.js';
   document.body.appendChild(jspdf);
   ```

4. **Execute the Script**
   - Press `Enter` to execute the script. The images will be converted to PDF with a **16:9 landscape** format.
   - The PDF will be downloaded automatically.

---

### **Conclusion**

Now you can choose between two options for downloading PDFs from Google Drive:

- **Vertical (default) PDF** for standard document layouts.
- **Landscape (16:9) PDF** for presentations or slides that require a horizontal layout.

By using the JavaScript snippets provided in this tutorial, you can easily convert and download your Google Drive documents in the desired format. Let me know if you encounter any issues or need further clarification!

#### Reference:
- https://www.youtube.com/watch?v=BwO_dMqO7iI
- GPT