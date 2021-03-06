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

[image1]: ./data-visualization/1.png "Visualization"
[image2]: ./data-visualization/2.png "Grayscaling"
[image3]: ./data-visualization/3.png "Random Noise"
[image4]: ./examples/placeholder.png "Traffic Sign 1"
[image5]: ./examples/placeholder.png "Traffic Sign 2"
[image6]: ./examples/placeholder.png "Traffic Sign 3"
[image7]: ./examples/placeholder.png "Traffic Sign 4"
[image8]: ./examples/placeholder.png "Traffic Sign 5"

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

![alt text][image1]
![alt text][image2]
![alt text][image3]

The exploratory visualiation of the dataset can be viewed in the notebook Traffic_Sign_Classifier.ipynb.

### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

I used the cv2 normalize function and the sklearn shuffle function as the primary techniques to preprocess the image data. Normalization allowed us to convert the image data to one of mean 0 and equal variance while retaining the content information. Shuffling ensured there was no relation between adjacent image samples and therefore made our model more robust.


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model consisted of the following layers:

| Layer         		|     Description	        					| 
|:---------------------:|:---------------------------------------------:| 
| Input         		| 32x32x3 RGB image   							| 
| Convolution         	| 1x1 stride, valid padding, outputs 28x28x16 	|
| RELU					|												|
| Max pooling	      	| 2x2 stride, valid padding, outputs 14x14x16 	|
| Convolution   	    | 1x1 stride, valid padding, outputs 10x10x64   |
| RELU           		| etc.        									|
| Max pooling			| 2x2 stride, valid padding, outputs 5x5x64     |
| Fully connected	    |                            					|
| RELU					|												|
| Dropout               | Dropout with keep probability of 0.5          |
| Fully connected       |                                               |
| RELU                  |                                               |
| Dropout               | Dropout with keep probability of 0.5          |
| Fully connected       |                                               |
| Softmax               | Minimize loss cross softmax cross entropy     |



#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

I configured the hyperparamters as follows. I set the batch size as 125 as this seem sufficient to provide a very accurate model with minimial memory usage. I set the epochs to 30, as the performance accuracy seemed to peak around this threshold. The other paramters, such as learning rate, were kept at the default values. I used the Adam Optimizer rather than Gradient Descent as this yielded much better performance. 

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 1.00
* validation set accuracy of 0.9619 
* test set accuracy of 0.9537

I chose the recommended LeNet architecture as it is the standard and performs well for classificaion of 32x32 pixels. I initially experimented with grayscaling the images, but I noticed the model has greater accuracy when I did not preprocess into grayscale. This is a perhaps unconventional feature of the training model. I figured for this classification problem, color may be an important feature in the model learning the traffic signs.

I also began training the model with the gradient descent optimizer. However, this failed to improve the accuracy beyond 70% even when increasing parameters such as dropouts and network depth. After switching to Adam Optimizer, the results were in the low to mid 90%. 

To achieve an accuracy in the high 90%, I began experimenting with the hyperparameters. I first tried regularization and added a dropout of 0.2 and noticed an improvement so I kept increasing it until 0.5, which seems to have the maximum optimal effect. Finally, I began including epoch size, beginning with 10 and noticing good results so I improved to 30. With these set of parameters, I achieved a validation set accuracy of 0.9619.

The training set accuracy is higher than that of the validation set. There may be some overfitting. Some possible solutions to solve the overfitting problems may be: use data augmentation, increasing the data set, and add more L1/L2 regularization to make the model more robust.
 

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

The images of the five German traffic signs I chose can be found in the notebook. 

For the speed sign, the angle from the left bottom may make it difficult to classify. There is also a shandow on the upper right and there is green tress in the background that may skew the model. The image has good brightness. 

The roundabout was also tilted at a upward angle. It has good brightness and is largely clear from any background noise besides a blue sky. The content is clear and there is no overt shadowing. 

The pedestrian and priority signs were at a straighforward angle with adequate brightness, but the actual content of the sign was a little difficult to discern due to the small image sizes. Moreove, the pedestiran image has some overshadow from the blue background.   

Finally, the ahead sign was straightforward and clear, so it did not present any issues. 

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

| Image			        |     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| Ahead          		| Ahead      									| 
| Pedestrian     	    | Pedestrian									|
| Priority				| Priority										|
| Roundabout	      	| Roundabout					 				|
| Speed          		| General Caution      							|


The model was able to correctly guess 4 of the 5 traffic signs, for an accuracy of 80%. This compares unfavorably to the accuracy on the test set of 95%. However, it seems with a larger set of traffic images, these two percentages would converge. Also as noted, the speed sign was at a very large upward tilt/angle, which skewed the model's prediction.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the 11th cell of the Ipython notebook.

For the ahead traffic sign:

| Probability         	|     Prediction	        					| 
|:---------------------:|:---------------------------------------------:| 
| 1.00         			| Ahead only   									| 
| 0.00     				| Go straight or right 							|
| 0.00					| Turn left ahead								|
| 0.00	      			| Roundabout mandatory			 				|
| 0.00				    | End of no passing    							|

For the pedestrian sign:

| Probability           |     Prediction                                | 
|:---------------------:|:---------------------------------------------:| 
| 1.00                   | Pedestrians                                   | 
| 0.00                   | General Caution                               |
| 0.00                   | Road Narrows on Right                         |
| 0.00                   | Speed limit 70                                |
| 0.00                   | Speed limit 20                                |

For the priority sign:

| Probability           |     Prediction                                | 
|:---------------------:|:---------------------------------------------:| 
| .60                   | Priority road                                 | 
| .20                   | Speed limit 20                                |
| .05                   | Speed limit 30                                |
| .04                   | Speed limit 50                                |
| .01                   | Speed limit 60                                |

For the roundabout sign:

| Probability           |     Prediction                                | 
|:---------------------:|:---------------------------------------------:| 
| 1.00                  | Roundabout mandatory                          | 
| 0.00                  | Keep left                                     |
| 0.00                  | Keep Right                                    |
| 0.00                  | Turn Right Ahead                              |
| 0.00                  | Speed limit 200                               |

For the ahead speed sign:

| Probability           |     Prediction                                | 
|:---------------------:|:---------------------------------------------:| 
| .92                  | General caution                               | 
| .02                  | Speed limit 20                                |
| .02                  | Bicycles Crossing                             |
| .006                 | Bumpy Road                                    |
| .003                 | Children Crossing                             |


For the second image ... 

### (Optional) Visualizing the Neural Network (See Step 4 of the Ipython notebook for more details)
#### 1. Discuss the visual output of your trained network's feature maps. What characteristics did the neural network use to make classifications?


