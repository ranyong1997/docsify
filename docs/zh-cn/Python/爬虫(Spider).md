# Python爬虫学习

## 一、什么是爬虫

### 1.1 爬虫Spider的概念

爬虫用于爬去数据，又称数据采集程序

爬去的数据来源于网络，网络中的数据可以由Web服务器（Nginx/Apache）、数据库服务器（MySQL/Redis）、索引库（ElastichSearch）、大数据（Hbase/Hive）、视频/图片库（FTP）、云存储（OSS）等

爬去数据是公开的、非盈利的。

### 1.2 Python爬虫

使用python编写的爬虫脚本（程序）可以完成定时、定量、指定目标（Web站点）的数据爬去。主要使用多（单）线程/进程、网络请求库、数据解析、数据存储、任务调度等相关技术。

## 二、爬虫与Web后端服务之间的关系

爬虫使用网络请求库，相当于客户端请求，Web后端服务根据请求相应数据。

爬虫即向Web服务器发起Http请求，正确的接收相应数据，然后根据数据类型(Content-Type)进行数据的解析及存储。

爬虫程序在发起请求前，需要伪造一个浏览器（User-Agent指定请求头），然后在向服务器发起请求，响应200的成功率高很多。

## 三、Python爬虫技术的相关库

网络请求：

​	● urllib

​	● requests

​	● selenium（UI自动化测试、动态js渲染）

​	● appium（手机App的爬虫或UI测试）

数据解析：

​	●  re正则

​	●  xpath

​	●  bs4

​	●  json

数据存储：

​	●  pymysql

​	●  mongodb

​	●  elasticsearch

多任务库：

​	●  多线程（threading）、线程队列 queue

​	●  协程（asynio、gevent/eventlet）

爬虫框架

​	●  scrapy

​	● scrapy-redis 分布式（多机爬虫）

## 四、常见反爬虫的策略

​	● UA（User-Agent）策略

​	● 登录限制（Cookie）策略

​	●请求频次（IP代码）策略

​	●验证码（图片-云打码，文字或物件图片选择验证、滑块）策略

​	●动态js（Selenium/Splash/Api接口）策略

```python
#!/usr/bin/env python
# -*- coding:utf-8 -*-

"""
Author: W9007851
Date: 2020/11/10
File: spider01_urllib.py
"""


"""
初次使用urllib实现爬虫数据请求
urllib.request.urlopen(url)  发起get请求
urllib.parse.quote()  将中文进行url编码
urllib.request.urlretrieve(url, filename) 下载url保存到filename
"""

from urllib.request import urlopen, urlretrieve, Request
from urllib.parse import quote
import ssl

ssl._create_unverified_context = ssl._create_unverified_context


def search_baidu(keyword="JAVA"):
    # 网络资源的接口(URL)
    url = 'http://mindoc.itest.wanyol.com/search?keyword=%s'

    # 生成请求对象，封装请求URL和heander
    request = Request(url % quote(keyword),
                      headers={
					 "Cookie": "mindoc_id=ad3432337ccefae8ef30bea881f158f8",
                          "User-Agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/83.0.4103.106Safari/537.36"
                      })
    response = urlopen(request)  # 发起请求头
    assert response.code == 200
    print('请求成功')

    # 读取响应的数据
    bytes_ = response.read()
    with open('%s.html' % keyword, "wb") as file:
        file.write(bytes_)


if __name__ == '__main__':
    search_baidu()
```

## 五、爬虫库Urllib【重要】

### 5.1 Urllib.request模块

##### 5.1.1 简单的请求

```py
from urllib.request import urlopen

# 发起请求
response = urlopen("http://www.hao123.com")
assert response.code == 200
print ("请求成功")
# 保存请求的网页
# f 变量接收open()函数返回的对象__enter__()返回结果
with open('sava.html', 'wb') as f:
	f.write(response.read())
```

urlopen(url, data=None)可以直接发起url的请求，如果data不为空时，则默认是POST请求，反之为GET请求。

response是http.client.HTTPResponse类对象

##### 5.1.2带请求头的请求

```python
from urllib.request import Request

def search_baidu():
    #网络资源的接口(URL)
    url = 'https://www.baidu.com'
    
    # 生成请求对象，封装请求的url和头header
    request = Request(url,
                     headers= {
                         "Cookie": "confluence.browse.space.cookie=space-blogposts; mywork.tab.tasks=false; JSESSIONID=F551A1F1927D5AEA7D7859ECC7CFB512"
                     })
    response = urlopen(request)  # 发起请求
    
    assert response.code == 200
    print("请求成功")
    
    # 读取响应的数据
    bytes_ = response.read()
    
    #将响应的数据写入文件中
    with open('index.html', 'wb') as file:
        file.write(bytes_)
```

### 5.2 urllib.parse模块

此模块有两个核心的函数：

- quote()仅对中文字符串进行url编码

- urlencode()可以针对一个字典中所有的values进行编码，然后转成key=value&key=value的字符串。

  ```python
  from urllib.parse import quote, urlencode
  quote('冉勇')
  输出：'%E5%86%89%E5%8B%87'
  urlencode({'wd':'冉勇'})
  输出：'wd=%E5%86%89%E5%8B%87'
  ```

  ```python
  """
  应用：百度翻译
  - urllib.request.Requtst
  - urllib.request.urlopen()
  - urllib.parse.urlencode()
  - 发起post请求
  """
  
  from urllib.request import Request, urlopen
  from urllib.parse import urlencode
  
  
  import ssl
  # ssl._create
  
  url = "https://fanyi.baidu.com/langdetect"  # 请求的api接口
  headers = {
      "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36 Edg/86.0.622.63",
      "cookie": "BIDUPSID=F379D6EE5D9F6B5CE164CBE299F7F077; PSTM=1604235296; BAIDUID=F379D6EE5D9F6B5C85B738C1A637D15B:FG=1; BDUSS=jF5TS0yTlljaFNtTU1jNmVkY3FoSGkyWXNuV0RmczZ4VUxYcWNkZnpwWnJQY1pmRVFBQUFBJCQAAAAAAAAAAAEAAAA-sw5d0qGw2si5z8K1xMDkxK4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGuwnl9rsJ5fe; BDUSS_BFESS=jF5TS0yTlljaFNtTU1jNmVkY3FoSGkyWXNuV0RmczZ4VUxYcWNkZnpwWnJQY1pmRVFBQUFBJCQAAAAAAAAAAAEAAAA-sw5d0qGw2si5z8K1xMDkxK4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGuwnl9rsJ5fe; BAIDUID_BFESS=8586C4AE0A65E3ECFB705BC873A2447B:FG=1; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; H_PS_PSSID=32817_1461_33050_32952_33059_31253_32972_32723_32962_32958_32846; BDRCVFR[feWj1Vr5u3D]=I67x6TjHwwYf0; delPer=0; PSINO=1; BA_HECTOR=a5a0058l80a48k97j81fqlfej0p; Hm_lvt_64ecd82404c51e03dc91cb9e8c025574=1605026773; Hm_lpvt_64ecd82404c51e03dc91cb9e8c025574=1605026773; REALTIME_TRANS_SWITCH=1; FANYI_WORD_SWITCH=1; HISTORY_SWITCH=1; SOUND_SPD_SWITCH=1; SOUND_PREFER_SWITCH=1; yjs_js_security_passport=af655cd186d5e402f5f8b2742e576ee7862bd4ac_1605026792_js",
      "x-requested-with": "XMLHttpRequest"  # 请求类型
  }
  
  
  def fanyi(query):
      data = {
          "query": query
      }
  
      # Request()中的data参数是byte类型
      request = Request(url, data=urlencode(data).encode('utf-8'))
      response = urlopen(request)
      assert response.code == 200
  
      json_data = response.read()  # byte
  
      content_encode = response.getheader("Content-Type")
      content_encode = "utf-8" if content_encode is None else content_encode.split("=")[-1]   # 截取从后往前数第一个"="内容
      return json_data(json_data.decode(content_encode))
      return json.loads(json_data.decode(content_encode))
  
  
  if __name__ == '__main__':
      fanyi('apple')
  ```

#### 实战：百度贴吧网页爬取

```python
"""
复杂的get请求，多页面请求下载
"""
from urllib.request import Request, urlopen
from urllib.parse import urlencode
from time import sleep

url = 'https://www.baidu.com/s?'
headers = {
    "user-agent": "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/86.0.4240.183 Safari/537.36 Edg/86.0.622.63",
    "cookie": "BIDUPSID=F379D6EE5D9F6B5CE164CBE299F7F077; PSTM=1604235296; BAIDUID=F379D6EE5D9F6B5C85B738C1A637D15B:FG=1; BDUSS=jF5TS0yTlljaFNtTU1jNmVkY3FoSGkyWXNuV0RmczZ4VUxYcWNkZnpwWnJQY1pmRVFBQUFBJCQAAAAAAAAAAAEAAAA-sw5d0qGw2si5z8K1xMDkxK4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGuwnl9rsJ5fe; BDUSS_BFESS=jF5TS0yTlljaFNtTU1jNmVkY3FoSGkyWXNuV0RmczZ4VUxYcWNkZnpwWnJQY1pmRVFBQUFBJCQAAAAAAAAAAAEAAAA-sw5d0qGw2si5z8K1xMDkxK4AAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAAGuwnl9rsJ5fe; BAIDUID_BFESS=8586C4AE0A65E3ECFB705BC873A2447B:FG=1; BDORZ=B490B5EBF6F3CD402E515D22BCDA1598; H_PS_PSSID=32817_1461_33050_32952_33059_31253_32972_32723_32962_32958_32846; BDRCVFR[feWj1Vr5u3D]=I67x6TjHwwYf0; delPer=0; PSINO=1; BA_HECTOR=a5a0058l80a48k97j81fqlfej0p; Hm_lvt_64ecd82404c51e03dc91cb9e8c025574=1605026773; Hm_lpvt_64ecd82404c51e03dc91cb9e8c025574=1605026773; REALTIME_TRANS_SWITCH=1; FANYI_WORD_SWITCH=1; HISTORY_SWITCH=1; SOUND_SPD_SWITCH=1; SOUND_PREFER_SWITCH=1; yjs_js_security_passport=af655cd186d5e402f5f8b2742e576ee7862bd4ac_1605026792_js",
    "x-requested-with": "XMLHttpRequest"  # 请求类型
}

params = {
    'wd': '',
    'pn': '0'
}


def pages_get(wd):
    params['wd'] = wd
    for page in range(1, 101):
        params['pn'] = (page-1)*10

        page_url = url+urlencode(params)
        response = urlopen(Request(page_url, headers=headers))

        assert response.code == 200
        file_name = 'baidu_pages/%s-%s.html' % (wd, page)
        with open(file_name, 'wb') as f:
            bytes_ = response.read()
            f.write(bytes_)
            print(f'{file_name} 写入成功!')
            sleep(0.5)

    print('下载 %s 100页成功！' % wd)


if __name__ == '__main__':
    pages_get('Python3.6')

```

### 5.3 HTTP处理器

创建Handler对象

```python
handler = urllib.request.HTTPHandler()
```

创建opener对象

```python
opener = urllib.request.build_opener(handler)
```

创建Request对象

发送Request请求

```python
opener.open(request)
```

