# go-micloud

## 简介
小米云服务中网盘功能的命令行客户端，主要实现了登录、文件下载、上传、分享功能！

- 登录这块完全是模拟小米云服务Web端的登录逻辑
- 首次登录需输入手机验证码，短暂离线再上线的话可以实现自动登录
- 账号密码本地会暂存，密码已加密，长时间不登录无需再输入账号密码
- 支持手动录入cookies方式登录
- 命令支持tab补全
- 支持分享功能，可以生成一个对外公开分享的链接，类似网盘功能
## 命令介绍

本工具采用Go语言开发，但是目前只支持Linux运行环境，Windows和Mac目前木有测试，后期可以考虑支持一下。

如果只是使用该工具，请在[release](https://github.com/wangbjun/go-micloud/releases)下载解压后，运行即可！


### 一、login
登录小米云服务账号

所有的操作都需要登录小米云服务账号之后才可以进行，所以第一步就是登录。

目前登录有2种方式：

1.自己在[小米云服务](https://i.mi.com/)上面登录，然后F12打开控制台复制cookies里面的 **userId** 和 **serviceToken**，然后编辑 **～/.config/short.ini** 配置文件，填入相关配置项。

2.输入账号密码，以及首次登录需要的手机验证码。

不管咋样，当你执行login命令的时候，工具会尝试采用第一种方式登录，如果失败则会采用第二种方式。

如果你采用了第二种方式登录，登录成功后，以后都不用输入账号密码了，工具会保存账号密码，不过请放心，密码是以加密的形式保存 **～/.config/short.ini** 配置里面，并且该工具绝对不会上传用户账号和密码，请大家放心。


### 二、ls
列出当前目录下的文件

```
total 13
d | ------ | 2018-10-26 12:51:07 | Doc
d | ------ | 2018-10-26 13:01:12 | Books
d | ------ | 2018-10-26 13:03:04 | Picture
d | ------ | 2018-10-26 13:03:10 | Package
- | 4.2 MB | 2019-12-23 17:13:12 | ProxifierSetup.exe
- | 71 MB  | 2019-12-23 17:20:11 | Geekbench-4.2.3-Linux.tar.gz
- | 69 MB  | 2019-12-23 17:20:56 | Postman-linux-x64-7.1.1.tar.gz
- | 259 MB | 2019-12-23 17:28:36 | wps-office_11.1.0.8722_amd64.deb
- | 140 MB | 2019-12-24 13:45:40 | navicat15-premium-en.AppImage
- | 1.9 MB | 2019-12-24 13:48:00 | Hacking_Device_v1.bik
- | 34 MB  | 2019-12-24 13:49:03 | ARM_Translation_Marshmallow.zip
- | 492 kB | 2019-12-24 13:49:09 | Baidu_Voice_RestApi_SampleCode.zip
- | 1.0 GB | 2019-12-24 13:53:30 | Deepin-Apps-Installation.zip
```
这个命令有点类似Linux系统下ls，只不过功能简单，没有额外参数，会显示文件类型、文件大小、创建时间、以及文件名。

### 三、cd
切换工作目录并列出当前目录下的文件
```
cd <dir>
```
这个命令类似Linux下的cd，但是功能有限，大家体验一下就知道了。

### 四、download
下载当前目录下的一个或多个文件（目前还不支持文件夹）,多个以空格隔开
```
download <file...>
```

```
Go@MiCloud:~$ download Postman-linux-x64-7.1.1.tar.gz
[ Postman-linux-x64-7.1.1.tar.gz ]开始下载！
[ Postman-linux-x64-7.1.1.tar.gz ]下载成功！
```

下载文件的存放位置，默认是当前工具的运行目录，如需配置，可以在**app.ini**里面配置**WORK_DIR**项

### 五、upload
上传一个文件到当前目录下（目前还不支持文件夹）
```
upload <filepath>
```
路径必须是绝对路径，如 /home/jwang/abc.jpg

由于小米云服务web端的限制，目前单个文件最大限制4GB

### 六、share
生成一个对外公开分享的链接，类似网盘
```
share <file>
```
这个功能需要单独说下，理论上说小米网盘的文件只能自己下载，但是其实并不是，小米还是提供了入口，只不过没有对外开放，但是有限制，一些大文件的无法分享。

根据我测试，一般几百MB左右的文件还是可以分享出去的，链接有效期是24小时，不过下载速度非常快，也不用开会员。
```
Go@MiCloud:~$ ls
total 13
d | ------ | 2018-10-26 12:51:07 | Doc
d | ------ | 2018-10-26 13:01:12 | Books
d | ------ | 2018-10-26 13:03:04 | Picture
d | ------ | 2018-10-26 13:03:10 | Package
- | 4.2 MB | 2019-12-23 17:13:12 | ProxifierSetup.exe
- | 71 MB  | 2019-12-23 17:20:11 | Geekbench-4.2.3-Linux.tar.gz
- | 69 MB  | 2019-12-23 17:20:56 | Postman-linux-x64-7.1.1.tar.gz
- | 259 MB | 2019-12-23 17:28:36 | wps-office_11.1.0.8722_amd64.deb
- | 140 MB | 2019-12-24 13:45:40 | navicat15-premium-en.AppImage
- | 1.9 MB | 2019-12-24 13:48:00 | Hacking_Device_v1.bik
- | 34 MB  | 2019-12-24 13:49:03 | ARM_Translation_Marshmallow.zip
- | 492 kB | 2019-12-24 13:49:09 | Baidu_Voice_RestApi_SampleCode.zip
- | 1.0 GB | 2019-12-24 13:53:30 | Deepin-Apps-Installation.zip
Go@MiCloud:~$ share wps-office_11.1.0.8722_amd64.deb
获取分享链接成功(采用了短链接，有效期24小时): http://t.wibliss.com/BRfnl
```

---
基本上就是这些功能，时间有限，难免会有bug，如果大家有什么意见或者bug需要反馈，可以直接提issue，后面我会继续完善。
