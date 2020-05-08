Hackintosh_i5-7500_B250_HD630_EFI
====
针对能正常使用于 10.15.4，只对核心显卡，网卡，声卡，以及 USB 做了相应的驱动设置。可能不适应其他 macOS 版本。在介绍后面已知的问题，如有解决的朋友请告知，多谢。

![系统信息](./Screenshot/System.png)
![音频](./Screenshot/ALC892.png)
![USB](./Screenshot/USB.png)

硬件&系统
----
* 主板：GIGABYTE B250M-D3H
* CPU：Intel i5 7500
* 显卡：Intel HD Graphics 630
* 使用安装镜像 10.15.4 (19E287)
* 使用 Clover v5116 引导
* 使用机型 iMac18,1、MacMini2018,1

驱动情况
----
* HD630 核显使用 WhateverGreen-1.3.8_t3.kext 正常，针对核心显卡修改后版本
* 声卡使用 AppleALC-1.3.9.kext 输出、输入正常，注入 ID = 20 (其他尝试可用：3，13)
* 网卡使用 AppleIntelE1000e.kext 正常
* USB 使用 USBInjectAll_v0.7.4.kext 正常识别 USB3.0,USB2.0
* 休眠正常
* 使用 DIV 接口正常
* 加入了 ACPISensors.kext、CPUSensors.kext、GPUSensors.kext 传感器，可删除不影响

问题
----
* 无法睡眠，临时解决方法：`sudo pmset -a disablesleep 1 ` 防止进入睡眠模式
* DP 无法开启 4k ，只有 1920*1080 。
* HDMI 待测试。
* clover v5116 引导界面主题图标显示错位，但不影响使用。
