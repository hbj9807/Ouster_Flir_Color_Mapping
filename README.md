#Ubuntu 20.04 LTS

#ROS Installation
https://wiki.ros.org/noetic/Installation/Ubuntu

#Ouster-ROS Install
https://github.com/ouster-lidar/ouster-ros

#OpenCV 3.4.16
https://docs.opencv.org/3.4/d7/d9f/tutorial_linux_install.html
(git checkout 3.4.16)

#Spinnaker ~ Install
Install Spinnaker SDK 2.2.0.48 -> fix static IP
Remove 2.2.0.48
Install Spinnaker SDK 1.26.0.31 -> for launching Flir_adk_Ethernet

#Flir 
https://github.com/FLIR/flir_adk_ethernet

#Network Setting for Scanning
Ouster  // IPv4 > Manual > 169.254.71.1 / 255.255.255.0 / 169.254.71.2
Livox  // IPv4 > Manual > 192.168.1.1XX / 255.255.255.0 / (XX: Serial Last Number)
IPv6 > Link-Local Only

#Calibartion
https://github.com/koide3/direct_visual_lidar_calibration?tab=readme-ov-file

roscore

rosrun direct_visual_lidar_calibration preprocess ouster ouster_preprocessed -dv \
  --camera_info_topic /flir_adk/camera_info \
  --image_topic /flir_adk/image_raw \
  --points_topic /ouster/points \
  --camera_model plumb_bob \
  --camera_intrinsic 1119.798146,1118.683090,740.720832,530.000662 \
  --camera_distortion_coeffs -0.363603,0.131881,0.001062,-0.000594,0.000000

rosrun direct_visual_lidar_calibration preprocess livox livox_preprocessed -dv \
  --camera_info_topic /flir_adk/camera_info \
  --image_topic /flir_adk/image_raw \
  --points_topic /livox/lidar \
  --camera_model plumb_bob \
  --camera_intrinsic 1119.798146,1118.683090,740.720832,530.000662 \
  --camera_distortion_coeffs -0.363603,0.131881,0.001062,-0.000594,0.000000

rosrun direct_visual_lidar_calibration preprocess ouster ouster_preprocessed -adv\

rosrun direct_visual_lidar_calibration initial_guess_manual ouster_preprocessed
rosrun direct_visual_lidar_calibration initial_guess_manual livox_preprocessed

rosrun direct_visual_lidar_calibration calibrate ouster_preprocessed
rosrun direct_visual_lidar_calibration calibrate livox_preprocessed

rosrun direct_visual_lidar_calibration calibrate ouster_preprocessed

rosrun direct_visual_lidar_calibration viewer ouster_preprocessed

matrix calculator: https://staff.aist.go.jp/k.koide/workspace/matrix_converter/matrix_converter.html

[ .. , .. , .. , ..
  .. , .. , .. , ..
  .. , .. , .. , ..
  .. , .. , .. , ..]

  3x3: rotation matrix --> needs inverse matrix
  3x1: translation matrix

#livox_ros_driver_for_R2LIVE Install
https://github.com/ziv-lin/livox_ros_driver_for_R2LIVE
input: serial 
roslaunch livox_ros_driver livox_lidar.launch 
(xfer_format: 0 --> PointCloud2 msg 
            : 1 --> CustomMsg) /
            
#R3LIVE Install
https://github.com/hku-mars/r3live#21-strong-robustness-in-various-challenging-scenarios

roslaunch r3live r3live_bag_ouster.launch

roslaunch r3live r3live_reconstruct_mesh.launch

#Record Data
roslaunch ouster_ros driver.launch 
roslaunch flir_adk_ethernet blackfly.launch 
rosbag record -b 2048 /ouster/points /ouster/imu /flir_adk/image_raw 
rosbag record -b 2048 /livox/lidar /livox/imu /flir_adk/image_raw


inadyn-mt --username hbj9807@gmail.com --password PfFiaYU9BeLy1tmy7uKPZ7Hk8fnzsw \
          --alias hbj9807.dynv6.net,hash ip6 \
          --dyndns_server_name dynv6.com\
          --dyndns_server_url /nic/update \
          --dyndns_system custom@http_svr_basic_auth \
          --verbose 5

http://dynv6.com/api/update?hostname=[hbj9807]&token=[PfFiaYU9BeLy1tmy7uKPZ7Hk8fnzsw]&ipv6prefix=[fe80::1e10:1e28:cbce:b66f]h
