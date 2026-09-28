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
