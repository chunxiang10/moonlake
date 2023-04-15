---
title: '低成本自建家庭监控系统'
date: '2023-04-15T15:09:44+0800'
tags: []
categories: []
---

### 低成本自建家庭监控系统

前两天在咸鱼上收了2个二手的海康摄像头，就想在家里建一个简单的监控系统，网上搜索了一圈，目前初步完工。仅以记录备忘。

#### 设备清单

|   设备名称   |     规格型号     | 单位 | 数量 |    单价    | 金额 | 备注 |
| :---------: | :----------: | :--: | :--: | :-----: | ---- | :-----: |
|  摄像头   | 海康威视 DS-2CD3326WD-I |  只  |  2   |  0   | 0 | 客厅、门口 |
| 旧笔记本电脑 |      联想 Y460     |  台  |  1   |  0   | 0 | Debain系统 |
|     TF卡     |     32G     |  只  |  1   |  0   | 0 |  |
|     网线    |      CAT5E       |  米  |  30  | 0 | 0 |  |
|    交换机    | 5口POE交换机 | 台 | 1 | 65 | 65 | 京东购买 |
|    辅材    | 螺丝、水晶头等 | 台 | 1 | 0 | 0 |  |

#### 拓扑图

{{<mermaid>}}
graph LR
    A(路由器)-->B(POE交换机)
    B--01-->D(录像机<br>联想Y460)
    B--04-->C(门口摄像头)
    B--05-->E(客厅摄像头)
{{</mermaid>}}

#### 设计图

![](/pictures/nvr/nvr01.jpg)

#### 施工过程

1. 综合布线

   客厅和餐饮的顶部沿墙角有一圈吊顶，网线全部藏在吊顶里。如果没有吊顶，可以选择白色网线。

2. 安装摄像头

   摄像头安装在吊顶下方，半球摄像头体积小，不突兀，美观大方。

3. 系统调试运行

   本方案采用了录像和实时预览分离的方式：`beltranalex928/nvr`这个docker镜像负责录制，手机使用VLC、KMPLAYER、XMPLAYER等APP观看实时影像。

   3.1 将TF卡插入Y460的USB接口并挂载

   ```bash
   # 查看tf卡在系统内的标识
   fdisk -l | grep FAT32
   # /dev/sdb1 * 56 640 3580928 c W95 FAT32 (LBA)
   # 创建挂载点
   mkdir ~/nvr
   # 挂载tf卡
   mount -t vfat /dev/sdb1 ~/nvr
   ```

   3.2 设置开机自动挂载

   ```bash
   # 打开文件
   sudo vim /etc/fstab
   # 在文件末尾添加下面一行
   UUID="xxxx" ~/nvr vfat defaults,nofail 0 0
   ```

   3.3 新建必要的文件夹

   ```bash
   cd ~/nvr
   mkdir {config,recordings}
   mkdir recordings/{Door,LivingRome}
   ```

   3.4 添加摄像头

   在`~/nvr/config`文件夹内新建`streams.txt`文件，安装下面格式增加摄像头

   ```bash
   # 创建streams.txt文件
   sudo vim streams.txt
   # 增加以下内容
   rtsp://username:password@address:port/Streaming/Channels/2;Door
   rtsp://username:password@address:port/Streaming/Channels/2;LivingRome
   ```

   3.5 使用docker安装nvr

   ```bash
   docker run -d \
   --name='NVR' \
   -e TZ="Asia/Shanghai" \
   -e 'MAX_SIZE'='0.03' \
   -e 'HOUSEKEEP_ENABLED'='True' \
   -e 'VIDEO_FORMAT'='mkv' \
   -e 'VIDEO_SEGMENT_TIME'='15' \
   -e 'PUID'='99' \
   -e 'PGID'='100' \
   -v '~/nvr/config':'/streams':'rw' \
   -v '~/nvr/recordings':'/recordings':'rw' \
   -it --restart=always 'beltranalex928/nvr:latest

   # -e "MAX_SIZE"="1" \ 是保存录像的最大容量，单位是T，1T=1024G
   # 我的tf卡容量是32G 32G/1024G≈0.03T
   ```

   3.6 APP安装及设置

   这里介绍VLC的设置方式，KMPLAYER和XMPLAYER的设置也类似，可以参考设置
  ![](/pictures/nvr/nvr02.jpg)
  ![](/pictures/nvr/nvr03.jpg)
  ![](/pictures/nvr/nvr04.jpg)
4. 项目验收

   本项目于2023年4月10日开工建设，于2023年4月10日完工，历时1天，安照设计方案完成全部工程。于当天邀请大小领导进行验收，领导很满意，验收结果合格！

#### 总结

本次项目很简单，不到1天就完成了，基本满足现在的需求。

未来还会增加语音通话功能，实现实时对讲。这需要摄像头支持语音和播放功能。


---