# Hackintosh_i5-7500_B250_HD630_EFI
针对能正常使用于 `Catelina 10.15.4`，只对核心显卡，网卡，声卡，以及 USB 做了相应的驱动设置。核心显卡，只选择机型，让 `WhateverGreen` 自动完成驱动。声卡使用 `AppleALC` 指定 `ID = 97`。可能不适应其他 macOS 版本。在介绍中的已知问题，如有解决的朋友请告知，多谢。

![系统信息](./Screenshot/System.png)
![音频](./Screenshot/ALC892.png)
![USB](./Screenshot/USB.png)


## 目录
- [硬件&系统](#硬件系统)
- [驱动情况](#驱动情况)
- [已知问题](#已知问题)
- [解决双系统时间不同步](#解决双系统时间不同步)
- [注入三码使正常使用 Apple 服务](#注入三码使正常使用-apple-服务)
- [开启 HiDPI](#开启-hidpi)
- [第三方 SSD 固态硬盘开启 TRIM](#第三方-ssd-固态硬盘开启-trim)
- [Clover Configurator 设置截图](#clover-configurator)



## 硬件&系统

- 主板：`GIGABYTE B250M-D3H`
- CPU：`Intel i5 7500`
- 显卡：`Intel HD Graphics 630`
- 声卡：`ALC892`
- 镜像： `macOS Catelina 10.15.4 (19E287)`
- Clover：`v5116` 
- 机型： `iMac18,1`、`MacMini2018,1`



## 驱动情况

- HD630 核显使用 `WhateverGreen-1.3.8_t3.kext` 正常
- 声卡使用 `AppleALC-1.3.9.kext` 输出、输入正常，注入 `ID = 97` (其他尝试可用：3，13，99)
- 网卡使用 `AppleIntelE1000e.kext` 正常
- `USB` 使用 `USBInjectAll_v0.7.4.kext` 正常识别 `USB3.0`,`USB2.0`
- 休眠正常
- 使用 `DIV` 接口正常，`HDMI`、`DP` 待测试
- 加入了 `ACPISensors.kext`、`CPUSensors.kext`、`GPUSensors.kext` 传感器，可删除不影响



## 已知问题

* 无法睡眠，临时解决方法使用以下命令防止进入睡眠模式：
  ``` bash
  sudo pmset -a disablesleep 1
  ```



## 解决双系统时间不同步

在安装了 windows 与 macOS 两个系统时，windows 的时间会慢 8 个小时，使用以下命令让 Windows 把硬件时间当作 UTC 时间。使用管理员启动命令行：
``` bash
Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1
```


## 注入三码使正常使用 Apple 服务

在正确注入有效三码后，可以正常使用 `iMessage`、`FaceTime`、`Siri` 和 `App Store` 。使用 `iMessageDebug` 工具可读取本机三码。

1. 这里将 `iMessageDebug` 工具放到桌面
2. 终端命令给 `iMessageDebug` 执行权限 `sudo chmod a+x /Users/admin/Desktop/iMessageDebug`
3. 双击 `iMessageDebug` 执行，将在终端打印出三码内容，按 y 还可以将三码内容保存到桌面文件中
4. 使用 `Clover Configurator` 编辑 `config.plist` 文件
5. 在三码内容中找到 `ROM` 填写到 `Rt Variables` -> `ROM`
6. 在三码内容中找到 `BoardSerialNumber` 填写到 `Rt Variables`(变量设置) -> `MLB`
7. 在三码内容中找到 `SerialNumber` 填写到 `SMBIOS`(机型设置) -> `Serial Number`
8. 在三码内容中找到 `BoardSerialNumber` 填写到 `SMBIOS`(机型设置) -> `Board Serial Number`
9. 在三码内容中找到 `Hardware UUID` 填写到 `SMBIOS`(机型设置) -> `SmUUID`
10. 在三码内容中找到 `System-ID` 填写到 `System Parameters`(系统参数) -> `Custom UUID`
11. 最后勾选 `Inject System ID`

![变量设置](./Screenshot/System-ID/RtVariables.png)
![机型设置](./Screenshot/System-ID/SMBIOS.png)
![系统参数](./Screenshot/System-ID/SystemParameters.png)



## 开启 HiDPI
- 开启 HiDPI
``` bash
sudo defaults write /Library/Preferences/com.apple.windowserver.plist DisplayResolutionEnabled -bool true
```
需要重启电脑生效。
- 关闭 HiDPI
``` bash
sudo defaults delete /Library/Preferences/com.apple.windowserver.plist DisplayResolutionEnabled
```
需要重启电脑生效。
- 安装 RDM 工具，方便设置分辨率，分辨率后带闪电标志的，表示支持 HiDPI 显示 
http://avi.alkalay.net/software/RDM/



## 第三方 SSD 固态硬盘开启 TRIM
开启TRIM能让 SSD 在长期使用中有更长的使用寿命及更快的速度。使用终端命令：
``` bash
sudo trimforce enable
```
![开启 TRIM](./Screenshot/TRIM.png)


## Clover Configurator
![ACPI](./Screenshot/CloverConfigurator/1.png)
![引导参数](./Screenshot/CloverConfigurator/2.png)
![设备设置](./Screenshot/CloverConfigurator/3.png)
![禁用驱动](./Screenshot/CloverConfigurator/4.png)
Tips:这里在 Clover 引导界面隐藏了 `Recovery`、`Windows`启动盘符，如果需要显示请删除。
![引导界面](./Screenshot/CloverConfigurator/5.png)
![显卡设置](./Screenshot/CloverConfigurator/6.png)
![内核和驱动补丁](./Screenshot/CloverConfigurator/7.png)
![机型参数](./Screenshot/CloverConfigurator/8.png)
![系统参数](./Screenshot/CloverConfigurator/9.png)


[返回顶部](#)