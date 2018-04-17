# A4_page_turning

Project Description:

 This is a ROS based soft gripper manipulation project. The intention is to discover the suitable parameters to successifully turn an A4 size paper.

Hardware setup:

    Arduino Board ; DAC Circuit ; Power Supply ; SMC ITV0030-2S pressure regulartor ; UR10 Robot Arm
  

Software Environment:

    Ubuntu 16.04 LTS ; ROS Kinetic

Additional Necessary ROS packages:

    ur_modern_driver ; apriltrags_ros ; image_pipline ; universal_robot ; rosserial ; usb_cam ; image_common
  
Three major parts of this project:

  1)ROS & Arduino
  
     Arduino is used in this project to generate a pwm wave to control soft robotics fingers.
     
     To Run this part, upload c code to Arduino first. c code can be found at /src/A4_paper_turning_Arduino.c
     
     Then: in terminal ROS environment.
     
     a) Open a roscore:
     
     $ roscore    
     
     b) Connect to serial_node (note that node name maybe different, check on arduino IDE first):
     
     $ rosrun rosserial_python serial_node.py /dev/ttyACM0
     
     c) Use rostopic to control:
     
     $ rostopic pub soft std_msgs/Empty --once
     
   2)Camera & Apriltags: 
   
     Camera must be calibrated first to ensure vision is rectified
     
     In termianl ROS environment:
     
     a) Launch usb_cam: (note that camera id maybe different if an internal camera is embedded in your computer)
     
     $ roslaunch usb_cam usb_cam-test.launch 
     
     b) Connect usb_cam to image proc:
     
     $ ROS_NAMESPACE=usb_cam rosrun image_proc image_proc
     
     c) Launch apriltags (note that dimension of each apriltag should be measured and stated in this launch file)
     
     $ roslaunch apriltags_ros example.launch 
     
     d) Monitor apriltag info：
     
     $ rostopic echo /tag_detections
     
   ３）Moveit & UR robot arm
   
     In termial ROS environment, check network connection first, in my case, it is:
     
     $ ping 192.168.1.10
     
     a) Launch ur_modern_driver:
     
     roslaunch ur_modern_driver ur10_bringup.launch robot_ip:=192.168.1.10 [reverse_port:=REVERSE_PORT]
     
     b) Launch Moveit:
     
     $ roslaunch ur10_moveit_config ur10_moveit_planning_execution.launch limited:=true 
     
     c) Launch Rviz:
     
     $ roslaunch ur10_moveit_config moveit_rviz.launch config:=true 
     
     d) Run python code:
     
     $ rosrun ur_moveit_myplan page_turning_april.py
     
  
