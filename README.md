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
