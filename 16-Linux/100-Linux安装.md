# Linux安装

## 1、VMWare安装

Linux
1、32位os最大利用内存是3.1G
2、centos 6 最小安装内存占用628M
3、网络
3.1、桥接:用于和本地真实网卡进行连接并通信的
3.2、NAT：用于和虚拟网卡VMnet8进行连接并通信的
3.3、仅主机模式：用于和虚拟网呀VMnet1进行连接并通信的

![image-20220325090656940](C:/Users/zhangyanqing/AppData/Roaming/Typora/typora-user-images/image-20220325090656940.png)

![image-20220325090741642](C:/Users/zhangyanqing/AppData/Roaming/Typora/typora-user-images/image-20220325090741642.png)

桥接默认是自动进行连接本地网卡的，但如果本地既有无线网卡，又有有线网卡，而无线网卡是连通可用的，有线网卡没有接网络不可用。这时如果你选择的是桥接模式，那虚拟机有可能连接上的是有线网卡，而导致虚拟机网络不可用。

![image-20220325091131756](C:/Users/zhangyanqing/AppData/Roaming/Typora/typora-user-images/image-20220325091131756.png)

解决这个问题的办法是：在菜单【编辑】-【虚拟网络编辑器】进行配置调整，如下图所示

![image-20220325091318285](C:/Users/zhangyanqing/AppData/Roaming/Typora/typora-user-images/image-20220325091318285.png)

![image-20220325091349838](C:/Users/zhangyanqing/AppData/Roaming/Typora/typora-user-images/image-20220325091349838.png)

![image-20220325091407188](C:/Users/zhangyanqing/AppData/Roaming/Typora/typora-user-images/image-20220325091407188.png)