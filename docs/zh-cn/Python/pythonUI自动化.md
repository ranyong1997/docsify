# Python3--Uiautomator2--Pytest--Alure使用
官方源码GitHub地址：[https://github.com/openatx/uiautomator2](https://github.com/openatx/uiautomator2)
在这里，我很感谢每一个拉我一把的人，特别是毕夏毕老师，我衷心的说声谢谢你。
### 介绍
uiautomator2 是一个可以使用Python对Android设备进行UI自动化的库。其底层基于Google uiautomator，Google提供的uiautomator库可以获取屏幕上任意一个APP的任意一个控件属性，并对其进行任意操作，但有两个缺点：
1、测试脚本只能使用Java语言。
2、测试脚本必须每次被上传到设备上运行。 我们希望测试能够用一个更脚本化的语言，
例如Python编写，同时可以每次所见即所得地修改测试、运行测试。


而uiautomator2项目是对[xiaocong/uiautomator](https://github.com/xiaocong/uiautomator)的增强，主要有以下部分：
1. 设备和开发机可以脱离数据线，通过WiFi互联（基于[atx-agent](https://github.com/openatx/atx-agent)）
2. 集成了[openstf/minicap](https://github.com/openstf/minicap)加快截图速度
3. 集成了[openstf/minitouch](https://github.com/openstf/minitouch)达到精确实时控制设备
4. 修复了xiaocong/uiautomator经常性退出的问题
5. 代码进行了重构和精简，方便维护


**工作原理**
![image.png](https://cdn.nlark.com/yuque/0/2019/png/153889/1576675428969-e2cd5484-9a0e-4769-94f6-52c49d66bc15.png#height=365&id=EJZZY&name=image.png&originHeight=729&originWidth=1323&originalType=binary&size=272539&status=done&style=none&width=661.5)




如图所示，python-uiautomator2主要分为两个部分，python客户端，移动设备
python端：运行脚本，并向移动设备发送HTTP请求
移动设备 ：移动设备上运行了封装了uiautomator2的HTTP服务，解析收到的请求，并转化成uiautomator2的代码。


整个过程
1. 在移动设备上安装atx-agent(守护进程), 随后atx-agent启动uiautomator2服务(默认7912端口)进行监听
2. 在PC上编写测试脚本并执行（相当于发送HTTP请求到移动设备的server端）
3. 移动设备通过WIFI或USB接收到PC上发来的HTTP请求，执行制定的操作
## 前期准备



---

### 环境搭建
**1.安装Python**
如命令行可以执行python，则跳过此步骤
在python官网[下载python](https://www.python.org/getit/)，并配置环境变量，可[点击查看详细教程](https://jingyan.baidu.com/article/25648fc19f61829191fd00d4.html)


**2.安装adb**
如命令行可以执行adb devices，则跳过此步骤
从谷歌官网下载[Android Platform Tools ](https://developer.android.com/studio/releases/platform-tools.html)， 解压，并加包含adb.exe的目录加入到系统的PATH中。


**3.安装uiautomator2**
同时需要pillow来处理截图数据
```basic
pip install --pre uiautomator2
pip install pillow
```


**4.初始化**
部署相关的守护进程。
电脑连接上一个手机或多个手机, 确保adb已经添加到环境变量中，执行下面的命令会自动安装本库所需要的设备端程序：uiautomator-server 、atx-agent、openstf/minicap、openstf/minitouch
```basic
python -m uiautomator2 init
```


安装完成，设备上会多一个uiautomator的应用。
例如：![](https://cdn.nlark.com/yuque/0/2021/png/21384865/1618476760614-244939b2-ef41-4aed-bfe1-158f46698a2b.png#height=103&id=uaca59092&margin=%5Bobject%20Object%5D&originHeight=103&originWidth=91&originalType=binary&size=14484&status=done&style=none&width=91)


**5.确定环境是否搭建成功**
接下来看一段Python脚本，运行成功时手机会弹出一句:Hello world， 显示时间为3秒
![image.png](https://cdn.nlark.com/yuque/0/2019/png/153889/1576675771742-63c61b05-467a-4dab-aabe-811afa44519a.png#height=153&id=02Y9h&name=image.png&originHeight=305&originWidth=1158&originalType=binary&size=72009&status=done&style=none&width=579)

---

### 定位元素
我们可以借助Android SDK自的uiautomatorviewer查看元素，这就要求手机必须以USB的方式连接PC，我前面使用的是WIFI连接进行连接的。所以，openatx提供了另外一个工具weditor 来解决这个问题。


GitHub地址：[https://github.com/openatx/weditor](https://github.com/openatx/weditor)


1、安装：
```basic
pip install --pre --upgrade weditor
```


2、使用：
```basic
python3 -m weditor
```


默认会通过浏览器打开页面：[http://localhost:17310/](http://localhost:17310/)
![](https://cdn.nlark.com/yuque/0/2021/png/21384865/1618476877396-0a2fa334-bb6d-42fc-8c0e-bc4089e73e0c.png#height=1000&id=ub00c04f9&margin=%5Bobject%20Object%5D&originHeight=1000&originWidth=1917&originalType=binary&size=219845&status=done&style=none&width=1917)


在页面左上角选择Android，点击Connect按钮。
当我们操作完手机后，可以点击“Dump Hierchy”按钮进行刷新，从而保持与设备上的界面保持同步。weditor 还可以帮我们生成代码。
总之，研究一下就会用了。



---

### 安装allure
GitHub地址：[https://github.com/allure-framework/allure2/releases](https://github.com/allure-framework/allure2/releases)
![](https://cdn.nlark.com/yuque/0/2021/png/21384865/1618477512913-e88d621a-61c6-4359-b4a3-5f2ac5ed32ef.png#height=623&id=uea334dc9&margin=%5Bobject%20Object%5D&originHeight=623&originWidth=1168&originalType=binary&size=54030&status=done&style=none&width=1168)
环境配置参考如下：
allure安装：[https://cloud.tencent.com/developer/article/1556151](https://cloud.tencent.com/developer/article/1556151)

---

### 安装JavaJDK-1.8
官网地址：[https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html](https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html)


根据自己电脑配置安装对应的JDK
![](https://cdn.nlark.com/yuque/0/2021/png/21384865/1618478603521-6dfa534b-535f-47cd-91ac-6194a164c00c.png#height=151&id=u6aaf8d5d&margin=%5Bobject%20Object%5D&originHeight=151&originWidth=1486&originalType=binary&size=22023&status=done&style=none&width=1486)
环境配置：[https://www.cnblogs.com/nojacky/p/9497724.html](https://www.cnblogs.com/nojacky/p/9497724.html)

---

### 安装git
官网地址：[https://git-scm.com/](https://git-scm.com/)
一直下一步就好了

---

## 万事俱备，只欠东风

---

# 概述：
本工程是在开源 [uiautomator2](https://github.com/openatx/uiautomator2) 和 [facebook-wda](https://github.com/openatx/facebook-wda) 项目的基础之上，进行了统一的封装，消除两个项目所提供的接口的差异性，同时也将分散的接口功能进行了聚合，降低接入门槛，简化使用操作，同时也最大化保留了原项目的诸多优点，并可以进行更加深度的定制化拓展。
工程采用业内主流的pytest+allure作为用例运行的基本脚手架和测试报告生成框架，脚本用例采用经典PO模式+业务流程封装+数据分离的思想进行组织和编写。
本工程编写的初衷是为了尽可能的降低UI自动化脚本的编写和使用门槛，着重在元素定位、异常和断言处理上进行了更进一步的策略优化和封装，使得测试人员可以聚焦在元素基本操作的编写和业务流程的逻辑组织上，而无需刻意关注随机的弹窗遮挡或点击按钮无法跳转下一页面的异常toast报错处理等操作，减少流程中过多的自定义断言处理，提高脚本稳定性和复用性(多平台复用、流程复用)，降低维护成本，加快脚本编写速度。
# 初始化安装：

1. 安装以上的的前期准备
1. 找一个目录，打开git bash，输入：`git clone [https://gitee.com/ran_yong/auto_uiautomator2.git](https://gitee.com/ran_yong/auto_uiautomator2.git)`
1. 打开Pycharm，打开上一步拉取下来的UIAutoTest文件夹，导入工程，点击settings---Project---Project Interpretor---add...---New environment---Location中在当前工程名后确认是否有/venv，没有的话手动输入，基本编译器选择python3的安装路径，确认即可
1. Pycharm需要先配置pip的repositories，推荐[https://pypi.tuna.tsinghua.edu.cn/simple/](https://pypi.tuna.tsinghua.edu.cn/simple/) ，Pycharm的Terminal中，输入：
`pip3 install -r requirements.txt`
`pip3 install ./install/whl/mtn_perf-0.1.0-py3-none-any.whl ./install/whl/mtn_speed-0.1.0-py3-none-any.whl ./install/whl/facebook_wda-1.3.2.dev31-py3-none-any.whl`
等待安装完成后，环境就配置完成了，后续执行测试只要执行下面的第9步即可
1. 手机进入开发者模式，插入usb，确保adb devices可以返回设备号，直接运行对应应用目录下的run.py即可(需要在手机端确认安装一些必须的软件)



# 工程分层结构
### 分层结构:
```
Fixtures辅助层(driver、data、业务相关的前后置操作)
     |
   用例层
     |
业务流程逻辑层
     |
PageObject层(继承BasePage)
     |
   元素层(元素数据、元素集切换)
```
### 目录结构
```
WalletUiTest/
  - common/ 存放公共文件
    - base_page.py: 提供基本页面元素操作
    - config_parser.py: 提供配置文件解析方法
    - image.py: 提供图像识别方法
    - logger.py: 提供日志全局配置方法
    - yaml_parser.py: 提供yaml文件解析方法
    - utils.py: 提供工具类方法
  - install/: 存放修改后的源码或依赖库
  - demo/: 文件夹名称表示对应的应用别称(功能域)，demo只是示例名称
    - vXXX/: 以vXXX表示应用版本号以区分版本用例，每个版本一个文件夹(包含文件夹内的文件结构)
      - case/: 存放用例
      - data/: 存放测试数据(yaml文件)
      - element/: 存放定位元素(yaml文件)
        - element_router.py: 元素选择路由，需按照实际补充相关逻辑
      - flow/: 存放业务流程逻辑
      - image/: 存放需要图像识别的元素图像文件
      - page/:  存放PageObject文件
      - conftest.py: 放置与应用强关联的Fixtures操作
      - debug.py: 做步骤调试
      - run.py: 用例统一执行和生成报告
    - config.ini: 应用相关配置
  - report/: 存放与报告相关的文件
    - log: 存放本工程的log日志
    - logcat: 存放抓取的系统和应用日志
    - screenrecord: 存放用例执行过程的录屏
    - screenshot: 存放截图
    - raw_data: 存放allure原始数据
    - html_report_xxxx: allure生成的html报告，以时间分割保存
  - config.ini: 全局相关配置
  - conftest.py: 放置通用的Fixtures和Hooks方法
  - pytest.ini: pytest运行配置
  - README.md: 使用说明文档
```


详情请参考项目文件README.md
# 开始编写用例
> **基本流程**： element(image)-->page-->flow-->case-->data

## 一个简单的demo：


