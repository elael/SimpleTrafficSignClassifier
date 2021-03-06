# **Traffic Sign Recognition** 

## Writeup

### You can use this file as a template for your writeup if you want to submit it as a markdown file, but feel free to use some other method and submit a pdf if you prefer.

---

**Build a Traffic Sign Recognition Project**

The goals / steps of this project are the following:
* Load the data set (see below for links to the project data set)
* Explore, summarize and visualize the data set
* Design, train and test a model architecture
* Use the model to make predictions on new images
* Analyze the softmax probabilities of the new images
* Summarize the results with a written report


[//]: # (Image References)

[test_data]: ./results/test_data.png "Test Data"
[train_data]: ./results/train_data.png "Train Data"
[image2]: ./examples/grayscale.jpg "Grayscaling"
[image3]: ./examples/random_noise.jpg "Random Noise"
[priority]: ./scaled_webimages/priority.jpg "Priority"
[stop]: ./scaled_webimages/stop.jpg "Stop"
[trafficlight]: ./scaled_webimages/trafficlight.jpg "Trafficlight"
[not2]: ./scaled_webimages/not2.jpg "Not2"
[turnright]: ./scaled_webimages/turnright.jpg "Turnright"
[small30k]: ./scaled_webimages/small30k.jpg "Small30k"
[not]: ./scaled_webimages/not.jpg "Not"
[turnleft]: ./scaled_webimages/turnleft.jpg "Turnleft"
[small80k]: ./scaled_webimages/small80k.jpg "Small80k"
[small60k]: ./scaled_webimages/small60k.jpg "Small60k"
[small100k]: ./scaled_webimages/small100k.jpg "Small100k"

## Rubric Points
### Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used the pandas library to calculate summary statistics of the traffic
signs data set:

* The size of training set is 34799
* The size of the validation set is 4410
* The size of test set is 12630
* The shape of a traffic sign image is (32, 32, 3)
* The number of unique classes/labels in the data set is 43

#### 2. Include an exploratory visualization of the dataset.

Here is an exploratory visualization of the data set. It is a bar chart showing how the data is distributed, clearly not very well balanced.

![alt text][train_data]

![alt text][test_data]

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

As a first step, I decided to convert the images to LAB because, based on suggested articles, local histogram normalization on L-like channels helps enhancing accuracy.

I ended up not applying any histogram normalization, just basic nomalization.
I maped the range from 0 => 255 to -1 => 1 for every channel.

I decided to have a first try with the network befor applying any other pre-processing or data augmentation.


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					|
|:---------------------:|:---------------------------------------------:|
| Input         		| 32x32x3 normalized LAB image   				|
| Convolution 5x5     	| 1x1 stride, same padding, outputs 28x28x6 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 14x14x6  				|
| Convolution 5x5     	| 1x1 stride, same padding, outputs 10x10x16 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride,  outputs 5x5x16   				|
| Flatten				| outputs 400									|
| Fully connected		| outputs 120  									|
| RELU					|												|
| Dropout				|												|
| Fully connected		| outputs 84  									|
| RELU					|												|
| Dropout				|												|
| Fully connected		| outputs 10  									|


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used AdamOptimizer with initial lerning rate of 0.001. It trained through 100 epochs with batch size of 64.
Weights were randomly initialized from a normal distribution with 0 mean and 0.1 std dev. Bias were initialized to zero.

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 99.9%
* validation set accuracy of 94.3%
* test set accuracy of 93.5%

I started with a variant of **LeNet**, specially because it was fast to train and evaluate its perfomance on this new class of problems. LeNet was very successifully used to recognized digits and letters, which can also appear on traffic signs.
This end result shows that the model works well in general, although not outstanding, with was further verified with the web images. Better analysis could also include the accurracy on every class.

### Test a Model on New Images

#### 1. Choose eleven German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are the eleven German traffic signs that I found on the web:

![alt text][not2]
![alt text][trafficlight]
![alt text][small80k]
![alt text][priority]
![alt text][stop]
![alt text][turnright]
![alt text][small30k]
![alt text][not]
![alt text][turnleft]
![alt text][small60k]
![alt text][small100k]

The first image might be difficult to classify because there are some stikers on the image, also the second image can present some complication because of the reflection.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

All predictions were correct, except the first image, which was classified as "No vehicles" with high certanty.

The model was able to correctly guess 10 of the 11 traffic signs, which gives an accuracy of 91%. This compares similar to the accuracy on the test set of 93.5%.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 87th cell of the Ipython notebook.
Results for the top three predictions are:

| Image         		|     1st	     |   		2nd | 3rd	| 4th | 5th		| 
|:---------------------:|:--------------------:|:------------:|:-----------:|:-----------:|:-------------:| 
|![alt text][priority] | Yield 100.0% | Speed limit (20km/h) 0.0% | Speed limit (30km/h) 0.0% | Speed limit (50km/h) 0.0% | Speed limit (60km/h) 0.0%|
|![alt text][stop] | Stop 100.0% | Speed limit (30km/h) 0.0% | Speed limit (80km/h) 0.0% | Yield 0.0% | Bicycles crossing 0.0%|
|![alt text][trafficlight] | Traffic signals 100.0% | General caution 0.0% | Go straight or left 0.0% | No vehicles 0.0% | Turn right ahead 0.0%|
|![alt text][not2] | No vehicles 100.0% | Bumpy road 0.0% | Speed limit (50km/h) 0.0% | Speed limit (70km/h) 0.0% | No entry 0.0%|
|![alt text][turnright] | Go straight or right 100.0% | End of all speed and passing limits 0.0% | Ahead only 0.0% | Children crossing 0.0% | Roundabout mandatory 0.0%|
|![alt text][small30k] | Speed limit (30km/h) 100.0% | End of speed limit (80km/h) 0.0% | Speed limit (80km/h) 0.0% | Speed limit (70km/h) 0.0% | Speed limit (20km/h) 0.0%|
|![alt text][not] | No entry 100.0% | Speed limit (20km/h) 0.0% | Speed limit (30km/h) 0.0% | Speed limit (50km/h) 0.0% | Speed limit (60km/h) 0.0%|
|![alt text][turnleft] | Go straight or left 100.0% | Ahead only 0.0% | No passing for vehicles over 3.5 metric tons 0.0% | Priority road 0.0% | Speed limit (80km/h) 0.0%|
|![alt text][small80k] | Speed limit (80km/h) 99.5% | Speed limit (50km/h) 0.5% | Speed limit (30km/h) 0.0% | End of speed limit (80km/h) 0.0% | Keep right 0.0%|
|![alt text][small60k] | Speed limit (60km/h) 100.0% | Children crossing 0.0% | Speed limit (80km/h) 0.0% | Turn left ahead 0.0% | Ahead only 0.0%|
|![alt text][small100k] | Speed limit (100km/h) 99.9% | Speed limit (80km/h) 0.1% | Speed limit (30km/h) 0.0% | Speed limit (120km/h) 0.0% | End of speed limit (80km/h) 0.0%|

### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?

For now, I decided to learn more tools (like Keras) before diving deeper into training and analysing more networks.


