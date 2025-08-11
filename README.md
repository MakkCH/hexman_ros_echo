# HEXMAN_ROS_使用指南

**Disclaimer: 本項目並非由HEXMAN尋界機械人開發、認可、贊助或附屬。**

官方SDK只提供了纯底盘ROS开发包，而導航工具（i.e. SLAM, Move base 和 導航）則沒有提供。需要自行開發

> [!warning]
> 警告：本項目暫時只適用於ECHO型號+rplidar激光雷達，其他型號需自行修改參數

## 1. 安裝

請先確保`ros-noetic`已正確安裝，並且已安裝 [`rplidar_ros`](https://wiki.ros.org/rplidar)

### 1.1. 安裝官方SDK
> 如之前已安裝過，可以則跳過

於命令行執行以下命令：
```bash
wget -O hextool.bash https://ros.dl.hexman.cn/hextool.bash && bash hextool.bash
```
參照 https://docs.hexman.cn/ROS/ROS-SDK/#ros 進行安裝

### 1.2. 安裝本項目
下面以`ECHO SDK`為例
```bash
cd ~/sdk_echo_ws/src/demo/demo/
rm -rf demo_general_chassis # 刪去原有demo，或者mv demo_general_chassis demo_general_chassis.old
git clone https://github.com/MakkCH/hexman_ros_echo.git demo_general_chassis
```
下載完成後，回到`~/sdk_echo_ws`構建包
```bash
cd ~/sdk_echo_ws
catkin_make
```

## 2. 啟動底盤

使用本項目提供bringup包與rplidar包，執行以下命令後
鑑於`rplidar`和`chassis`會出現衝突，如要同時打開雷達和底盤，建議按照以下步驟執行

1. 確保主機和`chassis`斷聯，並執行
```bash
roslaunch xpkg_demo chassis_rplidar.launch
```
2. 連接`chassis`，並執行
```bash
roslaunch xpkg_demo chassis_bringup.launch
```

這應會啟動底盤以及雷達，可用rviz打開`rviz/rviz_vehicle.rviz`檢查是否正確啟動

> [!tip]
> 可用以下命令透過鍵盤移動底盤
> ```bash
> roslaunch xpkg_demo chassis_key_ctrl.launch
> ```

## 3. 導航與SLAM

如要進行SLAM, 你可執行以下命令
```bash
roslaunch xpkg_demo chassis_slam.launch
```
保存地圖：`rosrun map_server map_saver -f <your-map-name>`
詳看[`map_server`](https://wiki.ros.org/map_server)

如要進行導航，你可執行以下命令
```bash
roslaunch xpkg_demo chassis_navigation.launch map_file:=<your-map-file-path>
```
