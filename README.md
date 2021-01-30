### ROS noetic and moveit-instructions
Instructions to install moveit from source if you are using a virtualbox.

Remember that opening the terminal to access bash is as easy as pressing ctrl+alt+T at the same time.

# ROS NOETIC INSTALL 
---
```
sudo apt-get install python3-pip -y
sudo sh -c 'echo "deb http://packages.ros.org/ros/ubuntu $(lsb_release -sc) main" > /etc/apt/sources.list.d/ros-latest.list'
```
### Set up your keys
```
sudo apt-key adv --keyserver 'hkp://keyserver.ubuntu.com:80' --recv-key C1CF6E31E6BADE8868B172B4F42ED6FBAB17C654
```
### Installation
```
sudo apt update -y
sudo apt install ros-noetic-desktop-full -y
echo "source /opt/ros/noetic/setup.bash" >> ~/.bashrc
source ~/.bashrc
```

# MOVEIT     INSTALL 
---
# STEP 1
### get Ubuntu 20.04 and install ROS Noetic
Get the latest versions of ROS packages installed:
```
sudo apt install python3-rosdep2 -y
rosdep update
sudo apt update -y
sudo apt dist-upgrade  -y
```
Source installation requires wstool, catkin_tools, and optionally clang-format:
```
sudo apt install python3-wstool python3-catkin-tools clang-format-10 -y
```
go to where you want the installation
```
mkdir $HOME/workspace/ws_moveit -p
cd $HOME/workspace/ws_moveit 
```

# STEP 2
### Download Source Code
Pull down required repositories and build from within the root directory of your catkin workspace:
```
wstool init src
wstool merge -t src https://raw.githubusercontent.com/ros-planning/moveit/master/moveit.rosinstall
wstool update -t src
rosdep install -y --from-paths src --ignore-src --rosdistro ${ROS_DISTRO}
sudo apt-get install python3-osrf-pycommon python3-catkin-tools -y
catkin config --extend /opt/ros/${ROS_DISTRO} --cmake-args -DCMAKE_BUILD_TYPE=Release
```

Note October 2020 Until the next ROS release sync, you will need to install ros-noetic-fcl manually from the ros-testing repositories:
```
sudo apt install ros-noetic-fcl -y
```
# STEP 3
### Build MoveIt
we shouldn't have to run this funny looking catkin_make but if allowed a clean build.  [reference](https://answers.ros.org/question/353111/following-installation-instructions-catkin_make-generates-a-cmake-error/)
```
#sudo apt install ros-noetic-rostest
catkin_make -DPYTHON_EXECUTABLE=/usr/bin/python3
```
### Source the Catkin Workspace
Setup your environment - you can do this every time you work with this particular source install of the code, or you can add this to your .bashrc (recommended):
```
source ~/workspace/ws_moveit/devel/setup.bash 
```
# STEP 4
### lets party
Open another terminal and make sure it's pointing to the same directory and has the necessary environment variables.
```
cd ~/workspace/ws_moveit
source /opt/ros/noetic/setup.bash
source ~/workspace/ws_moveit/devel/setup.bash 
```
In that same terminal 
```
roslaunch panda_moveit_config demo.launch
```



In the original terminal run the following after the previous command is fully loaded
```
sudo apt install ros-noetic-rosbash -y
rosrun moveit_tutorials pick_place_tutorial
```
# STEP DONE
### try the other demo's 
[Move Group C++ Interface](https://ros-planning.github.io/moveit_tutorials/doc/move_group_interface/move_group_interface_tutorial.html)
Running the Code
Open two shells. In the first shell start RViz and wait for everything to finish loading:
```
roslaunch panda_moveit_config demo.launch
```
In the second shell, run the launch file:
```
roslaunch moveit_tutorials move_group_interface_tutorial.launch
```
[Move Group Python Interface](https://ros-planning.github.io/moveit_tutorials/doc/move_group_python_interface/move_group_python_interface_tutorial.html)
Start RViz and MoveGroup node
Open two shells. Start RViz and wait for everything to finish loading in the first shell:
```
roslaunch panda_moveit_config demo.launch
```
Now run the Python code directly in the other shell using rosrun. Note in some instances you may need to make the python script executable:
```
rosrun moveit_tutorials move_group_python_interface_tutorial.py
```

enjoy :)
