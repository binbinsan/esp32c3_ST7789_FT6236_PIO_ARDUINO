# ESP32 Smart Clock with MQTT Control

## 功能概述

这是一个基于 ESP32 的智能时钟项目，集成了以下功能：
- 高精度时间显示（支持12/24小时制切换）
- 温湿度监测（SHT31传感器）
- WiFi 连接和管理
- MQTT 远程控制
- GPIO 状态监控和控制
- 触摸屏交互界面
- 多页面导航系统

## 硬件要求

- ESP32C3/ESP32S3 开发板
- ST7789 显示屏 (240x240)
- FT6236 触摸控制器
- SHT31 温湿度传感器
- 其他配件：
  - GPIO 控制设备（可选）
  - LED 灯（用于亮度控制，可选）

## 引脚配置

### ESP32C3
- I2C SDA: GPIO4
- I2C SCL: GPIO5
- GPIO控制引脚：
  - GPIO0
  - GPIO1
  - GPIO2
  - GPIO3 (PWM亮度控制)

#### TFT SPI接口 (ESP32C3)
#define TFT_SCLK 2
#define TFT_MOSI 3   // 修改为3，因为11可能有问题
#define TFT_RST  10
#define TFT_DC   6
#define TFT_CS   7
#define TFT_MISO -1  // 如果不需要读取显示屏，设为-1
#define TFT_BL   9   // 修改为有效的GPIO



### 注意事项
- TFT显示屏使用SPI通信
- 背光控制使用PWM调节亮度
- 请确保TFT_eSPI库中的引脚配置与实际接线一致
- 建议使用屏蔽线连接SPI信号线，减少干扰

## 软件功能

### 1. 时间显示
- 实时显示时间和日期
- 支持12/24小时制切换（双击时间进行切换）
- NTP网络时间自动同步
- 自动根据时间调整背景颜色（日间/夜间模式）

### 2. 温湿度监测
- 实时显示温度和湿度数据
- 每5秒自动更新一次
- 支持摄氏度显示

### 3. WiFi管理
- 使用WiFiManager进行WiFi配置
- 支持WiFi扫描功能
- 显示详细的WiFi连接信息
- 支持WiFi重置功能

### 4. MQTT控制
- 支持MQTT服务器配置
- GPIO远程控制
- 亮度远程调节
- 实时状态反馈
- 自动重连机制

### 5. GPIO控制
- 3个可控GPIO接口
- 实时状态显示
- 支持本地触摸控制和远程MQTT控制
- PWM亮度调节

### 6. 界面导航
- 多页面系统：
  - 主页面（时钟显示）
  - WiFi信息页面
  - GPIO控制页面
  - WiFi扫描页面
  - MQTT控制页面
- 支持左右滑动切换页面
- 页面切换动画效果

## 使用说明

### 首次使用
1. 给设备上电
2. 设备将创建一个名为"SmartClockAP"的WiFi热点
3. 使用手机或电脑连接该热点
4. 在弹出的配置页面中：
   - 选择并配置WiFi连接
   - 配置MQTT服务器信息（可选）
   - 设置其他参数

### 页面操作
- 左右滑动：切换不同功能页面
- 双击时间：切换12/24小时制
- 点击GPIO按钮：切换对应GPIO状态
- 使用亮度滑块：调节显示亮度

### MQTT主题
设备支持以下MQTT主题（默认前缀：smartclock/）：
- `smartclock/gpio/0`：控制GPIO0（0/1）
- `smartclock/gpio/1`：控制GPIO1（0/1）
- `smartclock/gpio/2`：控制GPIO2（0/1）
- `smartclock/brightness`：控制亮度（0-255）

### WiFi重置
如需重置WiFi设置：
1. 进入WiFi扫描页面
2. 点击"RESET"按钮
3. 设备将重启并返回配置模式

## 故障排除

### 设备无法连接WiFi
- 检查WiFi密码是否正确
- 确保WiFi信号强度足够
- 尝试重置WiFi设置

### 温湿度显示异常
- 检查SHT31传感器连接
- 确保I2C地址设置正确（默认0x45）
- 检查传感器是否工作正常

### MQTT连接问题
- 验证MQTT服务器地址和端口
- 检查用户名和密码是否正确
- 确认网络连接状态
- 查看MQTT主题配置

### 触摸屏无响应
- 检查FT6236连接
- 验证I2C地址设置
- 调整触摸灵敏度参数

## 开发信息

### 依赖库
- LVGL：用于GUI界面
- TFT_eSPI：显示屏驱动
- FT6236：触摸控制
- WiFiManager：WiFi配置管理
- PubSubClient：MQTT客户端
- Adafruit_SHT31：温湿度传感器驱动

### 代码结构
- 页面管理：使用PageManager类
- 事件处理：基于LVGL事件系统
- 状态更新：使用定时器管理
- 配置存储：使用WiFiManager

## 注意事项

1. 首次使用需要进行WiFi配置
2. MQTT功能为可选，可以不配置MQTT服务器
3. GPIO使用前请确认引脚功能，避免冲突
4. 建议使用5V-2A以上电源供电
5. 避免长时间暴露在高温环境下

## 更新日志

### v1.0.0
- 初始版本发布
- 基本功能实现
- 多页面系统
- MQTT控制支持

## 许可证

本项目采用 MIT 许可证。详见 LICENSE 文件。 