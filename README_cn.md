 D-robotics-rdloc5 智能小车项目
本项目基于ROS2开发，整合SLAM Toolbox建图、2D导航路标跟综、YOLOv8目标识物、人脸检测，搭配TTS文字转音频与自定义voice_broadcaster语音播报文件，实现智能小车自主移动、环境感知与语音播报全流程功能。

核心功能特性
1.  建图功能：采用SLAM Toolbox实现激光SLAM实时建图，支持栅格地图生成、保存与加载，适配小车实时环境建模需求；
2.  导航功能：基于ROS2 2D导航框架，主打路标跟综模式，可按预设路标完成自主路径规划，同时具备动态避障能力；
3.  视觉感知功能：集成YOLOv8目标检测模型，实现多类环境物体的识别、定位与分类；新增人脸检测功能，支持人脸的实时检测与跟踪；
4.  语音播报功能：通过TTS完成文字到音频的转换，结合自定义开发的voice_broadcaster语音播报文件，实现检测结果、导航状态等信息的自主语音播报。

项目文件结构
因单文件体积限制，项目拆分为2个压缩包，内容分工清晰：
源代码(1).zip：包含SLAM建图、2D导航路标跟随、YOLOv8目标识物、语音播报，人脸检测的全部核心源码及对应配置文件；
tts_make_ros2.zip：包含TTS文字合成音频模块源码，以及讯飞.jet发音人资源的配置逻辑文件。

运行环境与依赖
操作系统：Ubuntu 20.04 或 Ubuntu 22.04
ROS版本：ROS2 Humble / ROS2 Iron
核心依赖库（需提前安装）：
  建图导航类：ros-slam-toolbox、ros-navigation2
  视觉检测类：OpenCV 4.x、Ultralytics YOLOv8、dlib（人脸检测配套依赖）
  语音合成类：讯飞离线TTS SDK、Python 3.8及以上版本

编译与运行步骤
1.  将两个项目压缩包，全部解压到你的ROS2工作空间下的src目录中；
2.  进入ROS2工作空间根目录，执行编译与环境变量配置命令：
3.  启动SLAM建图功能（生成环境地图）：
    ros2 launch wheeltec_slam_toolbox online_async_launch.py
4.  启动2D导航路标跟踪功能：
    ros2 launch wheeltec_nav2 wheeltec_nav2launch.py
    打开rviz2,选择路标点，开始目标跟踪
6.  启动视觉感知功能（YOLOv8识物+人脸检测）：
    ros2 launch d_robotics_rdloc5 detect_launch.py
7.  启动自定义语音播报功能（触发对应场景语音输出）：
    ros2 launch tts tts_make.launch.py
    ros2 run voice_broadcaster detection_subscriber

关键注意事项
1.  讯飞.jet发音人文件为闭源资源，需自行前往[讯飞开放平台](https://www.xfyun.cn/)申请，下载后放置到tts_make_ros2/config/bin/msc/res目录；
2.  YOLOv8预训练模型权重需替换为适配自身场景的模型，权重文件路径在config/yolov8_config.yaml中修改配置；
3.  人脸检测模块若使用dlib相关功能，需单独下载dlib预训练模型，同时配置好模型文件的本地路径；
4.  路标跟随功能需提前在RViz可视化界面中设置好导航路标，路标相关参数在config/waypoints.yaml中调整；
5.  若编译时提示依赖缺失，可根据终端报错信息，用sudo apt install命令补全对应依赖。

开源协议
本项目采用MIT License开源协议，详细协议内容见仓库根目录下的LICENSE文件。
