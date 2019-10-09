## Hackintosh Build: iMac 4K 2019

|hardware|spec|
|-|-|
|系统| MacOS 10.15 + Windows 10 |
|处理器| Intel Core i3-8100 @ 3.6GHz |
|主板| MSI B360m Fire |
|内存| Corsair 8*2G DDR4-2400 |
|硬盘| Intel 760P 256G NVMe + WD 1T HDD |
|显卡| Yeston Radeon RX570 4G GDDR5|
|~~无线网络~~| ~~CF-915AC USB WIFI~~ |
|网络| 网线 |
|声卡| Realtek® ALC887 Codec |
|BIOS| 7B53v14 |
|CLOVER| r5070 |

- 现状：目前声卡、网卡、硬解、睡眠、HWMonitor温度检测均正常。
- 鱼：基于8代intel平台搭建，很新的Clover和kexts驱动，支持**直接升级macos10.14.6/10.15**。
- 渔：基于tonymac的install guide，使用unibeast和multibeast构建。
  - multibeast原生会把kext安装到/L/E下面，为了保持kexts的稳定性，我建议把所有驱动搬迁到/EFI/CLOVER/Kexts/$(version)下面去，让驱动和引导放在一起；
  - 调整kexts后最好重建下kext缓存，以免开机起不来之类的；操作指令`sudo kextcache -i /`。网上有很多用Kext Utility操作的，我看日志应该和kextcache操作一样。
- 使用建议：没有做集成显卡id的注入，没有做ssdt/dsdt，定制少。相似硬件的朋友可以参考使用(需先按照guide设置好BIOS)，**使用的时候请在CCG中换一个SMBIOS再使用**，否则会因为机器有同一个硬件ID而被苹果封锁。

## update log

#### 20191009：升级到10.15
  - 使用Clover Configurator升级Clover版本从4934到5070.
  - 使用Clover Configurator升级AppleALC.kext、WhateverGreen.kext到最新
  - 参考：https://www.bilibili.com/video/av49751074?t=72 
  - 系统更新升级到最新系统

#### 20190824：升级到10.14.6
  - 升级了3个kext驱动，直接系统更新到10.14.6

#### 20190617：升级usb wifi网卡驱动
  - CF-915AC新版本的驱动都无法稳定支持5G，请使用`mac_utils/EW-7811Un_Mac_driver_v1.0.1.4.zip`
  - CF-811AC新版的驱动都可以用，更小更便宜，请使用`mac_utils/RTLWlanU_MacOS10.9_MacOS10.14_Driver_1830.20.b31_1827.4.b32_UI_5.0.8.b4.zip`，另[官方下载地址](http://www.comfast.cn/index.php?m=content&c=index&a=show&catid=30&id=335)

#### 20190610：升级clover，主题精简
  - 使用multibeast-11.3.0升级clover到r4934
  - 新增thinkpad e450/t450 Laptop的EFI，见t450分支
  - 主题仅保留一个最简的Minimalism主题

![](EFI/CLOVER/themes/Minimalism/screenshot.png)

#### 20190519：新增硬件检测
  - 新增Apps文件夹，存放硬件检测软件
  - 新增HWMonitor和相关的kexts，主要是为了看温度
  - CPU-S, intel(R) Power Gadge查看系统cpu变频之类的
  - VideoProc查看硬件解码，通过
  - fliqlo为流行的时钟主题
  - RTLWlanU_*_Driver为USB WIFI的驱动。

#### 20190519：直接升级到mojave10.14.5
  - 基于multibeast 11.2.0构建，clover版本为r4920，支持macos10.14.5；
  - kexts都已更新；
  - 网络问题导致直接安装报错，于是下载macos combo更新来直接安装，基本无人值守可以自行安装完成；
  - 更新细节：更新中会有一个新的 `Install MAC` clover boot选项，更新中会自动进入这个2次吧，最后这个选项消失，更新就完成了；然后CLover boot选项会变得和之前一样。

#### 20190517：修复无法预览jpg图片错误
预览要集成显卡的硬解功能
  - 我的i3-8100有集成显卡的，于是在bios里面开启igpu就可以了。
  - 网上的教程是把smbios改成不带集成显卡的机器，然后用NoVPAJpeg.kext来fix。

#### 20190517：修复shutdown reboots(关机重启)错误
  - BIOS降级: 请使用7B53v14或者13版本的bios，再高会出现问题，亲测。
  - 进入/EFI/CLOVER/drivers64UEFI；删除AptioMemoryFix-64.efi，添加OsxAptioFix2Drv-free2000.efi 和 EmuVariableUefi-64.efi；同时修改config.plist的boot模块添加slide=0。
