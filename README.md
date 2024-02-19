# Sunniwell RK3568 Laoptop Z96A

朝歌科技(Sunniwell)出品的云笔记本电脑(Cloud laptop)，业界简称**云笔电**。

本质上是为云服务供应商设计的云电脑服务瘦终端，但区别于以前的瘦终端盒子，直接做成了笔记本形态。

类似的产品还有云电脑一体机，参考天翼云电脑[终端适配清单](https://gdoss.xstore.ctyun.cn/ctyun-it-0727/help/754947050092864512.xlsx)

## 设备信息 | Device Information
- Model: Z96A
- CPU: Rockchip RK3568B2 4 x 2.0GHz ARM Cortex-A55
  - GPU: Mail-G52 1-Core-2EE， H.264 4K@60fps，H.265 4K@60fps
  - NPU: 1TOPs
- PMIC/SOUND/CODEC: Rockchip RK817
- Memory: 4GB LPDDR4X (Maxium support 8GB, verified)
- Storage: 32GB eMMC 5.1
- Power: DC 12V 2A （3.5×1.35mm）
- Display
  - Built-in eDP with a 1920x1080 panel from BOE, model NV140FHM-N48 (3.3V，VP1@eDP0，backlight@PWM10)
  - mini HDMI 2.0(Max 4K@60Hz)
- Baterry: SHT 3585130-2S，5000mAh/37Wh，Voltage 8.4V
- Charger Management: CN3705
- WiFi/BT: LB-Link BL-M8821CS1
  - WiFi: RTL8821CS，SDIO2.0，1T1R 802.11a/b/g/n/ac WiFi，433Mbps
  - BT: 3-Wire HCIUART(USRT1) BT4.2
- USB: 1xUSB3.0 Type-A + 1xUSB2.0 Type-A + 1xUSB Type-C OTG
  - FE1.1s USB 2.0 HUB
  - Type-C Controller: HUSB311 or FUSB302
  - AU3841 USB2.0 Web camera
  - Built-in USB2.0 Touchpad
- Keyboard Controller: SH61F83Q(A 51 MCU)

## 相关参考 | Related Reference

GeekBench5, original 4GB RAM + 32GB EMMC
[Score](https://browser.geekbench.com/v5/cpu/21264096)

GeekBench6, Bingo's modified hardware, 8GB RAM + 128GB EMMC
[Score](https://browser.geekbench.com/v6/compute/1584178)

Linux下使能Panfrost驱动后，glmark2-es2-wayland跑分可以到470+

## 补充说明 | Note

从原机自带的Android 11固件(4.19内核)提取了dtb，反编译出来，并对照其他设备的dts，还原出来了[原始dts](https://github.com/bingo1991/RK3568_Sunniwell_Laptop_Z96A/blob/main/rk3568-z96a.dts)。

中间耗费的时间、精力比想象的多，但如今看来还是值得的，该dts目前逐步优化已经可以在5.10内核中正常编译使用。

Working：
- eDP Display
- Keyboard
- Touchpad
- RK817 sound card
- RTL8821CS SDIO WiFi
- RTL8211CS UART BT
- Panfrost GPU driver
- HDMI output
- USB Web camera
- Hall sensor for lid open/close
- Battery
  - 该设备的锂电池电压为8.4V，不支持用 RK817 管理充电，而是使用了免管理的 CN3705 芯片，这是与一众 rk3566 + rk817 方案的平板电脑不同的地方。
  - 电池信息获取同样没有使用 RK817，厂家使用了 ADC 的 channel 1来采集电压，并修改了 RK817 的电池管理驱动代码来呈现信息。
  - 由于我们没有厂家修改后的代码，只好群策群力，重新手搓了一段补丁代码，解决了电池信息显示的问题。

Not working yet：
- None, everything works fine by now

