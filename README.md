# CH32V307VCT6 裸机开发示例集

基于沁恒微电子 CH32V307VCT6 RISC-V 微控制器的裸机开发示例工程集合。

## 硬件平台

- **MCU**: CH32V307VCT6
- **内核**: QingKe V4F RISC-V 32位处理器
- **主频**: 144MHz
- **Flash**: 256KB
- **SRAM**: 64KB
- **封装**: LQFP100

## 软件架构

```
├── Core/           # RISC-V 内核支持文件
├── Debug/          # 调试支持
├── Peripheral/     # 标准外设库
│   ├── inc/        # 外设头文件
│   └── src/        # 外设源文件
├── Startup/        # 启动文件
├── User/           # 用户应用层
│   ├── main.c      # 主程序入口
│   ├── config.c/h  # 配置文件
│   └── ch32v30x_it.c/h  # 中断服务程序
├── myApp/          # 应用层模块
│   ├── scheduler/  # 协作式调度器
│   ├── sensor/     # 传感器算法(AHRS)
│   └── ui/         # 用户界面
└── myDriver/       # 驱动层
    ├── Gpio/       # GPIO驱动
    ├── Key/        # 按键驱动
    ├── LCD/        # LCD显示驱动
    ├── Oled/       # OLED显示驱动
    ├── Adc/        # ADC驱动
    ├── Mpu6050/    # MPU6050 IMU驱动
    ├── Tim/        # 定时器驱动
    ├── Rtc/        # RTC驱动
    └── System/     # 系统时钟驱动
```

## 示例列表

| 编号 | 示例名称 | 功能描述 | 关键技术 |
|------|---------|---------|---------|
| 001 | led_NoRTOS | LED闪烁 | GPIO输出、SysTick定时、协作式调度器 |
| 002 | gpio_NoRTOS | GPIO输入输出 | GPIO配置、推挽输出、上下拉输入 |
| 003 | key_NoRTOS | 按键检测 | GPIO输入、按键消抖、外部中断EXTI |
| 004 | oled_iic_sw_NoRTOS | OLED显示(软件I2C) | 软件模拟I2C、OLED驱动、字符显示 |
| 005 | lcd_spi_hw_NoRTOS | LCD显示(硬件SPI) | 硬件SPI驱动、LCD显示、ASCII字体 |
| 006 | mpu6050_ahrs_NoRTOS | MPU6050姿态解算 | I2C通信、IMU数据读取、AHRS姿态融合算法 |
| 007 | adc_dma_NoRTOS | ADC+DMA采集 | ADC多通道采集、DMA传输、传感器数据融合 |
| 008 | lcdsprintf_spi_hw_NoRTOS | LCD格式化显示 | sprintf格式化、LCD显示优化 |
| 009 | rtc_ui_NoRTOS | RTC时钟界面 | RTC实时时钟、UI界面设计 |
| 010 | tim_freqCapture_NoRTOS_error | 定时器频率捕获 | 定时器输入捕获、频率测量(调试中) |
| 011 | adc_Waveform_NoRTOS | ADC波形显示 | ADC采集、波形绘制、示波器效果 |

## 开发环境

### 推荐IDE
- **MounRiver Studio** (官方推荐)
- **Eclipse CDT** (需配置RISC-V工具链)

### 工具链
- RISC-V GCC 工具链
- OpenOCD 调试工具

### 硬件调试器
- WCH-Link (官方调试器)
- WCH-LinkE (经济版)

## 快速开始

### 1. 克隆仓库
```bash
git clone <repository-url>
```

### 2. 导入工程
1. 打开 MounRiver Studio
2. File → Import → Existing Projects into Workspace
3. 选择对应的示例目录（如 `001_led_NoRTOS`）
4. 点击 Finish

### 3. 编译下载
1. 连接 WCH-Link 到开发板
2. 右键工程 → Build Project
3. 右键工程 → Run As → MounRiver Firmware Download

### 4. 调试
1. 右键工程 → Debug As → MounRiver Firmware Debug
2. 使用GDB调试器进行断点调试

## 核心特性

### 协作式调度器
项目实现了轻量级协作式调度器，支持：
- 基于SysTick的时基
- 任务注册与调度
- 非阻塞式任务切换
- 低CPU占用率

### 驱动分层架构
```
应用层 (myApp)
    ↓
驱动层 (myDriver)
    ↓
外设库 (Peripheral)
    ↓
硬件抽象层 (Core)
```

### AHRS姿态解算
集成开源Fusion算法库，支持：
- 四元数姿态计算
- 陀螺仪零偏校准
- 磁力计校准
- 欧拉角输出

## 外设驱动说明

### GPIO驱动
- 支持输入/输出配置
- 支持上拉/下拉配置
- 支持推挽/开漏输出
- 支持GPIO翻转操作

### SPI驱动
- 支持硬件SPI配置
- 可配置时钟极性和相位
- 支持DMA传输模式
- 最高18MHz时钟

### I2C驱动
- 软件模拟I2C实现
- 支持标准模式(100kHz)
- 支持快速模式(400kHz)
- 兼容硬件I2C接口

### ADC驱动
- 支持多通道采集
- 支持DMA传输
- 12位分辨率
- 支持连续转换模式

### 定时器驱动
- 支持输入捕获
- 支持PWM输出
- 支持编码器接口
- 支持频率测量

## 注意事项

1. **时钟配置**: 默认使用内部8MHz晶振，通过PLL倍频至144MHz
2. **中断优先级**: 使用NVIC优先级组2，支持4级抢占优先级
3. **调试输出**: USART1 (PA9) 用于printf调试输出，波特率115200
4. **内存管理**: 禁止动态内存分配，所有变量使用静态分配

## 已知问题

- **010_tim_freqCapture_NoRTOS_error**: 定时器频率捕获功能存在bug，正在调试中

## 参考资料

- [CH32FV2x_V3x参考手册](https://www.wch.cn)
- [沁恒微电子官网](https://www.wch.cn)
- [MounRiver Studio下载](http://www.mounriver.com)

## 许可证

本项目基于 WCH 官方示例代码修改，遵循相关许可协议。

## 贡献

欢迎提交 Issue 和 Pull Request。

## 更新日志

- 2024-12: 初始版本，包含11个基础示例
