## 详细原理

当前的 OLED 屏幕具有独立的亮度控制输入，用于控制在最高信号电平输入下的亮度。因此，如果我们想要显示亮度为 20 cd/m<sup>2</sup> 的白色，以下两种方式都可以实现：

  1. 调整亮度控制输入，使信号输入电平最高时的亮度为 20 cd/m<sup>2</sup>，然后输入最高电平信号（“白色”）；
  2. 在当前具有更高的亮度控制输入时，比如 160 cd/m<sup>2</sup>，输入一个较低的信号电平（“灰色”），产生 12.5% 最高亮度（20 cd/m<sup>2</sup>）的输出。

部分 OLED 屏幕在第二种工作情况下会使用更高的 PWM 频率/占空比，一些用户认为这有助于减轻长时间或在黑暗环境下观看屏幕时的不适感。

信号电平与输出亮度之间不是线性关系，因此，第二种方式并不是简单的将输入信号乘以一个系数。但基于以下前提，我们可以实现精确的控制：

  1. Android 系统假定屏幕具有 sRGB 响应曲线；
  2. 基于 sRGB 响应曲线，系统框架及 HAL 提供在线性空间中的 RGB 矩阵变换；
  3. 自 Android 12 开始，系统框架及 HAL 接口支持以 cd/m<sup>2</sup>（或称为 nits）为单位的绝对亮度控制。