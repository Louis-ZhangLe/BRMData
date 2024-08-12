# BRMData
A Bimanual-mobile Robot Manipulation Dataset specifically designed for household applications

# 1 environment setup

1. download
~~~python
git clone https://github.com/agilexrobotics/cobot_magic.git
~~~

2. compilation
~~~python
cd cobot_magic/remote_control
./tools/build.sh

cd cobot_magic/camera_ws
catkin_make
~~~


# 2. testing

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


# 3 collect data

~~~python
# 1 start roscore
roscore

# 2 activate the robotic arm and camera
./tools/remote.sh

## 3 collect data
python collect_data.py --max_timesteps 500 --dataset_dir ./data --episode_idx 0
~~~

# 4 model train and inference

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

---


We will update various robot manipulation methods in the future, including MT-act and so on.