# ROS CONNECTION TO NAO ROBOT

There are three things we need for this connection: *ROS*, *NAO* and *ros package* to connect the robot with ROS.
1. I assumed you already install ROS. If not, go to Google, search for "install ros" and there will be instructions to follow on ros.org website.
  
2. We will be simulating the bringup between a virtual NAO and ROS, then try it on a real NAO robot (if you had one)
- There are three things we need to setup: Choregraphe (or Rviz), C++ SDK and Python SDK.

  - From [Softbank Robotics webpage](https://www.softbankrobotics.com/emea/en/support/nao-6/downloads-softwares). Download:
    - Choregraphe v2.1.4
    - C++ SDK v2.1.4
    - Python SDK v2.8 (make sure you are using python 2.7)

  *Note:* you can download any version, just make sure that they are in same generation.
  
  - (optional) For Rviz, simply...
    ```
    sudo apt-get update
    sudo apt-get install rviz
    ```
  - After downloading those three from the webpage, extract them. For Python SDK 2.8, I named my extracted file as *naoqi2.8*, then
    ```
    echo export PYTHONPATH=${PYTHONPATH}:<link-to-site-packages-of-extracted-file> >> ~/.bashrc
    echo export QI_SDK_PREFIX=<link-to-extracted-file> >> ~/.bashrc
    ```
    - For example:
    ```
    PYTHONPATH=${PYTHONPATH}:/home/myuser/Downloads/naoqi2.8/lib/python2.7/site-packages
    QI_SDK_PREFIX=/home/myuser/Downloads/naoqi2.8
    ```
    - For other versions of Python SDK, please follow the installation guide from Aldebaran site.
      
3. For the ros package,
  ```
  sudo apt-get install ros-*-nao-robot  (* for your version of ros, for example, ros-kinetic-nao-robot)
  sudo apt-get install ros-*-nao-meshes
  ```
_That is all the setup we need to do. It is time for the main course._

*Note:* If you are using Virtual Machine, now is the time to change network adapter setting to bridge network.

1) roscore
  
2) Create a NAO broker(virtual NAO) with C++ SDK
```
.<link-to-C++-SDK-extracted-file> - verbose - broker-ip <nao-ip-adress>
```
For example:
```
./Downloads/naoqi-sdk-2.1.4.13-linux64/naoqi - verbose - broker-ip 127.0.0.1
```
3) Open Choregraphe and connect to the robot we just created

4) Tell the system where to find ros packages (in this case, nao_bringup)
```
source /opt/ros/kinetic/setup.bash
```
5) Bringup NAO with ROS
```
roslaunch nao_bringup nao_full_py.launch nao_ip:=<nao-ip-adress> nao_port:=<nao-port-no>
```
For example:
```
roslaunch nao_bringup nao_full_py.launch nao_ip:=127.0.0.1 nao_port:=9559
```
6) List out all the topic to see if ROS and NAO have connected
```
rostopic list
```
- There should be at least these topic:
  - joint_states
  - tf
  - top camera
  - bottom camera
  - left sonar
  - right sonar
  - microphone
    
7) (optional) To see the robot in Rviz, simply `rosrun rviz rviz`. Make sure to change fixed frame into base_link and add..robot model
    
8) Finally, write some python scripts with NAO API, put it in any package and rosrun it.
```
rosrun <package> <script>
```
For example, I put my script NAOtest in package called hello_world:
```
rosrun hello_world NAOtest
```

*Note:* the dialog that NAO says can only be viewed in Choregraphe

With real robot, we only need step 1,4,5,6,8 with step 5 being real ip address and port number of NAO.
