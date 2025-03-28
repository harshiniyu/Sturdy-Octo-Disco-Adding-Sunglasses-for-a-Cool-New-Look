# Sturdy-Octo-Disco-Adding-Sunglasses-for-a-Cool-New-Look

Sturdy Octo Disco is a fun project that adds sunglasses to photos using image processing.

Welcome to Sturdy Octo Disco, a fun and creative project designed to overlay sunglasses on individual passport photos! This repository demonstrates how to use image processing techniques to create a playful transformation, making ordinary photos look extraordinary. Whether you're a beginner exploring computer vision or just looking for a quirky project to try, this is for you!

## Features:
- Detects the face in an image.
- Places a stylish sunglass overlay perfectly on the face.
- Works seamlessly with individual passport-size photos.
- Customizable for different sunglasses styles or photo types.

## Technologies Used:
- Python
- OpenCV for image processing
- Numpy for array manipulations

## How to Use:
1. Clone this repository.
2. Add your passport-sized photo to the `images` folder.
3. Run the script to see your "cool" transformation!

## Applications:
- Learning basic image processing techniques.
- Adding flair to your photos for fun.
- Practicing computer vision workflows.

Feel free to fork, contribute, or customize this project for your creative needs!
## program developed by
HARSHINI Y
212223240050
```
# Import libraries
import cv2
import numpy as np
import matplotlib.pyplot as plt

# Load the Face Image
faceImage = cv2.imread('Harshini.jpg')
plt.imshow(faceImage[:,:,::-1]);plt.title("Face")
```
![Screenshot 2025-03-28 110839](https://github.com/user-attachments/assets/753fdbef-f10d-43c9-930c-84c5dd6a2968)
```
img.shape
```
![Screenshot 2025-03-28 111043](https://github.com/user-attachments/assets/a2617178-ebcf-4381-a09a-fdb10dfade28)
```
# Load the Sunglass image with Alpha channel
# (http://pluspng.com/sunglass-png-1104.html)
glassPNG = cv2.imread('sunglass.png',-1)
plt.imshow(glassPNG[:,:,::-1]);plt.title("glassPNG")
```
![Screenshot 2025-03-28 111125](https://github.com/user-attachments/assets/5eeddc79-c462-48bd-86a2-a7f291c3b839)

```
# Resize the image to fit over the eye region
glassPNG = cv2.resize(glassPNG,(90,30))
print("image Dimension ={}".format(glassPNG.shape))
```
![Screenshot 2025-03-28 111207](https://github.com/user-attachments/assets/61687a0b-c900-4077-84e7-b168fa9c74c2)
```
# Separate the Color and alpha channels
glassBGR = glassPNG[:,:,0:3]
glassMask1 = glassPNG[:,:,3]
```
```
# Display the images for clarity
plt.figure(figsize=[15,15])
plt.subplot(121);plt.imshow(glassBGR[:,:,::-1]);plt.title('Sunglass Color channels');
plt.subplot(122);plt.imshow(glassMask1,cmap='gray');plt.title('Sunglass Alpha channel');
```
![Screenshot 2025-03-28 111816](https://github.com/user-attachments/assets/3502879a-ff26-449c-8167-62fd10af80e3)
```
# Make a copy
#faceWithGlassesNaive = resized_faceImage.copy()
faceWithGlassesNaive = faceImage.copy()

# Replace the eye region with the sunglass image
faceWithGlassesNaive[60:90,60:150]=glassBGR

plt.imshow(faceWithGlassesNaive[...,::-1])
```
![Screenshot 2025-03-28 111907](https://github.com/user-attachments/assets/fb6763f4-6951-48d8-a4cc-cf307a36a019)
```
# Make the dimensions of the mask same as the input image.
# Since Face Image is a 3-channel image, we create a 3 channel image for the mask
glassMask = cv2.merge((glassMask1,glassMask1,glassMask1))

# Make the values [0,1] since we are using arithmetic operations
glassMask = np.uint8(glassMask/255)

# Make a copy
faceWithGlassesArithmetic = faceImage.copy()

# Get the eye region from the face image
eyeROI= faceWithGlassesArithmetic[60:90,60:150]

# Use the mask to create the masked eye region
maskedEye = cv2.multiply(eyeROI,(1-  glassMask ))

# Use the mask to create the masked sunglass region
maskedGlass = cv2.multiply(glassBGR,glassMask)

# Combine the Sunglass in the Eye Region to get the augmented image
eyeRoiFinal = cv2.add(maskedEye, maskedGlass)

# Display the intermediate results
plt.figure(figsize=[20,20])
plt.subplot(131);plt.imshow(maskedEye[...,::-1]);plt.title("Masked Eye Region")
plt.subplot(132);plt.imshow(maskedGlass[...,::-1]);plt.title("Masked Sunglass Region")
plt.subplot(133);plt.imshow(eyeRoiFinal[...,::-1]);plt.title("Augmented Eye and Sunglass")
```
![Screenshot 2025-03-28 112132](https://github.com/user-attachments/assets/aa5b64fa-08b8-411a-9fae-2f063aed7a88)
```
# Replace the eye ROI with the output from the previous section
faceWithGlassesArithmetic[60:90,60:150]=eyeRoiFinal

# Display the final result
plt.figure(figsize=[20,20]);
plt.subplot(121);plt.imshow(faceImage[:,:,::-1]); plt.title("Original Image");
plt.subplot(122);plt.imshow(faceWithGlassesArithmetic[:,:,::-1]);plt.title("With Sunglasses");
```
![Screenshot 2025-03-28 112217](https://github.com/user-attachments/assets/baf2f6b0-641d-43ea-a3df-8085c6b5cd3d)
