# **Traffic Sign Recognition** 

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

[image1]: ./writeup_images/randomviz.png "Visualization"
[image2]: ./writeup_images/trainingClassDist.png "Training Class Distribution"
[image3]: ./writeup_images/validationClassDist.png "Validation Class Distribution"
[image4]: ./writeup_images/testClassDist.png "Test Class Distribution"
[image5]: ./writeup_images/architecture.JPG "Model Architecture"
[image6]: ./test_images/original/test1.jpg "Traffic Sign 1"
[image7]: ./test_images/original/test2.jpg "Traffic Sign 2"
[image8]: ./test_images/original/test3.jpg "Traffic Sign 3"
[image9]: ./test_images/original/test4.jpg "Traffic Sign 4"
[image10]: ./test_images/original/test5.jpg "Traffic Sign 5"
[image11]: ./writeup_images/testDiscussion.JPG "Test Images Prediction"
[image12]: ./writeup_images/test1Disc.JPG "Test Image 1 Probabilities"
[image13]: ./writeup_images/test2Disc.JPG "Test Image 2 Probabilities"
[image14]: ./writeup_images/test3Disc.JPG "Test Image 3 Probabilities"
[image15]: ./writeup_images/test4Disc.JPG "Test Image 4 Probabilities"
[image16]: ./writeup_images/test5Disc.JPG "Test Image 5 Probabilities"
[image17]: ./writeup_images/origImage.png "Original Image"
[image18]: ./writeup_images/transImage.png "Transformed Image"
[image19]: ./writeup_images/trainingProcess.png "Training Process"

## Rubric Points
###Here I will consider the [rubric points](https://review.udacity.com/#!/rubrics/481/view) individually and describe how I addressed each point in my implementation.  

---
### Writeup / README

#### 1. Provide a Writeup / README that includes all the rubric points and how you addressed each one. You can submit your writeup as markdown or pdf. You can use this template as a guide for writing the report. The submission includes the project code.

You're reading it :)! [Here](https://github.com/sseshadr/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier.ipynb) is the code. Note that the numbers may differ since the runs don't match exactly. But it should always pass the requirements.

### Data Set Summary & Exploration

#### 1. Provide a basic summary of the data set. In the code, the analysis should be done using python, numpy and/or pandas methods rather than hardcoding results manually.

I used python to calculate summary statistics of the traffic signs data set:


Number of training examples = 34799

Number of validation examples = 4410

Number of testing examples = 12630

Image data shape = (32, 32, 3)

Image data type = <class 'numpy.float64'>

Number of classes = 43

#### 2. Include an exploratory visualization of the dataset.

I did this a couple of ways. First was to randomly visualize an image from the training set as shown below:

![alt text][image1]

Next, I plotted a histogram of all classes in each of the training, validation and test sets to see if the distributions are more or less the same. I read in [this article](https://chatbotslife.com/german-sign-classification-using-deep-learning-neural-networks-98-8-solution-d05656bf51ad) that the relative ratio of the classes actually helps the network learn the probability of the sign occurring. So, all classes don’t require the same number of samples but all sets should probably require the same ratio of classes.

![alt text][image2]

![alt text][image3]

![alt text][image4]


### Design and Test a Model Architecture

#### 1. Describe how you preprocessed the image data. What techniques were chosen and why did you choose these techniques? Consider including images showing the output of each preprocessing technique. Pre-processing refers to techniques such as converting to grayscale, normalization, etc. (OPTIONAL: As described in the "Stand Out Suggestions" part of the rubric, if you generated additional data for training, describe why you decided to generate additional data, how you generated the data, and provide example images of the additional data. Then describe the characteristics of the augmented training set like number of images in the set, number of images for each class, etc.)

1.	I decided to work with the entire RGB data since the LeNet example used grayscale. So there is no preprocessing there because the dataset provided was in color. 
2.	I shuffled the training data set so that we don’t have any biases within the mini batches during training.
3.	Finally, I normalized the image with zero mean and equal variance so that the network doesn’t care that much about the intensity or illumination in the pixels.

I decided to generate additional data because the network did not perform to our requirements on test images from the internet. So adding more data was my first step to get a ‘better’ network.

To add more data to the data set, I randomly transformed the image using translation, rotation and shear factors. Additionally I also controlled the brightness by a random factor. For starters, I did this once for the already existing training simple thereby doubling the number of our training examples. [Here](https://medium.com/@vivek.yadav/dealing-with-unbalanced-data-generating-additional-data-by-jittering-the-original-image-7497fe2119c3) is my reference and [here](https://github.com/vxy10/ImageAugmentation) is the code for this approach.
Although I implemented this in [Traffic_Sign_Classifier-Augmented.ipynb](https://github.com/sseshadr/CarND-Traffic-Sign-Classifier-Project/blob/master/Traffic_Sign_Classifier-Augmented.ipynb), I ran into some size issues during training. Since this is an optional part of the project, I focused first on finishing the rest of the rubric. I will probably come back to this after I pass my project.

Here is an example of an original image and an augmented image:

![alt text][image17]

![alt text][image18] 


#### 2. Describe what your final model architecture looks like including model type, layers, layer sizes, connectivity, etc.) Consider including a diagram and/or table describing the final model.

My final model based on the LeNet architecture consisted of the following layers:

![alt text][image5]


#### 3. Describe how you trained your model. The discussion can include the type of optimizer, the batch size, number of epochs and any hyperparameters such as learning rate.

To train the model, I used the Adam optimizer that was provided in the LeNet lab. The following were my major hyper parameters:

Batch size = 128

Number of Epochs = 20

Learning Rate = 0.001


I also used a dropout layer with a keep probability value set to 0.5 during training and 1.0 during validation and test.

#### 4. Describe the approach taken for finding a solution and getting the validation set accuracy to be at least 0.93. Include in the discussion the results on the training, validation and test sets and where in the code these were calculated. Your approach may have been an iterative process, in which case, outline the steps you took to get to the final solution and why you chose those steps. Perhaps your solution involved an already well known implementation or architecture. In this case, discuss why you think the architecture is suitable for the current problem.

My final model results were:
* training set accuracy of 0.998
* validation set accuracy of 0.934
* test set accuracy of 0.931

* What was the first architecture that was tried and why was it chosen?
The LeNet architecture was suggested as a good starting point so that is what I chose. Since this model has already proved to be good at deciphering numbers and we know that traffic signs might contain numbers too, it is a good starting point. From there, we need to do empirical tests to see if it can recognize non-numeric signs just as well. [This reference paper](http://yann.lecun.com/exdb/publis/pdf/sermanet-ijcnn-11.pdf) was also useful.
* What were some problems with the initial architecture?
A good way to check if an architecture will work at all for the application is to train it so much that it actually overfits the training data. Once we get there, we can apply some deep learning techniques such as dropouts and regularizations to generalize the network and address the overfitting. So this is a problem we are intentionally working towards. With the initial architecture, we achieved overfitting. Training accuracy was ~0.95 and the validation accuracy was ~0.85. Here is when I decided to raise the epochs to see if the trends continue. I also added a couple of visualizations to plot the loss and the accuracy curve to see the trend better. We would see training always reach a really good accuracy. But validation loss will be moving around a whole lot instead of settling down.
![alt text][image19]
* How was the architecture adjusted and why was it adjusted? 
To address the overfitting, I added a drop out layer. This makes the network question the input data and not be over reliant on a particular input to be available to make a decision. We can set this probability of an input being available. A good choice is to use 0.5 during the training process. But in validation and test we set this to 1.0 since dropout is supposed to do its blind folding act only during training. During normal operation, we can think of it as a see through layer.
* Which parameters were tuned? How were they adjusted and why?
To be honest, the starting parameters themselves worked well. I only increased the epochs so that I could be sure by looking at the curves that steady state was achieved. One of the things I would have considered would be to add L2 regularization to address the overfitting if it still existed. But at this point, it seemed an overkill.
* How does the final model's accuracy on the training, validation and test set provide evidence that the model is working well?
The final model’s accuracy on training, validation and test set exceeds the requirement 0f 0.93 accuracy on the validation set.

### Test a Model on New Images

#### 1. Choose five German traffic signs found on the web and provide them in the report. For each image, discuss what quality or qualities might be difficult to classify.

Here are five German traffic signs that I found on the web:

![alt text][image6] ![alt text][image7] ![alt text][image8] 
![alt text][image9] ![alt text][image10]

All the images are from the internet. Some were cropped so that the sign is centrally placed in the image. The watermarks were left in there as noise to see how robust our classifier is.

#### 2. Discuss the model's predictions on these new traffic signs and compare the results to predicting on the test set. At a minimum, discuss what the predictions were, the accuracy on these new predictions, and compare the accuracy to the accuracy on the test set (OPTIONAL: Discuss the results in more detail as described in the "Stand Out Suggestions" part of the rubric).

Here are the results of the prediction:

![alt text][image11]


The model was able to correctly guess 3 of the 5 traffic signs, which gives an accuracy of 75%. On the provided test set itself, the accuracy was 93%.

#### 3. Describe how certain the model is when predicting on each of the five new images by looking at the softmax probabilities for each prediction. Provide the top 5 softmax probabilities for each image along with the sign type of each probability. (OPTIONAL: as described in the "Stand Out Suggestions" part of the rubric, visualizations can also be provided such as bar charts)

The code for making predictions on my final model is located in the final cell of the Ipython notebook Traffic_Sign_Classifier.ipynb.

For the first image, the model is relatively sure that this is a roundabout mandatory sign and the image does contain this sign. Other max probabilities were also related to arrow heads which means the network is doing a good job of identifying arrows.


![alt text][image12]


For the second image, we had a wild animals crossing sign. The model is relatively sure about this sign as well. Other probabilities consisted of ‘curves’.


![alt text][image13]


For the third image, we had a bicycle crossing sign. None of the top 5 probabilities show this. It will be interesting to see if the ‘blue’ nature of the sign has any effect on the different layers. 


![alt text][image14]


For the fourth image, we had a slippery road sign. The model is relatively sure about this sign. It is interesting to see that wild animals sign has the second highest probability. If we go back to the results for the second image, we see slippery road as the third highest. Something about these two images, the network seems to find similar features.


![alt text][image15]


For the fifth image, we had a beware of ice/snow sign. The model thinks there is only a second highest probability that this is that sign. 


![alt text][image16]

