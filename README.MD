# TrackNPred: A Software Framework for End-to-End Trajectory Prediction

This is the code base for our paper (*Under Review*). Please consider citing our work if you found this useful:

```
Example Bibtex (to be replaced):

@misc{ Nobody06,
       author = "Nobody Jr",
       title = "My Article",
       year = "2006" }
```

## A Visualized Approach to Deep Learning


<img src="img/demo_x250.gif">

## Overview

We present RobustTP, an end-to-end algorithm for predicting future trajectories of road-agents in dense traffic with noisy sensor input trajectories obtained from RGB cameras (either static or mov-ing) through a tracking algorithm. In this case, we consider noise as the deviation from the ground truth trajectory. The amount of noise depends on the accuracy of the tracking algorithm. Our approach is designed for dense heterogeneous traffic, where the road agents corresponding to a mixture of buses, cars, scooters, bicycles, or pedestrians. RobustTP is an approach that first computes trajectories using a combination of a non-linear motion model and a deep learning-based instance segmentation algorithm. Next,these noisy trajectories are trained using an LSTM-CNN neural network architecture that models the interactions between road-agents in dense and heterogeneous traffic. We also release a software framework, TrackNPred. The framework consists of implementations of state-of-the-art tracking and trajectory predictionmethods and tools to benchmark and evaluate them on real-world dense traffic datasets

<img src="img/pipeline.png">

TrackNPred currently supports the following:

1. **Tracking by Detection:**
<ul>
<li> <a href="https://github.com/pjreddie/darknet/wiki/YOLO:-Real-Time-Object-Detection"> YOLO </a>+ DeepSORT  </li>
<li> <a href="https://github.com/matterport/Mask_RCNN"> Mask R-CNN </a>+ DeepSORT</li>
</ul>

<img src="img/trackedTRAFCSCS19.png">

3. **Trajectory Prediction**
<ul>
<li> <a href="https://arxiv.org/abs/1803.10892"> Social-GAN </a> </li>
<li> <a href="https://arxiv.org/abs/1805.06771" >Convolutional Social LSTM </a> </li>
<li> <a href="http://openaccess.thecvf.com/content_CVPR_2019/papers/Chandra_TraPHic_Trajectory_Prediction_in_Dense_and_Heterogeneous_Traffic_Using_Weighted_CVPR_2019_paper.pdf"> TraPHic </a></li>

## Dependencies
Support Python 3.5 and above. For a list of dependencies see requirements.txt

### Installing Dependencies:
1. With pip or pip3 (Preferred Python 3): <br>
`pip3 install -r requirements.txt`
2. Creating a conda environement<br>
`conda create --name <env name e.g. tracknpred> --file requirements.txt`

## Dataset
* Currently we only accept mp4 file. 
* We need to create a folder that has the same name as the video file and put the video in that folder. 
* The datafolder needs to exist in the Direcotry specified in the GUI/commandline.<br>
	Example: if the data directory is `resources/data/TRAF`, and the video name is `DEMO.mp4`, then the location of the video should be `resources/data/TRAF/DEMO/DEMO.mp4`.
* All of the files generated will be in the corresponding datafolder.

## Usage
1. Run `python setup.py` to get resources folder containing dataset and model weights
2. Install Dependencies. See [Dependencies](##Dependencies)
3. Prepare the dataset. See [Dataset](##Dataset) section for more details.
4. We provide the options of GUI and command line control:<br>
	Examples:
	* Run `python main.py` to get the GUI, which will give you the options you can choose.
	* Run `python main_cmd.py -l 11 12` to run dataset TRAF11 and TRAF12 in command line.
	* Run `python main_cmd.py -l 11 -detection False -tracking False -train True -eval True` to only run prediction training and evaluating on dataset TRAF11 in command line.
		* `-l` is required. It takes in a series of numbers and specifies which dataset you want to run. 
		* Other flags are not required, but you can play around the options that was provided in the GUI. Please see how they are defined in the [main_cmd.py](main_cmd.py) file.


## Results
Our trajectory prediction algorithm outperforms state-of-the-art methods for end-to-end trajectory prediction using sensor inputs. We achieve an improvementof upto 18% in average displacement error and an improvement of up to 35.5% in final displacement error at the end of the prediction window (5 seconds) over the next best method. All experiments were set up on an Nvidia TiTan Xp GPU.

We evaluate RobustTP with methods that use manually annotated trajectory histories, on the TRAF Dataset. The results are reported in the following format: ADE/FDE, where ADE is the average displacement RMSE over the 5 seconds of prediction and FDE is the final displacement RMSE at the end of 5 seconds. We observe that RobustTP is at par with the stat-of-the-art:
	
| RNN-ED    | S-GAN  |CS-LSTM | TraPHic|RobustTP|
| ---        |    ----   |  ----   |----   |----   |
|3.24/5.16 | 2.76/4.79 | 1.15/3.35 | **0.78/2.44** | 1.75/3.42|

We also evaluate RobustTP using trajectory histories that are computed by two detection methods: Mask R-CNN and YOLO:

Using Mask R-CNN            |  Using YOLO
:-------------------------:|:-------------------------:
![](img/rmse.png)  |  ![](img/rmse_yolo.png)