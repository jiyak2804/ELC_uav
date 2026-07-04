# ELC UAV — LiDAR Setup

## To run:
1. Launch PX4 SITL with custom world (spawn at start station, NOT default pose):
   PX4_GZ_WORLD=elc PX4_GZ_MODEL_POSE="-6.0,-6.0,2.3,0,0,0" make px4_sitl gz_x500_lidar_2d
   (run this in a terminal WITHOUT ROS2 sourced — ROS's vendored Gazebo libs conflict with system Gazebo)

2. In a ROS2-sourced terminal, start the Micro XRCE-DDS Agent:
   MicroXRCEAgent udp4 -p 8888

3. In another ROS2-sourced terminal, launch the bridge:
   ros2 launch elc_bridge elc_bridge.launch.py

4. Publish a temporary static TF (until real odometry->TF bridge is added):
   ros2 run tf2_ros static_transform_publisher --x 0 --y 0 --z 0 --roll 0 --pitch 0 --yaw 0 --frame-id world --child-frame-id link

5. Open RViz with the saved config:
   rviz2 -d elc_lidar.rviz

## Known issues / in progress:
- TF is currently static (drone position won't update as it moves) — need to bridge PX4 odometry to /tf for real SLAM mapping
- Working on slam_toolbox integration next
