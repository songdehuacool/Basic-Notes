## 简述 TCP 的 TIME_WAIT

### 一. 什么是TIME_WAIT
   根据TCP协议定义的3次握手断开连接规定，发起socket主动关闭的一方socket将进入TIME_WAIT状态，TIME_WAIT状态将持续2个MSL（Max Segment Lifetime）,TIME_WAIT状态下的socket不能被回收使用；
   
### 二. 为什么需要TIME_WAIT
   1. 确保旧的连接状态不会对新的连接产生影响。是TCP用于保证被重新分配的socket不会受到之前残留的延迟重发报文影响，是必要的逻辑保证；
   2. 为使旧的数据包在网络因过期而消失

### 三. TIME_WAIT有什么影响
   TIME_WAIT状态下的socket不能被回收使用，对于一个处理大量短连接的服务器，如果是由服务器主动关闭客户端的连接，将导致服务器端存在大量的处于TIME_WAIT状态的socket，甚至比处于Established状态下的socket还要多，严重影响服务器的处理能力，甚至耗尽可用的socket而停止服务器；