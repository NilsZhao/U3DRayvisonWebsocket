# U3DRayvisonWebsocket
### 适用版本

适用于Unity2018.4.0及以上版本



### 使用说明

1. 下载RayvisionSocket.unitypackage，并导入项目中
2. 将预设拖入Hierarchy中，预设路径：Assets-->RayvisonWebsocket-->Prafab-->Rayvison_WSMgr

### Unity版本问题

Unity2020之后的版本默认包含了Newtonsoft插件，如果报错，删除 RayvisonWebsocket内的 JsonNet 文件夹即可

### 调用说明

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





