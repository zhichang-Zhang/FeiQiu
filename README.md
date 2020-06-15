# feiqiu-局域网通讯工具



## 一、项目介绍

​	本项目为个人学习项目，主要目的是学习锻炼以下知识点：

  - java 的swing图形化界面
  
  - 局域网内的网络协议（同局域网内的机器发现与通信）
  
  - 网络通信与网络流传输

    

## 二、项目设计

 V1.0 版本

  1. 实现双击打开软件后, 打开可聊天对象面板;
  2. 可以实现软件登录的自动发现: 当局域网内, 有其他机器打开了本软件, 当前节点可以发现新的登录节点；
  3. 双击联系人, 可以打开聊天窗口;
  4. 可以实现点对点聊天，点对点文件传输;


## 三、项目拆解
  1. 软件打开上线后，使用udp协议局域网内广播上线消息：
       * 上线机器使用udp广播当前机器上线，需要附带该机器的ip地址；
       * 其他机器收到udp消息后，需要将该机器的IP添加到上线机器列表中，并回复该消息，表明自己也处在线上；
       * 上线机器接收到所有的回复消息，完成本机器的上线机器列表。
  2. 软件打开后，展示所有在线用户列表的面板，并开启线程维护该列表状态（上线/下线）：
        * 接收到有机器发送的上线UDP消息后，需要将该上线机器添加到本机的上线用户列表中，并回复该消息，表明自己处于线上；
        * 接收到有机器发送的下线UDP消息后，需要将该下线机器从用户列表中剔除。
  3.  双击列表中指定的用户IP，即展开与该用户的聊天框：
        * 展开的面板中，附加上要展开聊天的用户IP；
        * 与改用户的聊天消息/文件传输，使用TCP传输；
        * 关闭聊天框，销毁该对话线程；
   4. 软件关闭下线时，使用UDP协议局域网内广播下线消息：
        * 下线机器使用UDP广播当前机器下线， 需要附带该机器的IP；
        * 其他机器收到UDP消息后，需要将该机器从上线用户列表中剔除，但是并不需要回复消息。