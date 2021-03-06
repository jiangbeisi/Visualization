<!--- 告警事件 --->

1. 车距检测和预警：
   预警系统不断监测与前方车辆的距离，让驾驶员始终保持安全的行车距离，并能在车距存在危险时发出警报。

2. 城市碰撞警示：
   在市内交通拥挤时，可能发生低速碰撞和追尾事故，对此，预警系统会提前发出警告。

3. 前碰撞警示：
   预警系统不断监测与前方车辆的距离、方位及相对速度，当存在潜在碰撞危险时发出警告。

4. 限速警示：
   预警系统比较监测到的车速和收到的限速讯号，在超速的情况下发出警告。
  
5. 车道偏离警示：
   由于车速过快、方向盘失控或者驾驶员有意识地偏离车道（却未打转向灯）等的缘故，而导致车辆偏离了原行车道，对此，预警系统会及时发出警告。



<!--- 离散化的速度值 --->

1档：<=10km/h；     2档：11km/h - 20km/h；     3档：21km/h - 30km/h；     

4档：31km/h - 40km/h；     5档：>=41km/h



<!--- 图解 --->

1. 模型介绍：
		告警(W) --- 消息id --- 速度(V) --- 档位(速度的基因)

(1) 告警(W)和速度(V)之间是多对多的关系：一个W可能对应多个V，一个V也可能对应多个W。

(2) 由于W-V有重复现象，而重复的边在Gephi中会被认为是同一条边，边的权重增加了，粗度就增加了。问题就在这里！>>
>>> 有的边权重很小(1,2,3……)，而有的边权重很大(300,400,500……)，因此可视化出来有的边要么很粗要么很细，效果很差劲。
>>> 改进方法：借助“消息id”这个中间节点来关联告警(W)和速度(V)，即：W-消息id-V。

(3) “速度值集合”包含了所有采集到的速度值，其中D1~D5是对速度集合的分类，对应一般手动挡汽车的5个档位。
  /* 将大规模的集合映射到较小规模的集合中去，便于更快地发现事物之间的关联特性。*/


2. 图解举例：
		../Visualization/3个告警/设备号10001.png
		
(1) 图中有3个告警事件：车道偏离、限速和前碰撞，每个“告警”节点周围都环绕着许多“消息id”节点（相当于该告警事件的集合）；
    图中有5个档位：1档~5档，分别对应一般手动挡汽车的5个档位，每个“档位”节点周围都环绕着许多速度值节点（相当于该档位下的速度集合）。

(2) 1个速度节点可以与多个告警节点相连，1个告警节点也可以与多个速度节点相连，体现了告警与速度多对多的关系；
    1个速度节点可以通过中间节点（消息id）与1个告警节点重复连接，体现了某些告警与速度之间的强关联特性。
 
(3) “车道偏离”只与“5档”有连线，且位置上远离1档~4档，说明“车道偏离”易在“5档”下发生；
    “限速、前碰撞”与“1档~5档”都有连线，但与“5档”连线居多，说明“限速、前碰撞”易在“5档”下发生；
    “1档~4档”靠近 “限速、前碰撞”，远离“车道偏离”，说明“1档~4档”下不易发生“车道偏离”；
    “5档”与3个告警事件都相连，说明“5档”下容易发生告警事件；
    “5档”周围部分速度值节点被拉伸出来向着“车道偏离”，1个速度值节点关联了多个消息id，说明在该速度值下发生的“车道偏离”告警事件多，符合力导向布局。
     
