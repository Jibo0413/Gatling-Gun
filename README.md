# Gatling-Gun
base on Node.js &amp; raspberry Pi make the remote control toy gun

### 写在最前
今天终于提测了，趁着QA同学在整理各种资料，总算有个半天空闲期。借到了一个树莓派，就难以抑制自己想要做点什么的冲动。这下冷静下来思考用这个小小的东西，能做个什么好玩的出来呢？（P.S 下面所提到的任何物品均为玩具，非管制器具，在电商平台上，搜索水弹一片一片的）
![树莓派](https://img.alicdn.com/tfs/TB1kCoJeAOWBuNjSsppXXXPgpXa-1056-768.png)

### 我想要的
![遥控火炮](https://img.alicdn.com/tfs/TB1JLwtewmTBuNjy1XbXXaMrVXa-599-290.gif)

想要做这么一个东西也是机缘巧合，我喜欢玩水弹枪，属于器械党（不是器具啊，老司机起开起开），东煌大厦20层前端开发的那个小会议室里面，一直放着一把AK47，欢迎大家来前端坐坐。现在水弹枪的圈子里能人太多了，动不动就研究出来一个遥控的C4炸弹，可旋转的加特林，现在更有甚者研究出来了迫击炮还有RPG…… 

### 能力分析
树莓派相当于一台小型电脑，我手里的这个三代树莓派更是拥有4核1.2GHz处理能力的CPU以及1G的内存，树莓派原生提供了摄像头接入，GPIO控制，4个USB口也可以通过一些转接板实现串口通讯，所以完全满足搭建服务器，回传视频，计算定位，控制云台转动的基本要求；

#### 我
是一名前端开发工程师，最顺手的是JavaScript语言，顺带会用Node搭个小服务器。通过研究发现，树莓派很容易就可以部署Node服务器（内网和阮大大都有好多文章），以及对外提供良好的操作GPIO的端口的SDK，也是完全符合要求的；

#### 物品选型
基本技能、业务能力够，下面就是物品的选型了。

- 树莓派已经有了，并且能力不弱，得意得意
- 云台，我们需要一个可以进行通讯，通过指令操控云台的水平旋转，垂直旋转，并且能得到云台反馈回来的“水平角”，“俯仰角”信息，进行二次计算处理，所以首选422全双工通讯，备选485半双工
- 水弹枪，这个没什么太多要求，可以用之前的废旧枪械，也可以来一个外观拉风的，比如这样的
- 摄像头：这个没什么其他选择，配合专用摄像头，接收图像，到时候可以进行图像识别

### 服务搭建
- Raspberry Pi 3 Model B 
- 5V 2A 电源
- 外接显示器一台（hdmi接口）
- 鼠标键盘（后期可不用）

### 环境搭建：
#### 系统安装：
- 下载 NOOBS。
- 格式化 Micro SD 卡为 FAT 格式（操作指导）。
- 解压NOOBS.zip到 Micro SD 卡根目录。
- 插入 Micro SD 卡到树莓派底部的卡槽，接通电源，启动系统。
 
#### Node环境搭建：
  $ curl -sL https://deb.nodesource.com/setup_8.x | sudo -E bash -
  
  $ sudo apt install nodejs

  通过以上两个步骤，基本的Node开发环境已经稳定就绪。现在通过终端命令，可以看到熟悉的 node -v 了，热泪盈眶
  
  代码编写：
  ```
    const http = require('http');const hostname = '127.0.0.1';
      const port = 3000;
      const server = http.createServer((req, res) => {
        res.statusCode = 200;
        res.setHeader('Content-Type', 'text/plain');
        res.end('Hello World\n');});
        
    server.listen(port, hostname, () => {
      console.log(`Server running at http://${hostname}:${port}/`);});
  ```

寥寥几行最基本的Nodejs代码，启动了一个小型服务器，用来监听当前机器的3000端口

#### 防火墙配置：
ufw是一个主机端的iptables类防火墙配置工具，比较容易上手。如果你有一台暴露在外网的树莓派，则可通过这个简单的配置提升安全性。


安装方法：sudo apt-get install ufw

启用防火墙：sudo ufw enable

开放3000端口:sudo ufw allow 300 

最终结果：
浏览器中输入：http://ipadress:3000/ 即会收到熟悉的 Hello world~~

### todo List：

#### 一期完成可以远程遥控转动，远程开枪
- 服务的异常监控，开机自启、崩溃自启等；
- 利用nodejs控制树莓派GPIO，输出5V电压，激发水弹枪；
- 利用USB转422/485，连接云台，通过node服务器远程发送云台控制指令，实现云台转动；
- 组合云台、树莓派、水弹枪
- 外观打磨、喷漆
 
#### 二期接入图像回传、图像识别，自动瞄准
- 实现树莓派图像回传
- 实现云台角度回传
- 根据图像中心点，以及图像识别出的“目标体”，暴力枚举云台自动转到对应角度；
 
#### 三期可以无限延伸，发散
- 可以接入beacon，在一个房间的各个角落，接收回传的信号强度，计算定位
- 等等等等
 
