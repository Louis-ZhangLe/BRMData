# BRMData
A Bimanual-mobile Robot Manipulation Dataset specifically designed for household applications.

# Environment Setup

1. download
~~~python
git clone https://github.com/Louis-ZhangLe/BRMData.git
~~~

2. compilation
~~~python
cd cobot_magic/remote_control
./tools/build.sh

cd cobot_magic/camera_ws
catkin_make
~~~


# Testing

~~~python
# 1 setup rule
ls /dev/ttyACM*

udevadm info -a -n /dev/ttyACM* | grep serial -m 1

sudo vim /etc/udev/rules.d/arx_can.rules

sudo udevadm control --reload && sudo  udevadm trigger

# 2 start remote arm
cd remote_control
./tools/can.sh

cd master1

source devel/setup.bash

roslaunch arm_control arx5v.launch
~~~


# Collect Data

~~~python
# 1 start roscore
roscore

# 2 activate the robotic arm and camera
./tools/remote.sh

## 3 collect data
python collect_data.py --max_timesteps 500 --dataset_dir ./data --episode_idx 0
~~~

# Model Train and Inference

~~~python
# 1 
conda activate aloha

# 2 train
python act/train.py --dataset_dir ~/data0314/ --ckpt_dir ~/train0314/ --batch_size 4 --num_epochs 3000

# 3 inference

cd remote_control
./tools/puppet.sh


python act/inference.py --ckpt_dir ~/train0314/
~~~

# Methods
We will update various robot manipulation methods in the future, including MT-ACT and so on. The methods and models involved are listed below:

Action Chunking with Transformers (ACT): https://github.com/MarkFzp/act-plus-plus

Diffusion Policy (DP): https://github.com/real-stanford/diffusion_policy

Multi-Task ACT (MT-ACT): https://github.com/robopen/roboagent/

Efficientnet-B3: https://github.com/tensorflow/tpu/tree/master/models/official/efficientnet

R3m: https://github.com/facebookresearch/r3m

# License
The BRMData datasets are both licensed under MIT License

# Citation
If you find the repository helpful, please consider citing our paper
~~~cite
@inproceedings{zhang2024empowering,
  author    = {Zhang, Tianle and Li, Dongjiang and Li, Yihang and Zeng, Zecui and Zhao, Lin and Sun, Lei and Chen, Yue and Wei, Xuelong and Zhan, Yibing and Li, Lusong and He, Xiaodong},
  title     = {Empowering Embodied Manipulation: A Bimanual-Mobile Robot Manipulation Dataset for Household Tasks},
  booktitle = {arXiv},
  year      = {2024},
}
~~~




---