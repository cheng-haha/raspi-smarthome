# raspi-smarthome
 树莓派+nodemcu的智能家居
1.emqx服务
下载emqx：
wget https://www.emqx.io/downloads/broker/v3.2.3/emqx-ubuntu16.04-v3.2.3_amd64.deb

安装
sudo dpkg -i emqx-ubuntu16.04-v3.2.3_amd64.deb

启动，查看emq服务
sudo service emqttd start
sudo service emqttd status

使用浏览器打开EMQ控制台，http://127.0.0.1:18083，输入默认用户名:admin，默认密码public。

2.树莓派安装mqtt
接下来需要在树莓派上装一个mqtt客户端：
sudo apt-get install mosquitto-clients

发布主题测试:
mosquitto_pub -h 服务器ip -t 主题 -m 消息

3.安装hass

为Ubuntu16.04安装homeassistant
安装python3.6(注意:这里新版hass可能要安装Python3.6才支持！)
sudo add-apt-repository ppa:deadsnakes/ppa
sudo apt-get update
sudo apt-get install python3.6

安装python3.6-venv
sudo apt-get install python3.6-venv

创建并启用虚拟环境
cd /opt

sudo mkdir hass

cd /hass

sudo python3.6 -m venv .

source bin/activate

我创建在/opt/hass下，你也可以换一个目录

安装一个python包
sudo python3.6 -m pip install wheel

安装并启动homeassistant的0.89.2版本
sudo pip3 install homeassistant==0.89.2

hass --open-ui

配置homeassistant:
nano .homeassistant/configuration.yaml

添加:(注意冒号后加空格)
http:
  api_password: 密码
api:

启动：
cd /opt/hass
source bin/activate
hass --open-ui
退出：deactivate

4.安装redpoint
使用：

将redpoint.py放置在~/.homeassistant/custom_components/目录下
配置文件：
redpoint:

mkdir ~/.homeassistant/custom_components
cd ~/.homeassistant/custom_components

git clone https://github.com/cheng-haha/raspi-smarthome.git


并修改配置文件：nano .homeassistant/configuration.yaml
加入:redpoint:

5.为NodeMCU刷入ESP Easy固件
下载固件下载链接，下载后解压，双击打开FlashESP8266.exe，选择对应COM-Port(电脑设备管理那里查看)，版本选择normal 4096(没有的话从bin/里拉出来)。点击Flash

刷完后，把板子通电，然后搜索热点，你会看到有一个叫ESP_EASY_0的热点，连上，密码是configesp，之后打开浏览器，地址栏输入http://192.168.4.1 即可登陆到ESPEasy的管理界面。

然后输入自己家里WiFi和密码让nodemcu连接你家的路由器。20s之后会显示分配到的ip地址，如果没有的话，到路由器管理员界面查看一下分配给Nodemcu的IP地址，即可通过浏览器访问。


