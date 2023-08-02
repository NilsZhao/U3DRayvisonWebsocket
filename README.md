# 通信插件

#### 适用版本

适用于Unity2018.4.0及以上版本

#### 使用说明

1. 下载RayvisionSocket.unitypackage，并导入项目中
2. 将预设拖入Hierarchy中，预设路径：Assets-->RayvisonWebsocket-->Prafab-->Rayvison_WSMgr

#### Unity版本问题

Unity2020之后的版本默认包含了Newtonsoft插件，如果报错，删除 RayvisonWebsocket内的 JsonNet 文件夹即可

#### 调用说明

利用RayvisonWebsocket和服务端交互非常简单，只需要将Rayvison_WSMgr预设拖放到项目的第一个场景即可。

Rayvison_WSMgr预设上挂挂载了WSMgr脚本，这是一个不被销毁的单例实例，您可以在任意需要的地方通过[WSMgr.Instance]进行调用

#### 消息收取示例

```C#
        //订阅事件
        WSMgr.Instance.OnBinaryMsgRecv += DealWithBinaryMsg;
        WSMgr.Instance.OnStringMsgRevice += DealWithStringMsg; 
```


```C#
        //取消订阅事件
        WSMgr.Instance.OnBinaryMsgRecv -= DealWithBinaryMsg;
        WSMgr.Instance.OnStringMsgRevice += DealWithStringMsg;
```

```C#
        //具体逻辑中处理二进制消息的方法（传入参数为已转换好的结果）
        public void DealWithBinaryMsg(string msg)
        { 
            Debug.Log(msg);
        }
        //具体逻辑中处理字符串消息的方法（传入参数为已转换好的结果）
        public void DealWithStringMsg(string msg)
        {
            Debug.Log(msg);
        }
```

#### 消息发送示例
```C#
        //调用方法，向服务端发送字符串消息
        public void SendStrMsgToServer(string msg)
        {
            WSMgr.Instance.SendString(msg);
        }

        //调用方法，向服务端发送二进制消息
        public void SendBinaryToServer(string msg)
        {
            WSMgr.Instance.SendBytes(msg);
        }
```

完整脚本可以参考RCRDemo.cs



------



# 多点触控注意事项



> 注意事项：云渲染支持应用的多点触控功能，无需接入任何SDK，但是需要注意，Unity的新版输入系统不能识别Windows平台的触摸事件，因此无法响应节点机发出的触控指令，目前只能等待Unity进行修复。如果项目中使用了该插件，需要切换为旧版本.

 

#### 确认项目中是否使用了新版插件

1. 打开工程，在Window--Package Manager中，查看项目中是否有Input System插件         

2. 打开场景，选中Event System，查看是否带有Input System UI Input Module 

#### 替换为旧版本输入系统

1. 移除Event System上的Input System UI Input Module组件

2. 添加Standalone Input Module组件 

#### 确认激活旧版输入系统

在项目设置中，将Active Input Handling设置为Input Manager(Old) 或者Both



------



# 打包方式

#### 如果遇到闪退情况：

> 使用某些插件时，容易有闪退的情况出现，如视频播放类插件、摄像头相关插件等。这是Mono自身的运行环境未考虑到云渲染时的挂载磁盘运行方式，导致插件在Mono环境中运行时报错并闪退。
>
> 强烈建议使用IL2CPP打包，解决这个问题的同时，性能方面也更加友好。

使用IL2CPP打包：https://docs.unity3d.com/cn/2021.1/Manual/IL2CPP-BuildingProject.html
