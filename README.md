# Action Chunking with Transformer for Yahboom Dofbot
This repository contains a re-adaptation of [Action Chunking Transformer](https://github.com/tonyzhaozh/act/tree/main) that works for this [Yahboom Dofbot Jetson Nano](https://category.yahboom.net/products/dofbot-jetson_nano?variant=33009975361620). 

FYI: The repo isn't super polished and you may run into some small errors along the way. If you do, please reach out and I will do my best to update it.

Instead of using Teleoperation:
- It utilizes the Dofbots pre-packaged SW and traditional controls to record the robot completing the task
- The data from using the traditional controls is used to train the ACT policy with the below modification
  - The _action_ data is simulated by interpolating between the current joint positions and the next joint positions after collection

https://github.com/bmiesch/act_dofbot/assets/96878387/80e3dff6-f465-4216-9ad5-688f12d8125b

## Setup
In PyTorch 1.10.0, please see the instructions below.
Install the requirements (Most of the dependencies are already installed):
~~~
pip3 install requirements.txt
~~~

## Getting Started

### Clone the repository
~~~
cd dofbot_ws/src
git clone https://github.com/bmiesch/act_dofbot
~~~

### Build and run ROS Server

~~~
cd dofbot_ws/
catkin_make
source devel/setup.bash
roslaunch dofbot_info dofbot_server.launch
~~~

### Run game with classical controls and collect data
~~~
python game_driver.py
~~~
*The data will be saved in a h5py file in data/pick_and_place/

### Train Model
*Training the model will take a while (1-2 hours).
~~~
python train.py --task pick_and_place
~~~

### Run game with the model
~~~
python model_control.py
~~~
*You'll need to manual confirm each set of joints before the robot will move.
You can turn this off but commenting out line 193 in model_control.py


### PyTorch 1.10 Installation
The PyTorch wheel file for the Jetson Nano can be downloaded from the following link:

[Download PyTorch Wheel for Jetson Nano](https://nvidia.box.com/shared/static/fjtbno0vpo676a25cgvuqc1wty0fkkg6.whl)


## Installation Steps

Follow these steps to install PyTorch on your Jetson Nano:

1. **Navigate to the Download Directory**

2. **Install the Wheel File**

   Install the wheel file using pip by running the following command:
   `pip3 install fjtbno0vpo676a25cgvuqc1wty0fkkg6.whl`

3. Verify the Installation

   Run the following command to verify that PyTorch is installed correctly:
   `python3 -c "import torch; print(torch.__version__)"`

   This should print the version number of the installed PyTorch (1.10.0).
