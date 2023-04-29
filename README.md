# uno-card-detection

This code is an implementation of a computer vision algorithm for detecting Uno cards in a video stream and on individual images using Python and OpenCV. 

The algorithm first extracts features and color information from each frame of the video stream, and then uses a pre-trained Random Forest Classifier to predict the type of the card. 

The predicted card type is displayed on the video stream in real-time using OpenCV. 

The code allows for the detection of specific types of Uno cards: "Number Cards", "Pick Two," "Reverse," and "Skip" 

The code can be easily adapted to detect other types of cards or objects by training a new classifier on a diverse and large dataset.

Below are the blocks of code that I used to build this project.

### Color Extraction
For extracting the color of the card we first select the region of interest and crop the rest of the image. 

After that I have used pre-defined HSV ranges for blue, red, green and yellow color to create a bitwise and mask to select specific colors in the card. 
After applying the mask I check for the maximum pixels of the color that remained after applying the mask and then classify the color of the card. 

### Extracting Features and creating a Dataset of features

In this code I get the contours of the card and select a particular contour that I am interested in i.e the contour of the number or the type of the card inside the ellipse. 

I select the contour by using the heirarchy retrived by the RETR_TREE method in OpenCV. The images I have used in this project only contain the uno card on a black background. If there is any noise in the images the features extractin will fail as it'll be difficult to reach to the contour of the number of the card inside the ellipse. 

After selecting all the child contours of the ellipse I merge them into a single conotur and apply feature extraction operations on it. 

The feature that I extract from the merged contour are: Aspect Ratio, Extent, Solidity, Equivalent Diameter, Hu Moments and the number of holes.

After extracting the contours I loop through all the images in the dataset, extract the features from each image and save the features with the card type as the classes using joblib. 

The dataset is similar to the iris dataset in structure i.e it contains a dictionary of arrays of features and classes. 

## Training Random Forest Classifier on the created Dataset

I have used Random Forest Classifier for this problem because it performs really well on the problems with multiple classes. 

## Testing 
### On the image files 
You can replace the folder uno_images_test with your test images. 
Note: This code is only limited to detecting images on a dark background and with no noise. 

### On the stream
Similar conditions are required when testing on the camera stream as well. 
