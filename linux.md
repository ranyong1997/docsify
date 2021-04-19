# Headlin1

> An awesome project.

## Linux基本指令

## 指令名称：cd 进入目录

```python
cd ..  回到上一级目录
cd ~ 回到用户的宿主目录
cd / 切换到根目录
cd ../usr 切换至上一层目录的usr目录
```

## 指令名称：ls 显示目录文件

```python
ls[-al] [文件或目录]
ls -a 显示所有的文件，包括隐藏
ls -l 使用长格式显示
```

## 指令名称：pwd 显示当前所在的工作目录

```python
pwd 显示当前所在的工作目录
```

## 指令名称：|（管道符）

```python
| 将上一个命令的输出作为下一个命令的输出
例如： tailf cdo-searchx-api.log |grep result
```

## 指令名称：chmod 改变文件或目录的权限

```python
chmod 777 abc  表示将abc文件权限改为rwxrwxrwx  
```

## 指令名称：chown 改变文件或目录的所有者

```python
chown -R [用户][群组][文件或目录]
chown service abc.txt   改变文件adb的所有者为service(用户名)
chown -R service :service games 改变目录games的所有者为service，所属组改为services
```

## 指令名称：mkdir 创建目录

```python
mkdir adc  创建一个adc的文件夹
mkdir adc dcd  当前目录前创建多个文件夹
mkdir -p adc/adb 创建文件夹及子目录
```

## 指令名称：cp 复制文件

```python
cp -pr [源文件或目录][目的文件或目录]
cp -p  保留源文件的日期属性
cp -r  递归复制所有文件
例如： cp file1 file2   将文件名为file1 的文件复制一份为文件名为file2的文件
cp file1 file2 dir 将文件file1 file2 复制到目录dir
cp -r dir1 dir2 将dir1下的所有目录包括子目录陆续地复制到dir2
cp file1 file1.bak 生成一个备份文件
```

## 指令名称：rm 删除文件

```python
rm -rf [文件或目录]
rm -r 递归删除目录
rm -f 强制删除
rm -i 删除已有文件或目录之前先询问用户
rm -rf dir 强制删除整个目录
rm -r* (删除所有文件)
```

## 指令名称：mv 移动文件

```python
mv -f [源文件或目录][目的文件或目录]
mv -f 强制移动
例如：mv file1 file2 将文件file1移动到file2(也是更名操作) 若file2为目录，则将file1移动到file2目录下
```

## 指令名称：find 文件搜索

```python
find -name 按照文件名查找
find -mtime -n +n 按照文件的更改时间来查找
find -type 查找某一处类型的文件
例如：find -name "*.txt"   查找文件名符合*.txt的文件
find -name c?? 查找所有以c开头后面有2个字符的文件
find -mtime -5 查找更改时间在5天以内的文件
```

## 指令名称：grep 文件搜索

```python
grep [要找的字串][要寻找字串的源文件]
例如：grep game /etc/hosts   查找host关于game的字段
```

## 指令名称：less 按页显示文件内容

```python
less [文件名]
例如：less/etc/services
使用键盘上的pageup/pagedown进行上翻和下翻
shfit+f 跳转至日志底部进行滚动
ctrl+c 暂停
/ 可进行搜索
```

## 指令名称: > 日志清理

```pyhon
>[文件名]  清空文件内容（不需要删除文件）
>service.log
```

## 指令名称：vim 文本编辑器

```python
i 进行插入模式
A 在当前行最后插入
O 在当前行上面新建一行插入
I 在当前行第一个非空字符前插入
ESC 退出
:wq 保存退出
:q! 强制退出
：set nu 显示行数
```

## 指令名称：tar 压缩解压

```python
tar -z 使用gzip压缩.tar文件
tar -c 产生一个.tar文件
tar -v 观看归档过程
tar -f 指定归档后的文件
tar -x 解开一个压缩文件的参数指令
例如：tar -cvf games.tar/usr/games 将目录usr/games下所有文件打包成一个文件
tar -cvf shuzi.tar 234.txt 123.txt 将234.txt 和123.txt打包成shuzi.tar文件
如果要解压 tar -xvf games.tar
```

## 指令名称：ps 查看系统中运行的进程

```python
ps -e 显示所有程序
ps -f 显示UID,PID,C,STIME
例如：ps -ef |grep java
```

## 指令名称：kill 停止进程

```Python
kill -9 强行杀掉指定进程
```

## 指令名称：netstat 网络连接、路由表和网络接口信息

```python
netstat -a 显示所有连接端口
netstat -p 显示PID或者进程名称
netstat -n 拒绝显示别名
netstat -l 只输出监听端口
例如：nestat -npa |grep 进程ID 可查看端口
nestat -nlp |grep 端口 可查看进程ID
```

## 指令名称：df 硬盘空间

```python
df -h 显示文件系统的硬盘使用情况统计
```

## 指令名称：du 硬盘内部的目录或者文件的信息

```python
du -s 仅显示目录的总值，单位KB
du -h 以K M G为单位显示，提高可读性
例如：du -sh 查看当前目录的总值
du -sh * 查看当前目录每个文件和子目录的值
```

## 指令名称：top 进程资源占用情况

```python
top 查看进程资源系统占用情况
```

## 指令名称：su 切换用户

```python
su -service 切换到service用户
su -root 回到root用户
```

