# EasyClick开发脚本学习
官方文档：[https://easyclick.gitee.io/docs/#/README](https://easyclick.gitee.io/docs/#/README)
解放双手，可编程式、智能化自动点击App；只要会敲键盘就能进行编程！

- 简单易编程
- 丰富的API,图像识别,免费OCR
- 可脱机运行，可单独发布
- 支持无障碍/免ROOT运行
- 智能IDE编程支持
- App爬虫
- 自动化测试App
- ...

教学视频：[https://www.bilibili.com/video/BV1vz4y1S7gd?p=7&spm_id_from=pageDriver](https://www.bilibili.com/video/BV1vz4y1S7gd?p=7&spm_id_from=pageDriver)
# 基础语法学习
```javascript
点击坐标
clickPoint(203, 1403);
文本点击
var selector = text("打卡");
var result = click(selector);
长按
var selector = text("打卡");
var result = longClick(selector);
点击文本
var result = clickText("QQ");
坐标长按
var result = press(100, 100, 5000);
向上滚动
var selector = scrollable(true);
var result = scrollForward(selector);
向下滚动
var selector = scrollable(true);
var result = scrollBackward(selector);
swipe滑动
var selectors = text("春风");
var result = swipe(selectors, 100, 100, 200);
坐标滑动
var result = swipeToPoint(10, 10, 100, 100, 200);
坐标拖动
var result = drag(10, 10, 100, 100, 200);
元素拖动到指定坐标
var selectors = text("设置");
var result = dragToPoint(selectors, 100, 100, 200);
输入文本
var selectors = clz("android.widget.EditText");
var result = inputText(selectors, "我是内容");
将数据传给粘贴板
var selectors = clz("android.widget.EditText");
var result = pasteText(selectors, "我是粘贴板");
清除文本
var selectors = clz("android.widget.EditText");
var result = clearTextField(selectors);
等待界面出现(需要获取服务)
var ac = "com.xxx.MainActivity";
var result = waitExistActivity(ac,10000);
获取文本
var selectors = clz("android.widget.EditText");
var result = getText(selectors);
查看当前包名
var result = getRunningPkg();
查看前运行的Activity类名
var result = getRunningActivity();
展示日志浮窗
var result = showLogWindow();
关闭日志浮窗
 closeLogWindow();
自定义一个悬浮窗
requestFloatViewPermission(1000);
var m = {
  "x": 100,
  "y": 200,
  "w": 600,
  "h": 600,
  "textSize": 12,
  "backgroundColor": "#ffffff",
  "title": "我是日222志",
  "showTitle": false
}
showLogWindow();
setLogViewSizeEx(m);
sleep(5000);
随机函数
random(100,1000);
-------------------------------------------------------------------分割线-------------------------------------
系统按键
home();  返回主页
power();  电源键
back();  返回键
openNotification();  打开通知栏
openQuickSettings();  快速打开设置
recentApps();   最近APP任务按键
-------------------------------------------------------------------分割线-------------------------------------
设置当前的输入法
agentEvent.setCurrentIme();
恢复到之前的输入法
agentEvent.restoreIme();
快速截屏幕的截图
var result = agentEvent.fastScreenshot("/sdcard/a.jpg");
屏幕宽度
var width = device.getScreenWidth();
屏幕高度
var height = device.getScreenHeight();
保持屏幕唤醒状态
device.keepScreenOn();
取消保持唤醒状态
device.cancelKeepingAwake();
-------------------------------------------------------------------分割线-------------------------------------
文件函数
读取为字符串
var data = file.readFile("/sdcard/test.txt");
写入文件(如果没有该文件会自动创建)
var data="Test";
file.writeFile(data,"/sdcard/test.txt");
创建文件
var create=file.create("/sdcard/test.txt");
删除
file.deleteAllFile("data/test.txt");
追加字符串
var data="sss";
var t=file.appendLine(data,"/sdcard/test.txt");
读取一行
var t=file.readLine("/sdcard/test.txt",1);
读取所有行
var t=file.readAllLines("/sdcard/test.txt");
创建文件夹
var t=file.mkdirs("/sdcard/testdir/");
-------------------------------------------------------------------分割线-------------------------------------
工具函数
打开APP(包名)
utils.openApp("com.xx");
打开APP(应用名称)
utils.openAppByName("xx");
通过Action打开某个界面
utils.openIntentAction("android.settings.ACCESSIBILITY_SETTINGS");
```






