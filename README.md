# __Real time traffic sign detection and classification with voice feedback__
[![GitHub version](https://badge.fury.io/gh/Naereen%2FStrapDown.js.svg)](https://github.com/Naereen/StrapDown.js)

With the increase in number of road accidents happening every year, there has been a need to develop a system that contributes to the safety of the drivers, pedestrians and vehicles. Traffic sign detection and recognition plays an integral role for driver assistant system as well as autonomous driving vehicles.

In this project, an approach to assist the driver through traffic sign recognition with much more faster detection in conjunction with human-like general voice feedback has been presented.

### Output video

[![Output Video](https://img.youtube.com/vi/A20vlHzG-ek/0.jpg)](https://www.youtube.com/watch?v=A20vlHzG-ek&feature=youtu.be "Output Video")

#### Technologies used: Python, Jupyter Notebook.
<img src="https://img.shields.io/badge/python%20-%2314354C.svg?&style=for-the-badge&logo=python&logoColor=white"/> <img src="https://img.shields.io/badge/Jupyter%20-%23F37626.svg?&style=for-the-badge&logo=Jupyter&logoColor=white" /> 

## _Table of Contents_
+ [Dataset](#dataset)
+ [Installation](#installation)
+ [Working](#working)
+ [Results](#results)
<br>

## Dataset

Our dataset mainly focused on German Traffic signs and videos related to same are collected. As the video collected was raw, some processing had to be performed in order to obtain clean dataset.

#### Classes of our dataset
![metadata](/Images/meta.png)

During the pre-processing, the part of the video that does not contain traffic sign is trimmed as it may add to the noisy data. After the entire video is trimmed, the frames are obtained at the rate 10 frames per second and each frame is saved as a jpeg file. Classes names are the found in obj.names file in the repository.

#### Dataset Distribution according to classes
![datastat](Images/Data%20Distribution.png)

## Installation

+ Initially set up darknet by [AlexeyAB](https://github.com/AlexeyAB/darknet) ([darknetSetup.ipynb](/darknetSetup.ipynb))
+ Calculate Anchor box values ([ancharCalculations.ipynb](/ancharCalculations.ipynb))
+ Model Training ([modelTraining.ipynb](/modelTraining.ipynb))
+ Weights Changing & Testing (Optional if you require Keras Implemenation [weightsChangingAndDataTesting.ipynb](/weightsChangingAndDataTesting.ipynb))
+ After training we get [Weights file](/yolov3-tiny-obj_last.weights) so these weights can be used on our local machine program 

```python
pip install opencv-python
pip install imutils
pip install subprocess
pip install pydub
pip install gTTS
```

For implementing real time audio we used [FFmpeg](https://ffmpeg.org/) software.

## Working

#### Flow of our system
![flow](/Images/Flow.png)

------
### 1. Bounding box mechanism
To accomplish this we used a tool [OpenLabeling](https://github.com/Cartucho/OpenLabeling) written in Python to draw bounding box for each traffic sign. YOLOv2 requires annotation text in XML file format while YOLOv3 requires the same in TXT file format. So, this tool generates a txt file for every image. The format of storing the annotation data in the txt file is as follows: <br>
`[class_id] [x] [y] [width] [height]`

Where:
 - `[class_id]` : integer number of object class from 0 to (classes-1)
 - `[x] [y] [width] [height]` : float values relative to width and height of image, it can be equal from (0.0 to 1.0]
 - for example: `[x] = [absolute_x] / [image_width]` or `[height] = [absolute_height] / [image_height]`
 - Note `[x] [y]`  is the centre of rectangle (are not top-left corner).

------
### 2. YoloV3
YOLOv3 makes prediction on the basis of darknet-53 at 3 different scales. Each location is being predicted 3 times by YOLOv3. Each prediction takes into account a boundary box, an object score and 44 class scores, i.e. N × N × [3 × (4 + 1 + 44)] predictions. Each block displays the following things that is the type of layer, the stride, number of filters and filter size. This means, with an input of 416 x 416, we make detections on scales 13 x 13, 26 x 26 and 52 x 52. Each cell predicts 3 bounding boxes using 3 anchors at each scale which makes the total number of anchors used as 9 (The anchors are different for different scales).

An image may contain many objects and each object is related with one grid cell. YOLO can work well in such situations where overlapping of centre points of two objects can occur. To allow a grid cell to detect multiple objects, YOLO uses anchor boxes. With the help of anchor boxes, a longer grid cell vector is created and multiple classes with each grid cell can be associated. Anchor boxes have a defined aspect ratio with which they try to detect objects that properly fit into a box with the defined ratio.

![table](Images/Architecture%20Yolov3.png)

------
### 3. Training
For our proposed model, we have collected upto 8200 images with each image labelled and having its annotation file associated with it such that the name of the txt file is same as that of the image. We trained our model on [Google Colab](https://colab.research.google.com/) as it provides a single 12GB NVIDIA Tesla K80 GPU for a runtime of about 12 hours. The total number of epochs were 15000 i.e. 15000 iterations with an average loss of 0.5 and a learning rate of 0.001. After every thousand iterations, weights were saved as weights file.

------
### 4. Voice Feedback
For including voice assistance in our system, we have used Google Text-to-Speech ([gTTS](https://pypi.org/project/gTTS/)) python library which generates sound based on the text. However, while speaking out the label of the detected traffic sign in current frame, the processing of the next frame used to halt. Hence, there was a delay with respect to the actual video. To overcome this problem, we used the concept of Multithreading in which the processing of the frame continues and the voice feedback does not interrupt the processing of the next frame. However, even after handling the issue of delay, another obstacle arised i.e. as the frames were continuous, in each frame the traffic sign detected was called out by the gTTS. Hence, we used a buffer array to check the repetition of consecutive traffic sign in the frames. If the sign detected is not in the buffer then the gTTS will speak the label of the class else it will look for the next frame. Solving these issues helped in increasing the performance of the model on our system.

------
### 5. Results
![Output image](output/output1.png)

![Output image](output/output2.png)
