# mini-status

中文 | [English](./readme.en_US.md)

微信小程序miniStatus的后端程序

## 介绍

miniStatus是一个美观可定制主题的服务器监控小程序，可实现小程序与服务器的点对点连接，展示服务器基本运行状态，这个项目是其对应的后端程序。


## 特点

- 直接通过小程序和服务器的点对点连接，数据不经过第三方中转
- 不要求服务器域名备案，可以直接使用ip，甚至局域网地址


### miniStatus小程序

**小程序地址**

搜索`miniStatus`小程序

**节点列表界面**

<div  align=center><img src="https://i.loli.net/2020/05/06/lH9BU7cmOMsY5xE.jpg" width="500"/></div>

**自定义主题样式界面**

<div  align=center><img src="https://i.loli.net/2020/05/06/hiwc8jVoC5k7mrH.jpg" width="500"/></div>


## 安装

```
npm install mini-status -g
```

## 使用

### 命令行参数

```
usage: mini-status [-h] [-v] [-u URI] [-a ADDRESS] [-p PASSWORD]

miniStatus backend program

Optional arguments:
  -h, --help            Show this help message and exit.
  -v, --version         Show program's version number and exit.
  -u URI, --uri URI     uri for api, default '/update'
  -a ADDRESS, --address ADDRESS
                        address to listen, default 0.0.0.0:8080
  -p PASSWORD, --password PASSWORD
                        access password, default 1234567890
```

### 配置微信小程序

点击小程序右下角的`+`按钮，填写对应的配置选项


例如，mini-status -a A.B.C.D:8080 -u /status -p 123 对应的信息为：

接口地址：`http://A.B.C.D:8080/status`

password: 123

节点名用于标识该节点，可以随便填写


## 原理

这是一个黑魔法（hhh开玩笑

原理很简单，是巧妙运用了[小程序image组件](https://developers.weixin.qq.com/miniprogram/dev/component/image.html)的bindload接口，当图片加载成功时会返回图片的宽和高。也就是说一个图片能够返回两个数值，前后端约定好请求API后可以动态创建image获取一系列数值。

注意这个接口是不要求图片地址是备案域名，不用在小程序开发信息中报备。但是这种信息传递方式比较低下，只适合传递少量的信息。所以拿来做了这个点对点的服务器监控小程序。

但是直接传递大体积的二进制图片很浪费带宽，解决方案是后端动态生成svg图片。也就是说，动态返回下面这种形式的文字信息：

```
<svg width="${width}" height="${height}" xmlns="http://www.w3.org/2000/svg"></svg>
```

详细的实现可以看[这个文件](./lib/wxImagePing.js)


## FAQ

1.我配置填写服务器数据存放在哪？

  存放在你手机本地，放心，咱不馋你数据

2.我是小白，没有node js的开发经验，但我看着挺好看也想用用怎么办？

  我写了一个简单的安装脚本，但还没有充分测试，欢迎反馈

```
wget https://gist.githubusercontent.com/axipo/81e148e47f4a02892c22e76339b68b63/raw/4ae1fc7f1ccc42ee6a4537358e41c42b415725bd/mini_status_easy_install.sh && chmod u+x mini_status_easy_install.sh && ./mini_status_easy_install.sh
```

  或者，也可以使用下面相关项目中的一键脚本（未充分测试，有问题去提issue）

## 相关项目

- [mini-status一键安装脚本](https://github.com/tadple/mini-status-shell)