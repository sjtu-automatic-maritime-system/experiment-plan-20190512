# experiment-plan-20190512
2019年5月12日星期日思源湖实验计划

# 摘要

1.
2.

# 预期目标和标准

# 预期目标和标准

## 目标
实现三体船单独避障, 实现彭正皓和简心语的合并, 若有时间再测试跟踪和避障合并.

标准：

* [ ] 单独避障, 采用GPS
* [ ] 彭正皓, 简心语接口测试
* [ ] 单独跟踪, 采用Lidar
* [ ] 单独避障, 采用Lidar
* [ ] 跟踪和避障, 采用GPS
* [ ] 跟踪和避障, 采用Lidar

# 实验流程

## Setup

结束指标：

* [ ] GPS, AHRS, WiFi稳定
* [ ] 电机正常工作
* [ ] 遥控正常工作

## 试验一：手工目标追踪

### 内容（操作步骤）：
采用另一艘三体船/帆船作为跟踪目标, 使用Lidar感知, 手动切换识别到的目标, 实现跟踪
### 指标：
* [ ] 稳定识别目标
* [ ] 稳定跟踪
### 应急预案：
1. 若GPS信号丢失
2. 若Ahrs信号丢失
3. 若WiFi信号丢失
4. 若电量耗尽

## 试验二: GPS下避障
### 内容(操作步骤）：
采用另一艘三体船作为移动障碍物, 使用GPS反馈障碍物坐标, 实现避障. 确定dynamic window config()各种参数
### 指标:
* [ ] crossing from left
* [ ] crossing from right
* [ ] overtake
* [ ] head-on
### 应急预案

## 试验三: 手工避障
### 内容(操作步骤）：
采用另一艘三体船作为移动障碍物, 使用Lidar感知, 手动切换识别到的目标, 实现避碰
### 指标:
* [ ] 稳定识别目标
* [ ] crossing from left
* [ ] crossing from right
* [ ] overtake
* [ ] head-on
### 应急预案

## 试验四: GPS跟踪+避障
### 内容(操作步骤）：
采用帆船作为跟踪目标, 另一艘三体船作为移动障碍物, 使用GPS反馈坐标, 实现跟踪和避碰的切换
### 指标:
* [ ] 跟踪和避碰的切换
* [ ] 避碰时不丢失目标
### 应急预案

## 试验五: 手工跟踪+避障
### 内容(操作步骤）：
采用帆船作为跟踪目标, 另一艘三体船作为移动障碍物, 使用Lidar感知, 手动切换跟踪目标和避碰目标, 实现跟踪和避碰
### 指标:
* [ ] 跟踪和避碰目标的切换
* [ ] 稳定跟踪和避碰
### 应急预案








# 附录一：三体船使用流程

1. 船载平台开机: 
   - 接好两船的电源. 
   - 将箱子外部开关朝向自己, 打开箱子内部右边三个开关, 再打开外部开关即可上电. 
   - 当听到悦耳的声音时表明主板开机成功.
2. 布置WIFI天线: 
   - 天线采用POE供电，一个网口接岸基台式机，一个网口接天线，
   - 如接多个电脑，则加一个交换机。接好后ping `192.168.1.19`
3. 检查是否连上船载电脑WiFi:
   岸基台式机上ping `192.168.1.150`，若不通，则ping `192.168.1.21`，说明路由器能接通，路由器和主板的通讯有问题
4. 布置GPS基站:
   - 在WiFi架子上放置好GPS天线, 连接至接收机
   - 给接收机供电, 并将其COM2接到岸基电脑，运行TLG001/Landed Program/GPS_BS_Configuration 读约一分钟的位置，取均值，认为是精确值，运行完自动停掉
   - 打开GPS_Tools, 选串口，打开。 input基站读到的位置数据，output是把差分信号往船上发
5. 运行船载平台程序:
   - putty连船上主板，第一艘名称是usv150, 对应GPS名称是gps100, 同理第二艘usv152&gps102，账号密码均为sjtu。 
   - `sudo python3 文件名 &`, 运行AHRS.py, GNSS.py, motor_tlg.py, voltage.py. 注: &是放到后台运行, 多开几个putty终端则无需在后台运行
   - 如何修改船载程序? 
     - 岸基电脑的桌面TLG001/002文件夹，直接在sublime修改相应文件, 保存，右键SFTP/Upload file，密码sjtu。 
   - 如何杀死后台程序?
     - "ps aux|grep py"展示所有后台程序 
     - "sudo kill -9 "编号""
6. 运行岸基电脑端程序:
   - 连上遥控杆
   - 打开landed_porgram/joystick_pub.py: 右键-打开方式-python运行
7. 检查船载GPS情况: 
   - putty登陆gps100 or gps102
   - 终端输入`log bestposa ontime 2`, 表示每两秒显示一次GPS数据. 其中single表示没有差分， narrowfolat/narrowint表示有差分
8. 关机sudo poweroff
9. 第二艘船USB口损坏了一个. 若插hub, 则需要改端口名字
   cd dev
   cd serial/by-id
   ls
   并在相应船载文件修改URL 
   
# 附录二：Appendix

1. 接线图：![circuit.PNG](https://i.loli.net/2019/05/03/5ccb19a6acf69.png)
2. 数据流：![flow.PNG](https://i.loli.net/2019/05/03/5ccb19fb394b9.png)
