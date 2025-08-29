# 本文所有应用建立在Windows环境下完成 建议Windows 10及以上系统


### 1.安装 Icecast[https://icecast.org/] 和 BUTT[https://danielnoethen.de/butt/]

### 什么是Icecast和BUTT?

Icecast 是流媒体服务器，负责将你的音频流分发到互联网；而BUTT是推流软件，负责将你的音频流推送到Icecast服务器.
下载windows版即可

###  2.配置Icecast
进入Icecast安装目录,打开icecast.xml文件。

关于这个XML的官方文档(https://icecast.org/docs/icecast-2.4.1/config-file.html)

但是没必要调这么多 你只需要修改以下几个地方：
- ####  source-password（源密码）：
拿到它的人可以冒充你进行广播，直接向你的听众发送任何他们想发送的内容

- #### admin-password（管理员密码）：
保护你的服务器设置不被篡改。
- #### relay-password（中继密码）：
目前暂时用不到。但是为了安全起见，你还是改一下。

把hackme改为自己的密码，推荐使用复杂密码，包含字母、数字和特殊字符。比如:20250829@0748! (~~除非你真的想被hack~~)

- #### hostname（主机名）：
你的服务器主机名。一般保持localhost不变即可。
- #### listen-socket(监听端口):
Icecast的监听端口。默认是8000

#### 可能会有影响的参数，自行调节:
<source-timeout>:如果启用，这将在客户端首次连接时提供突发数据，从而显著减少需要大量缓冲的监听器的启动时间。然而，这也会显著增加源客户端和监听客户端之间的延迟。对于低延迟设置，您可能需要禁用此功能。

#### 其他用来描述你自己电台的参数
- #### location（位置）：
描述你的服务器位置。比如:Beijing,China
- #### name（名称）：
你的服务器名称，比如：Godspeeddoge's Live Stream
- #### description（描述）：
对服务器的简单描述。
(大多数没必要改因为你可以等下可以直接在BUTT改)

保存后，运行在Icecast根目录下的icecast.bat。如果有Windows防火墙弹窗请允许。
打开浏览器输入  **本机IP地址:8000** 来看是否运行（如果你改了端口你就用你改了的端口）
### 3.配置BUTT
#### 打开Settings-Main:
- ##### Server选项下-点开ADD-选择Icecast
- Address: 填 IP地址或者你的公网地址
- Port:填你刚才在xml设置的监听端口
- Password:填写你的Source-passwords。

#### 自定义你的Stream infos:
- 不详细介绍 自己玩~

#### 打开Settings-Audio:
这里是关于你的音频输入.你可以更改你串流的音质，声道，比特率；声音来源；录音（录制电台）音质
- 声道肯定双声道(Stereo),采样率44.1k往上即可
- 建议Streaming选择OPUS-128K 这已经足以够用，FLAC再好也听不出什么差别其实 ~~（反正玩玩看你了）~~
- 怎么计算你带宽能多少人一起听？
去 [中国科学技术大学测速网站](https://test.ustc.edu.cn/)然后看上行带宽。单位应该是Mbps。你得到的数*1024/你的Streaming码率=最多多少人听

### 4.打开音乐播放器播放你的音乐
### 5.点击BUTT上的那个播放按钮。如果看到Stream Time就代表正在运行了~
### 6.打开网络电台
- 在本地可以使用IP地址:8000\stream
- 你可以使用Foobar2000/VLC Player/PotPlayer来播放
(创建一个文本文件 然后里面输入http://IP地址:8000/stream 然后你再改为m3u 就可以导入了 也可以直接输入链接收听)

###7.什么？你说你音乐声和游戏声音在打架？
如果你想在播放音乐的同时还能用电脑做其他事（如游戏、看视频）而不想把所有声音都广播出去，那么虚拟音频线是最佳选择。

你创建一个虚拟声卡。让播放器将声音输出到这个虚拟声卡，然后让 BUTT 从这个虚拟声卡抓取声音。

操作步骤：

#### 安装虚拟音频线软件：

- [VB-Cable (Windows)](https://vb-audio.com/Cable/)

安装后，你会在系统的声音设置里看到一个新的输入/输出设备，叫做 CABLE Input 和 CABLE Output。

#### 配置播放器：

- 打开你的播放器（如 foobar2000）。

-  进入播放器的输出设置，将音频输出设备从默认的扬声器更改为 VB-Cable Input。

- 现在，播放器的声音会送入虚拟电缆，而不会从你的真实音箱出来（除非你做下一步）。


#### 配置 BUTT：

- 打开 BUTT 设置，在 Audio Device 中，选择 VB-Cable Output 作为输入设备。这样 BUTT 就只会捕获从虚拟电缆传来的声音（即你的播放器的声音）。

- 重新配置Settings-Audio然后重新串流即可

##关于挂载点
- 挂载点是自动创建的
所以，你不需要提前为挂载点做任何额外的操作。你只需要：

- 在BUTT里设置：打开BUTT的Settings，在 Stream 标签页里，自己想一个名字填到 Icecast/Mountpoint 这个输入框里。

 - 格式：必须以斜杠 / 开头，并且通常以 .mp3 或 .ogg 结尾。

 - 举例：你可以填 /mystream.mp3，或者 /radio.ogg，或者 /my_live_stream（但不推荐不加扩展名）。

此时，你打开Icecast的管理后台 (http://localhost:8000) ,就能在“List Mountpoints”下看到你刚刚创建的 /live.mp3 挂载点，它的状态是“active”（活跃的）


##关于端口转发:
- 其实你可以直接通过软路由转发这个端口，但是不可避免的，别人可以用admin试着你的密码，你只能设置密码设置的好好的。越复杂越好。
