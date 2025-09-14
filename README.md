
## 模块介绍

| 模块 | 说明 |
| --- | --- |
| [`lidar`](./src/lidar/) | 激光雷达模块 |
| [`camera`](./src/tdt_vision/) | 相机模块（无相机驱动） |
| [`interface`](./src/interface/) | 自定义消息接口 |
| [`livox_driver`](./src/livox_driver/) | Livox驱动 |
| [`fusion`](./src/fusion/) | 传感器后融合模块 |
| [`utils`](./src/utils/) | 工具包 |
| [~~`llm_decision`~~](./src/llm_decision/) | ~~大模型决策模块~~ |

## 依赖

```bash
Ubuntu 24.04
ROS2 (Jazzy)
CUDA+CUDNN+TensorRT10
OpenCV 4.6.0
PCL 1.14.0
Livox_SDK(1)
```
>友情链接：Livox_SDK(1)-2404：https://github.com/mozihe/Livox-SDK-24.04
## 进程间通信消息名称及用途

### 1. ROS2 通信

#### 激光雷达

| 名称 | 类型 | 用途 |
| --- | --- | --- |
| livox/lidar | topic< sensor_msgs::msg::PointCloud2 > | Livox驱动接口 |
| livox/map | topic< sensor_msgs::msg::PointCloud2 > | 3D地图可视化 |
| livox/lidar_dynamic | topic< sensor_msgs::msg::PointCloud2 > | 动态点云 |
| livox/cluster | topic< sensor_msgs::msg::PointCloud2 > | 聚类结果 |
| livox/lidar_detect | topic< vision_interface::msg::RadarWarn > | 激光雷达预警 |



#### 相机

| 名称 | 类型 | 用途 |
| --- | --- | --- |
| camera_image | topic< sensor_msgs::msg::Image > | 相机驱动接口 |
| detect_result | topic< vision_interface::msg::DetectResult > | 识别结果 |
| resolve_result | topic< vision_interface::msg::DetectResult > | 解算结果 |


#### 传感器融合

| 名称 | 类型 | 用途 |
| --- | --- | --- |
| kalman_detect | topic<vision_interface::msg::DetectResult> | 卡尔曼节点输出 |
| match_info | topic<vision_interface::msg::MatchInfo > | 当前比赛的实时信息 |
| Radar2Sentry | topic<vision_interface::msg::Radar2Sentry> | 发送给串口的最终结果 |


## 测试
    
```bash
ros2 launch tdt_vision run_rosbag.launch.py #通过rosbag启动相机
ros2 launch dynamic_cloud lidar.launch.py #启动激光雷达识别
ros2 run debug_map debug_map #启动地图可视化
ros2 launch livox_ros2_driver livox_lidar_launch.py #启动Livox驱动
```
测试ros2bag下载
[谷歌网盘](https://drive.google.com/file/d/1hBJER_jnCrIQ21ojHj7HG4pWShSiCSPD/view?usp=sharing)

修改tdt_vision/launch对应的launch文件中的rosbag路径即可进程内播放对应的rosbag
### 相机外参标定
```bash
ros2 launch tdt_vision calib_rosbag.launch.py
```
按Enter键开始标定,依次点击我方堡垒靠近我方的底角，我方前哨站血条底部，敌方基地引导灯，敌方前哨战引导灯，敌方四十三度坡围挡左底角。


每次点击后可使用wasd调节上下左右，按n键保存当前点，保存5个点后自动计算外参并保存在config/out_matrix.yaml
## 可视化
Launch文件已集成foxglove-bridge,启动后直接打开foxglove-studio即可查看
## TODO
- 多相机/雷达从当前逻辑(结构)上可以实现，但是并没有进行对应ros2接口的适配
- 改进聚类与匹配算法
- 使用ros参数，实时调参

</div>
