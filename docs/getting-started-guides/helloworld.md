---
layout:docwithnav
assignees:
 -  ashvayka
title:入门
description:开始使用ThingsBoard开源IoT平台和模拟物联网设备

---

---
layout: docwithnav
assignees:
- ashvayka
title: 快速入门
description: 开始使用ThingsBoard开源IoT平台和模拟物联网设备.
---

* TOC
{:TOC}


## 介绍

本指南的目标是使用ThingsBoard收集和可视化一些物联网设备数据。
本指南将让您:

  - 配置您的设备
  - 管理设备凭据
  - 使用MQTT，CoAP或HTTP协议将数据从设备推送到您的ThingsBoard实例
  - 创建仪表板以可视化数据
  
<div id ="video">
    <div id ="video_wrapper">
        <iframe src ="https://www.youtube.com/embed/dIKXFxpfB_Q" frameborder ="0" allowfullscreen> </iframe>
    </DIV>
</DIV>

## 设置和要求

如果您无法访问正在运行的ThingsBoard实例，请使用[Live Demo](https://demo.thingsboard.io/signup)或
[安装指南](/docs/user-guide/install/installation-options/)
解决这个问题。

## 模拟账号

所有ThingsBoard安装都配备了一个模拟帐户，简化了第一次用户体验。
此演示帐户包含多个预先配置的设备，仪表板和规则链。
请注意，您可以在生产部署中自由删除此帐户。

您可以使用ThingsBoard设备模拟器来模拟真实设备，并使用服务器端API，数据可视化和处理逻辑进行游戏。
   
我们将在高级教程中使用这些模拟器，但是，出于本指南的目的，我们将使用预先配置的租户管理员帐户。
 
## 以租户管理员身份登录

第一步是登录管理Web UI。

如果您使用的是本地ThingsBoard安装，则可以使用默认帐户登录管理Web UI:
 
   - 用户名:**tenant@thingsboard.org**
   - 密码:**tenant**

如果您使用的是Live Demo，则可以使用租户管理员帐户(您在注册期间创建的帐户)登录[Live Demo](https://demo.thingsboard.io/login)服务器。
  
{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/login.png)
{:refdef}

## 配置您的设备

打开"设备"面板，然后单击页面右下角的"+"按钮。

{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/devices.png)
{:refdef}

填充并保存设备名称(例如，"SN-001")。稍后会将其称为$ DEVICE_NAME。
设备名称必须是唯一的。基于唯一序列号或其他设备标识符填充设备名称通常是个好主意。
单击"添加"按钮将相应的设备卡添加到面板。

{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/add-device.png)
{:refdef}


## 管理设备凭据

单击我们在上一步中创建的"设备卡"。此操作将打开"设备详细信息"面板。

单击面板顶部的"管理凭据"按钮。此操作将打开一个包含设备凭据的弹出窗口。

{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/manage-credentials.png)
{:refdef}

设备凭据窗口将显示您可以更改的自动生成的设备访问令牌。
请保存此设备令牌。它稍后将被称为** $ ACCESS_TOKEN **。

{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/device-credentials.png)
{:refdef}


恭喜！您刚配置了第一台设备！
现在，您可以将一些数据从此设备推送到ThingsBoard进行可视化和分析。

## 从设备推送数据

为了简化本指南，我们将使用本地PC上的MQTT，CoAP或HTTP协议来推送数据。
有关各种硬件平台的高级示例，请参阅[samples](/docs/samples/)。

### 客户端库安装

使用以下命令安装首选MQTT(Mosquitto或MQTT.js)，CoAP(CoAP.js)或HTTP(cURL)客户端。

{%capture tabspec%} mqtt-client
客户端A，MQTT.js，外shell，资源/node-mqtt.sh，/文档/工具入门导游/资源/node-mqtt.sh
ClientB，Mosquitto(Ubuntu)，shell，resources/mosquitto-ubuntu.sh，/docs/getting-started-guides/resources/mosquitto-ubuntu.sh
ClientC，Mosquitto(macOS)，shell，resources/mosquitto-macos.sh,/docs/getting-started-guides/resources/mosquitto-macos.sh
ClientD，CoAP.js，外shell，资源/node-coap.sh，/文档/工具入门导游/资源/node-coap.sh
ClientE，cURL(Ubuntu)，shell，resources/curl-ubuntu.sh，/docs/getting-started-guides/resources/curl-ubuntu.sh
ClientF，cURL(macOS)，shell，resources/curl-macos.sh，/docs/getting-started-guides/resources/curl-macos.sh {%endcapture%}
{%include tabs.html%}

### 示例数据文件

**创建一些文件夹**以存储本教程的所有必要文件。
下载到此文件夹或创建以下数据文件:

  -  {%include ghlink.html content ="** attributes-data.json **"ghlink ="/docs/getting-started-guides/resources/attributes-data.json"%}  - 包含两个设备属性值:firmware版本和序列号。
  -  {%include ghlink.html content ="** telemetry-data.json **"ghlink ="/docs/getting-started-guides/resources/telemetry-data.json"%}  - 包含三个时间序列值:温度，湿度和活动标志。
 
请注意，此文件中的数据基本上是键值格式。您可以使用自己的键和值。
参见[MQTT](/docs/reference/mqtt-api/＃key-value-format)，[CoAP](/docs/reference/coap-api/＃key-value-format)
或[HTTP](/docs/reference/http-api/＃key-value-format)协议参考以获取更多详细信息。

{%capture tabspec%}数据
A，属性 -  data.json，JSON，资源/属性 -  data.json，/文档/工具入门导游/资源/属性 -  data.json
B，telemetry-data.json，json，resources/telemetry-data.json，/docs/getting-started-guides/resources/telemetry-data.json {%endcapture%}
{%include tabs.html%}

### 使用MQTT，CoAP或HTTP推送数据

根据首选客户端将以下文件下载到**以前创建的文件夹**:

  -  ** MQTT.js(MQTT) **
    -  {%include ghlink.html content ="mqtt-js.sh"ghlink ="/docs/getting-started-guides/resources/mqtt-js.sh"%}(Ubuntu＆MacOS)或{%include ghlink.html content ="mqtt-js.bat"ghlink ="/docs/getting-started-guides/resources/mqtt-js.bat"%}(Windows)
    -  {%include ghlink.html content ="publish.js"ghlink ="/docs/getting-started-guides/resources/publish.js"%}
  -  ** Mosquitto(MQTT)**
    -  {%include ghlink.html content ="mosquitto.sh"ghlink ="/docs/getting-started-guides/resources/mosquitto.sh"%}
  -  ** CoAP.js(CoAP)**
    -  {%include ghlink.html content ="coap-js.sh"ghlink ="/docs/getting-started-guides/resources/coap-js.sh"%}
  -  ** cURL(HTTP)**
    -  {%include ghlink.html content ="curl.sh"ghlink ="/docs/getting-started-guides/resources/curl.sh"%}

如果您使用的是shell脚本(* .sh)，请确保它是可执行的:

```shell
chmod + x * .sh
```

在执行脚本之前不要忘记:

  - 用**设备凭证**窗口替换** $ ACCESS_TOKEN **。
  - 将** $ THINGSBOARD_HOST **替换为** 127.0.0.1 **(如果是本地安装)或** demo.thingsboard.io **(如果是现场演示)。

最后，执行相应的* .sh或* .bat脚本将数据推送到服务器。

下面是提供脚本内容的选项卡。
 
{%capture tabspec%} mqtt-telemetry-upload
A，MQTT.js(Ubuntu和MacOS)，shell，resources/mqtt-js.sh，/docs/getting-started-guides/resources/mqtt-js.sh
B，MQTT.js(Windows)，shell，resources/mqtt-js.bat，/docs/getting-started-guides/resources/mqtt-js.bat
C，publish.js，外shell，资源/publish.js，/文档/工具入门导游/资源/publish.js
D，Mosquitto(MQTT)，shell，resources/mosquitto.sh，/docs/getting-started-guides/resources/mosquitto.sh
E，CoAP.js(CoAP)，shell，resources/coap-js.sh，/docs/getting-started-guides/resources/coap-js.sh
F，cURL(HTTP)，shell，resources/curl.sh，/docs/getting-started-guides/resources/curl.sh {%endcapture%}
{%include tabs.html%}

## 在Web UI上观察设备数据

执行上面列出的命令后，您应在相应的设备详细信息选项卡中看到[attributes](/docs/user-guide/attributes/)和最新的[遥测数据](/docs/user-guide/telemetry/)。

{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/device-attributes.png)
{:refdef}


{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/device-telemetry.png)
{:refdef}

## 创建新的仪表板以可视化数据
 
创建新仪表板的最简单方法是选择设备属性并在小部件上显示它们

{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/attributes-selected.png)
{:refdef}

单击"在窗口小部件上显示"按钮后，您将看到"窗口小部件预览"面板

  - 选择小部件包
  - 选择首选小部件
  - 将小部件添加到新的或现有的仪表板
 
{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/widget-selected.png)
{:refdef}

让我们将第一个小部件添加到名为"SN-001 Dashboard"的新仪表板中

{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/add-widget.png)
{:refdef}

让我们添加小部件来显示温度

{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/temperature-selected.png)
{:refdef}

单击**在小部件上显示**并选择**数字仪表**包。使用轮播选择温度计小部件，如下所示。

{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/termometer-widget.png)
{:refdef}

**请注意**在这种情况下，我们会将一个小部件添加到现有的仪表板中。我们还将选择"打开仪表板"选项以查看我们的工作结果。

{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/add-termometer.png)
{:refdef}

最后，我们可以看到我们的新仪表板。现在我们可以编辑仪表板了

  - 配置仪表板设置
  - 调整小部件大小和布局
  - 修改单个小部件的高级设置
  - 添加新小部件或删除现有小部件
  - 小部件导入/导出
 
{:refdef:style ="text-align:center;"}
![image](../../images/helloworld/new-dashboard.png)
{:refdef}


 
## 下一步

{%assign currentGuide ="GettingStartedGuides"%} {%include templates/guides-banner.md%}

## 您的反馈

请毫不犹豫地在** [github](https://github.com/thingsboard/thingsboard) **上对ThingsBoard进行宣传，以帮助我们传播这个词。
如果您对此示例有疑问，请将其发布在** [论坛](https://groups.google.com/forum/#!forum/thingsboard)**上。
