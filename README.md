# 20260928
开学第三周
12.17
# ros1的nav1中有导航和避障的功能，不用在额外写
48.编写launch文件，一键启动Gmapping建图
catkin_create_pkg slam_pkg roscpp rospy std_msgs
<node pkg="gmapping" type="slam_gmapping" name="slam_gmapping" output="screen"/>,name是随意起的
roslaunch slam_pkg gampping.launch 
49.Gmapping建图的参数设置
<img width="961" height="520" alt="image" src="https://github.com/user-attachments/assets/dec4e1a0-96dc-4688-bcbf-9fba80ec7a24" />
50.如何在 ROS 中保存和加载地图
rosrun map_server(节点所在的包) map_saver(节点) -f lll
rosrun map_server map_server lll.yaml
rviz
51.5分钟，看懂ROS的Navigation导航系统
<img width="977" height="745" alt="2026-09-28 14-18-53屏幕截图" src="https://github.com/user-attachments/assets/a310ebeb-87cf-48fe-9c65-19ef7cf58ddb" />
### 原来move_base真在这里
52.move_base，年轻人的第一次导航
1，move_base导航节点
2，map_server地图服务节点，将map_server运行起来，并将导航需要的地图文件加载进去，move_base自动获取这些数据
3，sensor source多个传感器节点,仿真机器人模型只要加载到仿真环境中，自动会输出这些数据到相应的话题
4，里程计节点，和仿真机器人一样，都可以由仿真机器人的模型自己提供
5，传感器位置的tf
6,amcl节点，像map_server节点一样运行起来就行了
建图，roslaunch wpr_simulation wpb_gmapping.launch
roslaunch wpr_simulation key_board_vel_ctrl
保存rosrun map_server(节点所在的包) map_saver(节点) -f lll
加载rosrun map_server map_server lll.yaml
写nav.launch
catkin_make
roslaunch wpr_simulation wpb_stage_robocup.launch
roslaunch nav_pkg nav.launch 
rviz
53.ROS导航系统 | 全局规划器
<img width="1008" height="535" alt="2026-09-28 15-46-16屏幕截图" src="https://github.com/user-attachments/assets/0a920473-294b-4e0d-ba33-63c09c1e00de" />
<param name="base_global_planner" value="global_planner/GlobalPlanner" /> 默认使用迪杰斯特拉算法，而非A*，前者可找到最优路径
<img width="1003" height="494" alt="2026-09-28 15-54-33屏幕截图" src="https://github.com/user-attachments/assets/c65dfad2-9f17-435d-9177-6442b0227c82" />
54.ROS导航系统 | AMCL(自适应的蒙特卡洛定位算法) 定位算法  定位机器人在哪里
在已知地图中进行定位的算法，他同时使用了里程计和激光雷达数据
<img width="1063" height="739" alt="2026-09-28 16-21-37屏幕截图" src="https://github.com/user-attachments/assets/1891a86e-20c0-474a-a11f-96456bc92687" />
amcl节点负责输出map到odom的tf，里程计负责输出odom到base_footprint的tf,这样rviz就能在地图上显示机器人的位置
amcl节点切换本体和分身是在map到odom这段tf上产生跳跃突变来实现的所以在导航中能看到机器人一蹦一蹦的
catkin_make
roslaunch wpr_simulation wpb_stage_robocup.launch
roslaunch nav_pkg nav.launch 
rviz
55ROS导航系统 | 代价地图 Costmap
局部规划器主要是用来避障的
把rviz显示设置保存成文件,在rviz里选择保存
在nav.launch中调用    <node pkg="rviz" type="rviz" name="rviz" args="-d $(find nav_pkg)/rviz/nav.rviz"/>
56.ROS导航系统 | 代价地图的参数设置
catkin_make
roslaunch wpr_simulation wpb_stage_robocup.launch
roslaunch nav_pkg nav.launch 
顶部摄像头三维点云
全局代价地图中
如果想边建图边导航，修改static_map: true
如果tf有timeout，那么把transform_tolerance: 1.0改大
局部代价地图代码以odom作为参考系，测出的障碍物位置不易跳变
57.ROS导航系统 | 恢复行为 | Recovery Behaviors
<img width="923" height="634" alt="2026-09-28 20-22-00屏幕截图" src="https://github.com/user-attachments/assets/dc1f43cb-b930-4ee1-86b6-cd99052e0440" />
58，ROS导航系统 | 恢复行为的参数设置 | Recovery Behaviors
<img width="1203" height="749" alt="2026-09-28 20-37-12屏幕截图" src="https://github.com/user-attachments/assets/f6622af6-a5d5-4aca-90c1-cde0634bba6d" />
59.ROS导航系统 | 局部规划器 | Local Planner
机器人的导航路线是由全局规划器产生的，但机器人最后走成什么样，是由局部规划器决定的，局部规划器其实就是机器人的运动控制器
<param name="base_local_planner" value="wpbh_local_planner/WpbhLocalPlanner" />，人工势场算法
<img width="1075" height="393" alt="2026-09-28 20-50-40屏幕截图" src="https://github.com/user-attachments/assets/aea9683c-a6df-4983-bce3-ea34c10cec5c" />
60.ROS导航系统 | DWA规划器 | DWA Planner
动态窗口方法动态窗口，生成一系列轨迹和方案
  <img width="1009" height="69" alt="2026-09-28 20-55-29屏幕截图" src="https://github.com/user-attachments/assets/4b3c08f7-7775-4c37-a71e-358155e91af1" />
rosrun rqt_reconfigure rqt_reconfigure 在线调参
61.ROS导航系统 | TEB规划器 | TEB Planner
  时间弹力带

<img width="1045" height="608" alt="2026-09-28 21-17-17屏幕截图" src="https://github.com/user-attachments/assets/b94a67b2-2aef-467f-8a81-a6cd4b0f6f8a" />
<img width="989" height="101" alt="2026-09-28 21-20-41屏幕截图" src="https://github.com/user-attachments/assets/fc5efdcc-3c4d-464d-8789-413afd4b4674" />
到目标点会有弧线倒车的现象，倒车就要注意雷达能不能检测到后方，结构上是和阿克曼
rosrun rqt_reconfigure rqt_reconfigure 在线调参
62.ROS导航系统 | Action 编程接口
机器人是自主导航的，不能每次都手动设置导航·的目标点
使用navfation的导航接口自主导航，推荐使用action接口，action是双向的，

# 20260929
用action调用move_base的导航功能
C++编写客户端
. 和 :: 的区别：

:: 是作用域解析运算符，用来访问 命名空间或类里面的名字，比如 std::cout、ros::init。//:: 的基本含义就是：前面的东西是“范围”，后面的东西是这个范围里的名字

. 是成员访问运算符，用来访问 某个具体对象里面的成员，比如 goal.target_pose、ac.sendGoal(goal)。//. 就是“的”的意思，用来访问一个对象内部的成员。
65.一款开源的 ROS 航点导航插件
roslaunch wpr_simulation wpb_map_tool.launch 
rosrun wpr_simulation demo_map_tool
66.ROS 航点导航插件的集成和启动
在move_base的action的接口处，增加一个wp_navi_sever节点，他会按照前面坐标导航的方法，调用move_base的导航功能，只用启动wp_navi_sever节点，就不用自己再实现这个具体的导航功能了，wp_navi_sever节点的导航坐标点来自wp_manager节点，
wp_manager节点的航点来自上节设置的点  ，加载waypoints.xml节点。在launch里启动他们
<img width="1070" height="577" alt="2026-09-29 21-33-40屏幕截图" src="https://github.com/user-attachments/assets/d063e4e7-99e0-4374-aa88-f6780dbd42be" />
wp_navi_sever节点订阅目标航点名称，发布导航执行的结果，所以只用通过demo_map_tool发布目标航点名称，订阅导航执行的结果
roslaunch wpr_simulation wpb_stage_robocup.launch 
roslaunch nav_pkg nav.launch
rosrun wpr_simulation demo_map_tool  
# （四点自主巡航）67，ROS 航点导航功能的 C++ 实现
<img width="1024" height="619" alt="2026-09-30 09-42-02屏幕截图" src="https://github.com/user-attachments/assets/310f7e20-9c27-4c7f-94aa-52080b72e865" />
roslaunch wpr_simulation wpb_stage_robocup.launch
roslaunch nav_pkg nav.launch
rosrun nav_pkg wp_node 
catkin_make
rosrun nav_pkg wp_node 
69.
<img width="1020" height="496" alt="2026-09-30 13-42-32屏幕截图" src="https://github.com/user-attachments/assets/b91b361e-aa63-4e0a-b91a-e4dc2851e94c" />
70.ROS 相机图像实时获取的 C++ 实现
catkin_create_pkg cv_pkg roscpp cv_bridge
roslaunch wpr_simulation wpb_balls.launch
rosrun cv_pkg cv_image_node
rosrun wpr_simulation ball_random_move 
72.ROS 颜色目标跟随的 C++ 实现(加入ros跟踪）

# 20261002仿真+实物
## 仿真
ros launch p3dx_gazebo p3dx_gazebo.launch
没有动
rosrun teleop_twist_keyboard teleop_twist_keyboard.py /cmd_vel:=/RosAria/cmd_vel
j左，l右
rosrun tf view_frame
没有odom到base_link,
# 因为环境里放置了多个source,加载错了环境
# 我当前项目的tf树中为什么没有odom到base_link
rosrun rviz rviz1. /cmd_vel：我要怎么运动。

2. 差速控制器：把“怎么运动”转换成左右轮速度。

3. /odom：机器人根据运动反馈估计自己走到了哪里。

4. TF：告诉 ROS 各个坐标系之间的空间关系。

5. odom → base_link：移动机器人最重要的基础 TF 之一。
保存世界
0. 保存你的世界:Gazebo 菜单 File → Save World As,存到:
~/p3dx_learning_ws/src/p3dx/p3dx_gazebo/worlds/my_arena.world
(没有 worlds 目录就先建一个)。以后启动仿真都加载这个世界,障碍物就固定下来了。
建图
roslaunch p3dx_gazebo p3dx_slam.launch
rosrun teleop_twist_keyboard teleop_twist_keyboard.py cmd_vel:=/RosAria/cmd_vel
- 最后回环:开回起点附近绕一下,让算法修正累计误差
- 地图出现"重影"(同一堵墙两个影子)就是转太快或粒子不够的表现
保存地图(地图满意后):
mkdir -p ~/p3dx_learning_ws/src/p3dx/p3dx_gazebo/maps
cd ~/p3dx_learning_ws/src/p3dx/p3dx_gazebo/maps
rosrun map_server map_saver -f my_arena

roslaunch p3dx_gazebo p3dx_nav.launch
rviz
RViz Add:Map(话题 /move_base/global_costmap/costmap)、Map(话题 /move_base/local_costmap/costmap)、TF、LaserScan。

## 实物
roslaunch p3dx_gazebo p3dx_real_nav.launch
rviz

20261003
# 视觉slam14讲
ch1
其它传感器定位导航依赖外界，另一类不依赖
单双深
ch2
<img width="522" height="199" alt="image" src="https://github.com/user-attachments/assets/500440d1-a481-4f2f-a174-45e688a77f92" />
视觉里程计(Visual Odometry,VO): 视觉里程计的任务是估算相邻图像间相机的运动,以及局部地图的样子.VO又称为前端(Front End).
把相邻时刻的运动串起来，就构成了机器人的运动轨迹，以解决定位问题
根据每个时刻相机位置，计算出各像素对应的空间点位置，得到地图
视觉里程计不可避免地会出现累积漂移(Accumulating Drift)问题，导致建图倾斜，需要回环检测和后端优化

后端优化 (Optimization): 后端接受不同时刻视觉里程计测量的相机位姿,以及回环检测的信息,对它们进行优化,得到全局一致的轨迹和地图.由于接在VO之后,又称为后端(Back End).
在视觉 SLAM中,前端和计算机视觉研究领域更为相关,比如图像的特征提取与匹配等,后端则主要是滤波与非线性优化算法.

回环检测 (Loop Closing): 回环检测通过图片之间的相似性判断机器人是否到达过先前的位置.如果检测到回环,它会把信息提供给后端进行处理
定位时是稀疏地图，导航时是稠密地图
<img width="809" height="848" alt="2026-10-03 11-43-41屏幕截图" src="https://github.com/user-attachments/assets/1da390f9-3848-4169-b55b-088792be132d" />

整理命令

ch3
<img width="901" height="322" alt="2026-10-03 16-53-07屏幕截图" src="https://github.com/user-attachments/assets/96d22e89-efe3-49f1-9a69-d4e9fd249fef" />
旋转变换，欧式变换
旋转矩阵与变换矩阵
<img width="1186" height="712" alt="2026-10-03 19-05-09屏幕截图" src="https://github.com/user-attachments/assets/3d637ce4-d9b9-4417-9641-19e9aefb91dd" />

欧拉角不适合增量式调整（因为是矩阵变换 ），绕定轴不会出现万象锁，绕动轴会出现万象🔓
四元数真难理解
左世右体，绕世界系旋转用左乘，绕本体轴旋转用右乘

实操
Eigen是纯用文件搭建的库

考研时的，爽了
第三讲视频讲解还没看完，四元数视频还没看

<img width="1920" height="2160" alt="2026-10-03 14-43-54 的屏幕截图" src="https://github.com/user-attachments/assets/3dd8212c-17ae-49de-8f0c-44f64bcc7f32" />

