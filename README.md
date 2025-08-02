# Minecraft-WSS-5K
WSS-5K项目是由chenmoufan发起的将我的世界WSS服务器五千格内区域下载为存档的项目。该仓库包含了运行WSS-5K项目的必要资源。当然，你也可以使用仓库内的资源来下载其他服务器的存档（先让你的服务器管理员知道这件事！）
## 项目介绍
WSS-5K项目基于我在25年初制作的另一个基于baritone机器人的项目，但是那个项目只能点亮xaero地图，不能将服务器区块下载为存档。本项目在其基础上增加了一个代理（准确来说是两个，因为我把用于版本转换的viafabric独立出来了）用于截留服务器地图数据。
# 配置&运行
接下来你需要配置这几个基于java的程序：Minecraft游戏本体以及它的模组，viaproxy代理，world-downloader代理。你需要保证你的电脑上有至少4G空余存储空间，6G可用运行内存；并且25568端口，25565端口和11451端口应保持空闲。
本项目适用于版本为1.8~1.21的我的世界服务器（这取决于viafabric代理支持的版本），且没有特别严格的反作弊，无验证码登录插件。
该教程默认你的配置环境是ubuntu22以上版本，且安装了Java17和Java21。
## 配置viaproxy代理
要获取viafabric，你可以访问 https://github.com/ViaVersion/ViaProxy/releases 并下载形如`ViaProxy-3.4.4.jar`的jar文件并把它保存到一个好找到的独立文件夹中。将它命名为ViaProxy.jar。
### 第一次运行
为了今后能快速启动这个代理，你可以在viaproxy所在文件夹打开终端，并使用如下创建一个`start.sh`文件
```
nano start.sh
```
如果你的Ubuntu没有nano编辑器，请使用`sudo apt install nano`来下载它。
如果你的Ubuntu有nano且已经打开了，请其中编辑如下内容
```
java -jar ViaProxy.jar cli
```
该命令将使你使用命令行模式启动viaproxy（你也可以尝试去掉`cli`来使用图形化运行viaproxy）
按下ctrl+x键退出，此时nano询问你是否写入缓冲区（Save modified buffer?），按y，接着询问是否保存到一个路径，按回车，之后使用nano编辑文件都是如此。
退出后使用`bash start.sh`运行。
如果一切正常，那么你的终端此时应出现以下内容
```
[16:58:44] [main/INFO] (ViaProxy) Initializing ViaProxy CLI v3.4.0 (git-ViaProxy-3.4.0:1a6580b) (Injected using Launcher Agent)...
[16:58:44] [main/INFO] (ViaProxy) Using java version: OpenJDK 64-Bit Server VM 21.0.8 (Ubuntu) on Linux
......
[16:58:59] [main/INFO] (ViaProxy) Binding proxy server to 0.0.0.0:25568
[16:59:03] [Via Async Scheduler 0/INFO] (ViaVersion) Finished mapping loading, shutting down loader executor.
```
如果出现`[16:59:04] [ForkJoinPool.commonPool-worker-1/WARN] (ViaProxy) You are running an outdated version of ViaProxy!`的提示可以不必理会，这只是新版本检测。
### 配置
接下来输入exit退出viaproxy，在第一次运行后，它应该创建了一些文件。
输入`ls`命令查看当前文件夹内的文件，如果viaproxy真的运行成功了，应该会有以下文件
```
ViaLoader  ViaProxy.jar  jars  logs  plugins  saves.json  start.sh  viaproxy.yml
```
输入`nano viaproxy.yml`编辑配置。
通常来说，我们只需要修改其中的一小部分就好了（你可以根据自己需要自行修改部分配置）。
```
bind-address: 0.0.0.0:25568  #将其中的0.0.0.0替换为你想运行minecraft的设备ip地址，如果viafabric与minecraft在同一设备上运行，请改为127.0.0.1
target-address: 127.0.0.1:25565  #将127.0.0.1:25565替换为你想要连接的服务器地址
target-version: Auto Detect (1.7+ servers)  #将Auto Detect (1.7+ servers)替换为你的服务器版本
```
再次运行start.sh，在确保没有影响运行的报错后viaproxy就算配置完成了。
## 配置游戏本体
### 启动器下载
这里选用hmcl启动器，因为我们需要用到hmcl启动器的高级配置选项，请确保你的设备安装了javafx和java17。你可以通过`https://hmcl.huangyuhui.net/download/`下载，对于不能忍受城通网盘的人，你也可以到
### 启动器配置（如果你在x86架构上运行minecraft，你可以跳过这一步）
对于使用aarch64架构处理器的设备（如树莓派，香橙派）你需要补全
