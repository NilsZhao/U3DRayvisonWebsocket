# U3DRayvisonWebsocket
RayvisonWebsocket plugin for U3D
适用版本:2018.4.0及以上

1. 使用RayvisionSocket进行通信
RayvisionSocket会在应用启动后自动连接到3DCAT平台, 无需任何的额外配置, 并提供了RayvisionWebSocket组件获取WebSocket连接事件.
1.1 插件下载与安装
下载对应引擎版本的RayvisionSocket.unitypackage插件直接拖到项目中,import插件到项目中；
1.2 插件的使用
打开你的项目,找到预制体 Rayvison_WSMgr，将其拖到你要使用的场景中。 
 
2. 使用其他方式进行通信
平台会基于WebSocket协议将Web端消息转发至节点机的本地端口, 因此, 只需要建立本地WebSocket连接即可进行与Web端的通信.
2.1 从启动命令行中获取Socket端口
在应用启动前, 3dcat平台会自动为应用分配本机端口并通过启动参数传入U3D中, 端口参数固定为启动参数的最后一位, 在整个应用生存周期内, 这个端口不会发生变化.
 
2.2 具体的使用
在你需要接收数据的C#文件中监听两个事件：
 
如果服务器发送过来的是二进制文件使用 WSMgr.StringHandler来监听：
如果服务器发送过来的是字符串的json数据使用WSMgr.JsonHandler来监听：
 



