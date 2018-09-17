# **Finding Lane Lines on the Road** 

## Writeup Template

### You can use this file as a template for your writeup if you want to submit it as a markdown file. But feel free to use some other method and submit a pdf if you prefer.

---

**Finding Lane Lines on the Road**

The goals / steps of this project are the following:

- Make a pipeline that finds lane lines on the road

* Reflect on your work in a written report


[//]: # "Image References"

[image1]: ./pics/Step4.jpg "Step4"

[image2]: ./pics/Step5.jpg "Step5"

---

### Instructions to run the Notebook

#### 1. Run the Helper Functions (section 1.6)

#### 2. Run the process_image() function

#### 3. Run All Cells

---

### Reflection

### 1. Describe your pipeline:

### When developing my pipeline I used several cells and printed the respective output after a relevant operation on the image to see the intermediate results. This description here describes those steps as in the Notebook chapter "1.5 My Picture Pipeline". As soon as I had a working Algorithm to detect lane lines, I compressed the algorithm into the function process_image(), which I used on the test_images and the test_videos.

My pipeline consists of the following steps:

- **Step 1: Convert image to HSL color space (Hue, Saturation, Lightness)**

  - This makes the identification of lane lines under varying lighting conditions with manually set (and static) thresholds more robust, especially for the yellow lanes

- **Step 2: Threshold for white and yellow and mask**

  - I found the Hue range of the color yellow by looking at: https://www.w3schools.com/colors/colors_hsl.asp
  - Saturation and Lightness thresholds were set using trial and error
  - For white lanes only Lightness is thresholded

- **Step 3: Convert to grayscale and define ROIs**

  - I defined two regions of interest: One for the left lane line and one for the right

- **Step 4: Blur, Canny, Hough**
  - In this pipeline component, the Hough lines are detected after Gaussian blurring and edge detection with Canny edges
  - The below pictures show those three steps for the left and right ROI individually
  - A rather high value for the parameter 'max_line_gap' was chosen, which connects distinct blobs of edges as can be seen in the right column from 2nd to 3rd picture

  ![alt text][image1]

- **Step 5: Averaging and drawing the final results**

  - I created a function 'average_lines', which prioritizes longer line elements over shorter ones and calculates an averaged slope and the intersection with the bottom of the image for left and right lane respectively

  - This result is then drawn (green transparent) on the original image together with the raw Hough lines (red, hardly visible in this one). See the below picture:

![alt text][image2]



### 2. Identify potential shortcomings with your current pipeline

- I wouldn't bet that thresholding for yellow lines specifically is a sensible step. If the Hue of the line is not in the Hue range it might be missed. It was simply a first step to filter out the lanes
- Using two separate ROIs (for left and right lane marking) is not as computationally efficient as using only one, moreover it only works if the car is relatively centered in the lane
- In the Video the drawn green lines are jittering and in certain frames suddenly deviate from the previous orientation


### 3. Suggest possible improvements to your pipeline

A possible improvement would be to add some memory on how the lane markings in the frame before were oriented to add some damping/inertia to limit the jitter and sudden deviation of the detected lines.

Another potential improvement could be to find a more accurate way to connect the detected edges on the intermittent lane markings on the street inside.

Lines, which are at a more or less right angle to the actual line orientation, should be discarded more effectively to prevent them from influencing the averaged orientation.