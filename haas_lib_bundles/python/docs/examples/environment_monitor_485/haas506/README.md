# 环境监测及远程控制
&emsp;&emsp;
本案例开发需要经过4步。
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-导览.png width=90%/>
</div>

## 简介

&emsp;&emsp;
实时监测温湿度、光照等环境数据，并根据这些环境数据远程控制现场的温湿度和光照环境，是农业大棚、智慧养殖、数字工厂等领域最常见的需求之一。本案例将基于HaaS506 DTU开发板自带的Modbus接口控制“建大仁科光照温湿度变送器”和“郎汉德6路继电器”， 并结合阿里云物联网平台和阿里云IoT Studio组态软件搭建云端一体应用。


&emsp;&emsp;
在本案例中,手机小程序可以远程查看现场的实时温度、适度、亮度，并远程控制继电器的6路开关量，从而实现对风扇、加湿器、照明设施的控制。


<br>

### 背景知识

&emsp;&emsp;
建大仁科光照温湿度变送器支持Modbus-RTU协议，是工农业场景普选用最多的传感器之一。
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-htd485.png width=80%/>
</div>
<br>

&emsp;&emsp;
郎汉德6路继电器支持Modbus-RTU协议，是工农业场景普选用最多的传感器之一。
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-zzio606介绍.png width=80%/>
</div>
<br>

## 准备

**1. 硬件器材**

<div align="center">

|配件名称|数量|购买链接|
|-----|:---:|----|
|HaaS506开发|1|[点我购买](https://item.taobao.com/item.htm?spm=a230r.1.14.1.712a1a7bHThfbt&id=651798670356&ns=1&abbucket=18#detail)|
|建大仁科光照温湿度变送器|1|[点我购买](https://detail.tmall.com/item.htm?spm=a230r.1.14.16.11095768YuP4H7&id=541287779267&ns=1&abbucket=18&skuId=3250003424276)|
|ZZ-IO606 6路继电器|1|[点我购买](https://detail.tmall.com/item.htm?spm=a230r.1.14.6.193526cdAGmC9m&id=566327359493&ns=1&abbucket=18&skuId=4243099105859)|
|12V～24V DC电源|1||
|杜邦线|若干||
|风扇、电灯等被控设备|实验场景不建议配置|高压设备，非专业人士慎用|

</div>
<br>

**2. 硬件连线图**
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-连线图.png width=80%/>
</div>
<br>


## 物联网平台开发

物联网平台开发需要依次完成以下5步
1. 开通物联网平台实例
2. 在实例中创建云端产品
3. 为产品配置物模型
4. 为产品创建云端设备
5. 基于IoT Studio开发移动应用

### 开通物联网平台实例
&emsp;&emsp;
登陆[物联网平台](https://iot.console.aliyun.com/lk/summary/new)。
第一次使用物联网平台时候，首先需要在物联网平台创建一个实例。本案例选择使用免费的公共实例进行开发。如果您需要保证更多设备同时上云，需要购买企业实例。物联网平台创建公共实例的方式如下图所示， 左上角选择“华东2-上海”，点击“公共实例”，即可开通。
<div align="center">
<img src=./../../../images/5_3_开通公共实例.png
 width=100%/>
</div>

<br>

### 在实例中创建云端产品
&emsp;&emsp;
点击上一小节创建的公共实例就可以进入实例管理页面，在页面的左侧菜单中选中“设备管理” -> “产品”菜单项开始创建物联网产品。具体创建过程如下图所示：


**1. 点击创建产品按钮**
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-创建产品1.png
 width=100%/>
</div>

&emsp;&emsp;

**2. 填写产品基础信息以后点击“确认”按钮**

<div align="center">
<img src=./../../../images/485环境监测/485环境监测-创建产品2.png
 width=60%/>
</div>

&emsp;&emsp;

**3. 产品创建成功**
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-创建产品3.png
 width=100%/>
</div>

<br>

### 为产品配置物模型

&emsp;&emsp;
点击“设备管理” -> "产品"菜单进入产品列表页，双击刚才创建的“环境数据采集及远程控制”产品 并 点击 “功能定义” 菜单，开始定义产品功能。


**1. 功能定义页面点击“编辑草稿”**
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-定义产品功能1.png width=90%>
</div>



**2. 点击“快速导入”**

<div align="center">
<img src=./../../../images/485环境监测/485环境监测-定义产品功能2.png width=60%>
</div>


**3. 点击“上传物模型”**

<div align="center">
<img src=./../../../images/485环境监测/485环境监测-定义产品功能3.png width=60%>
</div>


**3. 物模型成功以后，效果如下图，点击“发布上线”**

<div align="center">
<img src=./../../../images/485环境监测/485环境监测-定义产品功能4.png width=100%>
</div>

<br>

### 为产品创建云端设备


**1. 在产品列表页面中，点击”环境数据采集及远程控制“对应的“管理设备”按钮，进入设备管理页面。**

<div align="center">
<img src=./../../../images/485环境监测/485环境监测-创建云端设备1.png
 width=100%/>
</div>


**2. 点击“添加设备”按钮**
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-创建云端设备2.png width=80%/>
</div>


**3. 弹框中不填写任何信息，直接点击“确认”完成设备添加**
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-创建云端设备3.png width=50%/>
</div>


**4. 创建完云端设备以后，点击“设备管理”-> “设备” 菜单可以看到刚才创建的设备， 点击设备对应的“查看”按钮进入设备详情页面。 在详情页点击“查看” 按钮获取设备三元组。 设备三元组信息需要填写到设备端代码中。**

<div align="center">
<img src=./../../../images/485环境监测/485环境监测-创建云端设备4.png width=90%/>
</div>
<br>

### 基于IoT Studio开发移动应用


**1. 新建一个空白项目**

&emsp;&emsp;
打开[IoT Studio官网](https://studio.iot.aliyun.com/)，点击屏幕左侧“项目管理”菜单 -> 点击“新建项目”按钮 -> 点击“创建空白项目“，项目名称填写“传感器数据采集”
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-创建IoTStudio项目.png width=90%/>
</div>

<br>

**2. 关联物联网产品和物联网设备”**

&emsp;&emsp;
点击“产品”旁边的“关联”按钮，然后选中前一章节创建的物联网产品完成关联。

&emsp;&emsp;
点击“设备”旁边的“关联”按钮，然后选中前一章节创建的物联网设备完成关联。

<div align="center">
<img src=./../../../images/485环境监测/485环境监测-关联产品和设备.png width=90%/>
</div>

**3. 新建“移动应用”**

&emsp;&emsp;
点击“移动应用 ”按钮 ->点击“新建“按钮， 开始创建项目。 
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-创建移动应用.png width=90%/>
</div>
&emsp;&emsp;
创建完毕以后自动跳转到应用UI可视化搭建页面。


**4. 可视化搭建”**
本届中用到了”实时曲线“组件和“开关”组件。需要从左侧组件列表中依次拖动道UI面板对应位置
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-可视化搭建.png width=90%/>
</div>

为“事实曲线组件”配置数据源
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-设定实时曲线.png width=90%/>
</div>

为“开关组件”配置数据源，配置方式参考“实时曲线组件”的数据源配置方法

<br>


## 设备端开发

### 设备端开发流程
    1. 搭建开发环境
    2. 创建HaaS Studio工程
    3. 拷贝案例代码
    4. 填写三元组信息
    5. 部署案例代码到客户端
    6. 配置设备波特率（第一次使用时候，需要将两台485设备配置到相同的波特率）
    7. 设定“郎汉德6路继电器”地址
    8. 重启设备，自动连云，云端看结果


### 搭建开发环境
&emsp;&emsp;
参考[HaaS506开发环境](../../../startup/HaaS506_startup.md)说明文档搭建软件开发环境。

&emsp;&emsp;
参考本文章开始处的“硬件连接图”连接各硬件模块，硬件连接完毕效果如下图所示
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-实物接线效果.png width=80%/>
</div>

<br>

### 创建HaaS Studio工程

&emsp;&emsp;
如下图所示，打开HaaS Studio之后在新建一个基于helloworld的python工程，设定好工程名称（“environment_monitor”）及工作区路径之后，硬件类型选择HaaS506，点击立即创建，创建一个Python轻应用的解决方案。
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-创建工程.png width=60%/>
</div>


### 拷贝案例代码

&emsp;&emsp;
将本案例代码复制到“环境监测”工程根目录下并覆盖工程原来的同名文件。 代码详细逻辑可以参考代码中的注释。
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-代码工程.png width=100%/>
</div>

<br>

### 填写三元组信息
&emsp;&emsp;
根据创建云端设备章节中获取到的设备三元组信息 修改main.py中 "productKey" "deviceName" "deviceName"三个变量。 然后点击部署运行按钮并查看运行结果。

<br>

### 部署运行
&emsp;&emsp;
点击IDE左下角的“部署运行”按钮，部署应用到haas506开发板。

<br>

### 配置波特率
&emsp;&emsp;
因为“建大仁科光照温湿度变送器”和“郎汉德6路继电器”两个传感器公用一条rs485总线，所以，两个传感器需要设定为相同的工作波特率。“建大仁科光照温湿度变送器”默认工作波特率为4800， “郎汉德6路继电器”默认工作波特率为9600. 所以，本案例将“郎汉德6路继电器”默认工作波特率设定为4800，具体设置方法如下：
系统重启默认进入repl模式，在repl模式依次输入一下python脚本
```
系统重启默认进入repl模式，在repl模式依次输入一下python脚本

>> import modbus as mb
>> mb.init("modbus_485_9600")
>> mb.writeHoldingRegister(1, 0x03e8,2,200)  
```

<br>

### 设定“郎汉德6路继电器”地址
&emsp;&emsp;
因为“建大仁科光照温湿度变送器”和“郎汉德6路继电器”两个传感器公用一条rs485总线，所以，两个传感器需需要设定不一样的modbus地址。“建大仁科光照温湿度变送器”和“郎汉德6路继电器”两个传感器的默认地址都是1. 所以，本案例将“郎汉德6路继电器”默认工作波特率设定为2，具体需要按照下图所示设定“郎汉德6路继电器”物理地址开关

<div align="center">
<img src=./../../../images/485环境监测/485环境监测-设定zzio606地址.png width=50%/>
</div>


<br>

### 重启设备，自动连云
&emsp;&emsp;
重启设备，设备自动连云

<br>

### 手机扫码实时显示传感器数据。远程控制继电器开关。

&emsp;&emsp;
再次进入iot studio 移动应用界面，点击"预览"
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-预览.png width=90%/>
</div>

&emsp;&emsp;
实时显示环境数据，远程控制继电器开关，没路继电器开闭状态都有指示灯标识，开路状态亮红灯，闭路状态不亮灯
<div align="center">
<img src=./../../../images/485环境监测/485环境监测-运行效果.png width=40%/>
</div>

<br>

### 代码

代码流程：
1. 链接网络，网络链接成功以后，haas506开发版 网络状态led灯会点亮
2. 初始化modbus总线
3. 链接物联网平台，并注册平台消息监听器， 连接成功以后会打印“物联网平台连接成功”
4. 周期上报温度、湿度、光照数据


main.py
```
#!/usr/bin/env python
# -*- encoding: utf-8 -*-

import utime
import ujson
from aliyunIoT import Device
import modbus as mb
import yuanda_htb485
import zzio606
import network
from driver import GPIO

# wifi连网代码
def connect_wifi(ssid, pwd):
    wlan = network.WLAN(network.STA_IF)
    wlan.active(True)
    wlan.connect(ssid, pwd)

# 4G.Cat1连网代码
g_connect_status = False
def on_4g_cb(args):
    print("4g callback.")
    global g_connect_status
    pdp = args[0]
    netwk_sta = args[1]
    if netwk_sta == 1:
        g_connect_status = True
    else:
        g_connect_status = False

def connect_4g_network():
    global on_4g_cb
    net = network.NetWorkClient()
    net_led = GPIO()
    net_led.open('net_led')
    
    g_register_network = False
    if net._stagecode is not None and net._stagecode == 3 and net._subcode == 1:
        g_register_network = True
    else:
        g_register_network = False
        
    if g_register_network:
        net.on(1,on_4g_cb)
        net.connect({"apn":"CMNET"})
    else:
        print('network register failed')
        
    while True:
        if g_connect_status==False:
            net_led.write(0)
        if g_connect_status:
            print('network register successed')
            #连上网之后，点亮cloud_led
            net_led.write(1)
            break
        utime.sleep_ms(20)
    return 0

# 物联网平台全局变量
productKey = " "
deviceName = " "
deviceName = " "
device = None
zzio606Obj = None
switchList = ["switch0", "switch1", "switch2", "switch3", "switch4", "switch5"]

# 物联网平台连接成功的回调函数
def on_connect(data):
    print("物联网平台连接成功")

# 处理物联网平台下行指令
def on_props(request):
    global alarm_on, device, zzio606Obj, switchList
    
    payload = ujson.loads(request['params'])    
    for i in range(0, len(switchList)):
        if switchList[i] in payload.keys():
            print(switchList[i])
            value = payload[switchList[i]]
            if (value):
                zzio606Obj.openChannel(i)
            else:
                zzio606Obj.closeChannel(i)
                
            prop = ujson.dumps({
                switchList[i]: value,
            })
            print(prop)
            upload_data = {'params': prop}
            device.postProps(upload_data)

# 连接物联网平台
def connect_lk(productKey, deviceName, deviceSecret):
    global device
    key_info = {
        'region': 'cn-shanghai',
        'productKey': productKey,
        'deviceName': deviceName,
        'deviceSecret': deviceSecret,
        'keepaliveSec': 60
    }
    device = Device()
    device.on(Device.ON_CONNECT, on_connect)
    device.on(Device.ON_PROPS, on_props)
    device.connect(key_info)

# 上传htb数据
def upload_htb_data():
    global device, zzio606Obj

    htb485Obj = yuanda_htb485.HTB485(mb, 1)
    zzio606Obj = zzio606.ZZIO606(mb, 3)

    while True:
        if htb485Obj is None:
            break      
        htb = htb485Obj.getHTB()
        # "humidity"/"temperature"/"brightness"必须和物联网平台的属性一致
        upload_data = {'params': ujson.dumps({
            'humidity': htb[0],
            'temperature': htb[1],
            "brightness":htb[3]
        })
        }
        # 上传温度和湿度信息到物联网平台
        device.postProps(upload_data)
        utime.sleep(2)
        
    mb.deinit()

if __name__ == '__main__':
    # 链接wifi
    # connect_wifi('xxx', 'xxxxx')
    # 链接4g网络
    connect_4g_network()
    # 初始化modbus总线
    mb.init('modbus_485_4800')
    utime.sleep(2)
    # 链接物联网平台
    connect_lk(productKey, deviceName, deviceSecret)
    utime.sleep(2)
    # 定时上报传感器数据
    upload_htb_data()

```
board.json
```
{
  "name": "haas506",
  "version": "1.0.0",
  "io": {
      "modbus_485_4800": {
          "type": "MODBUS",
          "mode": 0,
          "port": 2,
          "baudrate": 4800,
          "priority": 0,
          "timeout": 200
      },
      "modbus_485_9600": {
        "type": "MODBUS",
        "mode": 0,
        "port": 2,
        "baudrate": 9600,
        "priority": 0,
        "timeout": 200
      },
      "cloud_led": {
        "type": "GPIO",
        "port": 9,
        "dir": "output",
        "pull": "pulldown"
      },
      "net_led": {
        "type": "GPIO",
        "port": 7,
        "dir": "output",
        "pull": "pulldown"
      }
  },
  "debugLevel": "ERROR",
  "repl": "disable"
}

```
yuanda_htb485.py
```
import ustruct

class HTB485(object):
    
    def __init__(self, mbObj, devAddr):
        self.mbObj = mbObj
        self.devAddr = devAddr
    
    def getHumidity(self):
        if self.mbObj is None:
            raise ValueError("invalid modbus object.")
        value = bytearray(4)
        ret = self.mbObj.readHoldingRegisters(self.devAddr, 0, 2, value, 200)
        if ret[0] < 0:
            raise ValueError("readHoldingRegisters failed. errno:", ret[0])
        
        humidity = ustruct.unpack('hh',value)
        return humidity[0]
    
    def getTemperature(self):
        if self.mbObj is None:
            raise ValueError("invalid modbus object.")
        value = bytearray(4)
        ret = self.mbObj.readHoldingRegisters(self.devAddr, 0, 2, value, 200)
        if ret[0] < 0:
            raise ValueError("readHoldingRegisters failed. errno:", ret[0])
        temperature = ustruct.unpack('hh',value)
        return temperature[1]
    
    def getBrightness(self):
        if self.mbObj is None:
            raise ValueError("invalid modbus object.")
        value = bytearray(4)
        ret = self.mbObj.readHoldingRegisters(self.devAddr, 2, 2, value, 200)
        if ret[0] < 0:
            raise ValueError("readHoldingRegisters failed. errno:", ret[0])
        brightness = ustruct.unpack('hh',value)
        return brightness[1]
    
    def getHTB(self):
        if self.mbObj is None:
            raise ValueError("invalid modbus object.")
        value = bytearray(10)
        ret = self.mbObj.readHoldingRegisters(self.devAddr, 0, 5, value, 200)
        if ret[0] < 0:
            raise ValueError("readHoldingRegisters failed. errno:", ret[0])
        htb = ustruct.unpack('hhhhh',value)
        return htb
```
zzio606.py
```
class ZZIO606(object):
    
    def __init__(self, mbObj, devAddr):
        self.mbObj = mbObj
        self.devAddr = devAddr

    def openChannel(self, chid):
        if self.mbObj is None:
            raise ValueError("invalid modbus object.")
        ret = self.mbObj.writeCoil(self.devAddr, chid, 0xff00, 200)
        return ret[0]

    def closeChannel(self, chid):
        if self.mbObj is None:
            raise ValueError("invalid modbus object.")
        ret = self.mbObj.writeCoil(self.devAddr, chid, 0, 200)
        return ret[0]
    
    def getChannelStatus(self):
        if self.mbObj is None:
            raise ValueError("invalid modbus object.")
        status = bytearray(1)
        ret = self.mbObj.readCoils(self.devAddr, 0, 6, status, 200)
        if ret[0] < 0:
            raise ValueError("modbus readCoils failed, errno:", ret[0])
        return status
```
