D-robotics-rdloc5 Smart-Car Project

This project is built on ROS2 and integrates SLAM Toolbox mapping, 2-D waypoint navigation, YOLOv8 object detection, face detection, TTS text-to-speech, and a custom voice_broadcaster node, enabling the robot to move autonomously, perceive its environment, and speak the results.

Core Features  
1. Mapping – real-time 2-D LiDAR SLAM via SLAM Toolbox; generate, save, and reload occupancy-grid maps on-the-fly.  
2. Navigation – ROS2 2-D navigation stack in waypoint-following mode; plans paths to preset waypoints and avoids dynamic obstacles.  
3. Vision – YOLOv8 for multi-class object localization & classification; real-time face detection & tracking.  
4. Speech – offline TTS converts text to audio; custom voice_broadcaster node automatically announces detection results and navigation status.

Package Layout

Because of file-size limits the project is split into two archives:

source_code(1).zip – all core source code and config files for SLAM, navigation, YOLOv8, face detection, and voice broadcasting.

tts_make_ros2.zip – TTS text-to-speech module source plus iFlytek .jet voice-resource configuration logic.

Runtime Environment & Dependencies

OS: Ubuntu 20.04 or 22.04

ROS2: Humble or Iron

Key libraries (install beforehand):

Mapping & nav: ros-slam-toolbox, ros-navigation2

Vision: OpenCV 4.x, Ultralytics YOLOv8, dlib (for face detection)

Speech: iFlytek offline TTS SDK, Python ≥ 3.8

Build & Run  
1. Extract both archives into your ROS2 workspace src folder.  
2. From the workspace root compile and source:

   colcon build && source install/setup.bash  
3. Start SLAM mapping (generate the map):

   ros2 launch wheeltec_slam_toolbox online_async_launch.py  
4. Launch 2-D waypoint navigation:

   ros2 launch wheeltec_nav2 wheeltec_nav2launch.py

   Open rviz2, set waypoints, and start following.  
5. Launch vision pipeline:

   ros2 launch dnn_node_example dnn_node_example.launch.py dnn_example_config_file:=config/yolov8workconfig.json dnn_example_image_width:=1920 dnn_example_image_height:=1082  
6. Launch custom voice announcements:

   ros2 launch tts tts_make.launch.py

   ros2 run voice_broadcaster detection_subscriber  

Important Notes  
1. The iFlytek .jet voice file is closed-source; apply on the [iFlytek Open Platform](https://www.xfyun.cn/), download it, and place in tts_make_ros2/config/bin/msc/res.  
2. Replace the YOLOv8 pretrained weights with a model suited to your scene; path is set in config/yolov8_config.yaml.  
3. If you use dlib for face detection, download its pretrained model and update the local path accordingly.  
4. Waypoint following requires preset waypoints in RViz; adjust parameters in config/waypoints.yaml.  
5. If dependencies are missing during build, install them with sudo apt install according to the terminal errors.

License

Released under the MIT License; see LICENSE in the repository root.
