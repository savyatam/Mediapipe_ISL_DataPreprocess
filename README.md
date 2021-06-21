# Modify data with Mediapipe 
#### Multi hand tracking and Face mesh

This repo contains code taken from [rabBit64's project](https://github.com/rabBit64/Sign-language-recognition-with-RNN-and-Mediapipe)

### Data Preprocessing with Mediapipe (Desktop on CPU)
Create training data on Desktop with input video using [Multi Hand Tracking](https://github.com/google/mediapipe/blob/master/mediapipe/docs/multi_hand_tracking_mobile_gpu.md).
Gesture recognition with deep learning model can be done with **hand landmark and face landmark features** per frame with RNN training .

**CUSTOMIZE:**
- Use video input instead of Webcam on Desktop to train with video data
- Preprocess hand landmarks for every frame per one word and make it into one txt file
- Similarly reprocess face landmarks for every frame per one word and make it into one txt file. 
- Now append each hand landmarks txt file to corresponding face landmarks txt file .

### New : Use [Holistic Tracking](https://google.github.io/mediapipe/solutions/holistic.html) instead 


### 1. Set up Hand Tracking framework
* Install Medapipe
```shell
  git clone https://github.com/google/mediapipe.git
```
See the rest of installation documents [here](https://google.github.io/mediapipe/getting_started/install.html).
* Change **end_loop_calculator.h** file
```shell
  cd ~/mediapipe/mediapipe/calculators/core
  rm end_loop_calculator.h
```
to our new /end_loop_calculator.h file in the modified_mediapipe folder.

* Change **demo_run_graph_main.cc** file 
```shell
  cd ~/mediapipe/mediapipe/examples/desktop
  rm demo_run_graph_main.cc
```
to our new demo_run_graph_main.cc file in the modified_mediapipe folder.

* Change **landmarks_to_render_data_calculator.cc** file
```shell
  cd ~/mediapipe/mediapipe/calculators/util
  rm landmarks_to_render_data_calculator.cc
```
to our new landmarks_to_render_data_calculator.cc file in the modified_mediapipe folder.

### 2. Create your own training data
Make **train_videos** for each sign language word in one folder. Use **build_multihands.py** and **build_facemesh.py** files to your mediapipe directory.
* Usage

To make mp4 file and txt file with mediapipe automatically, run inside mediapipe directory.
```shell
  python build_file.py --input_data_path=[INPUT_PATH] --output_data_path=[OUTPUT_PATH]
```
*Example
```shell
  python build_multihands --input_data_path=Input/ --output_data_path=OutputHands/
```



IMPORTANT: Name the folder carefully as the folder name will be the label itself for the video data.
(DO NOT add space bar or '_' to your folder name eg: Gesture_1 (X))

For example:
```shell
Input
├── Gesture1
│   ├── Gesture1_1.MOV
│   ├── Gesture1_2.MOV
│   ├── Gesture1_3.MOV
│   └── Gesture1_4.MOV
└── Gesture2
    ├── Gesture2_1.MOV
    ├── Gesture2_2.MOV
    ├── Gesture2_3.MOV
    └── Gesture2_4.MOV
    ...
```
The output path is initially an empty directory, and when the build is complete, Mp4 and txt folders are extracted to your folder path.

Created folder example:
```shell
OutputHands
├── Absolute
│   └── Gesture1
│       ├── Gesture1_1.txt
│       ├── Gesture1_2.txt
│       ├── Gesture1_3.txt
│       └── Gesture1_4.txt
|       ...
├── Relative
│   └── Gesture1
│       ├── Gesture1_1.txt
│       ├── Gesture1_2.txt
│       ├── Gesture1_3.txt
│       └── Gesture1_4.txt
│       ...
└── _Gesture1
     ├── Gesture1_1.mp4 
     ├── Gesture1_2.mp4
     ├── Gesture1_3.mp4
     └── Gesture1_4.mp4
```

You can find Indian Sign Language dataset [here](https://zenodo.org/record/4010759#.YNBrERHhVuR).


IMPORTANT: 

This code would crash (error: segmentation fault) if you apply for any other solutions with greater landmarks. 

```shell
cd ~/mediapipe/mediapipe/examples/desktop/demo_run_graph_main.cc

In line 35: vector<pair<float,float>> posl(468);

```
In such cases make sure the size assigned to **posl** variable is greater than the maximum possible landmarks.


### 3. Use either relative or absolute data to train your RNN model.



