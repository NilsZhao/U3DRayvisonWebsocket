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



### 多点触控

Unity在Windows平台默认不支持触摸，若要在PC端获取触控输入数据，只能在Windows触屏电脑上运行，且需要安装InputManager插件，开启模拟触摸输入。且Unity是闭源引擎，Input类不接受数据的注入，是只读的，暂时无法在Unity端和我们的系统通讯来模拟触控输入。

后续我们将在节点机系统上作处理，来模拟触发Windows触摸，Unity再模拟触控输入。这一步目前还在开发中。

现在我们还提供了一个临时方案，在OnMessage方法中可以接收触屏点位数据，然后手动解析处理。

#### 消息格式示例：

>TouchIndex表示序号，从0开始
>TouchType表示状态，0是按下，1是移动，2是抬起
>PosX PosY表示屏幕位置坐标，左上角为0,0

```json
RayInput_Touch:{"TouchType":1,"TouchIndex":0,"PosX":20,"PosY":30,"Force":1}
```



#### 解析示例：

```C#
  public void DealWithMutiTouchMsg(string msg)
    {
        if (msg.Contains("RayInput_Touch"))
        {
            Debug.Log("消息内容" + msg);
            string msgJson = msg.Replace("RayInput_Touch:", "");
            var jdata = JsonMapper.ToObject(msgJson);
            Debug.Log("位置：X: " + jdata["PosX"].ToString() + ", Y: " + jdata["PosY"].ToString());
            Debug.Log("TouchType: " + jdata["TouchType"].ToString() + ", TouchIndex: " + jdata["TouchIndex"].ToString());

            RayVisoinTouch rayVisionTouch = new RayVisoinTouch();
            rayVisionTouch.TouchType = int.Parse(jdata["TouchType"].ToString());
            rayVisionTouch.TouchIndex = int.Parse(jdata["TouchIndex"].ToString());
            rayVisionTouch.PosX = double.Parse(jdata["PosX"].ToString());
            rayVisionTouch.PosY = double.Parse(jdata["PosY"].ToString());
            rayVisionTouch.Force = double.Parse(jdata["Force"].ToString());
            if (rayVisionTouch.TouchIndex >= rayVisionTouchs.Length)
            {
                Debug.Log("超过最大点触识别数");
                return;
            }
            rayVisionTouchs[rayVisionTouch.TouchIndex] = rayVisionTouch;
        }
    }
}
```

完整脚本可以参考MultiTouchDemo.cs



