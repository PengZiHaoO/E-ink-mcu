# E-ink-mcu

基于 **STM32U031F8Px**（Cortex-M0+，TSSOP20）的电子纸（E-ink）显示设备固件。

## 硬件外设

| 外设 | 引脚 | 用途 |
|---|---|---|
| SPI1 | PA1 (SCK) / PA12 (MOSI) | 电子纸 4 线 SPI |
| I2C2 | PA6 (SDA) / PA7 (SCL) | 传感器 / RTC |
| GPIO | PA8 `EPD_CS`、PA11 `EPD_DC` | 电子纸控制 |
| GPIO | PB1 `EBD_BUSY`（输入） | 电子纸忙信号 |
| GPIO | PA0 `NFCSTA`（WKUP1） | NFC 状态 / 唤醒 |

- 主频 16 MHz（HSI 直出，未启用 PLL）
- 驱动层使用 STM32 LL 库，无 HAL 高层 API

## 构建

依赖：CMake ≥ 3.22、Ninja、`arm-none-eabi-gcc`。

```bash
cmake --preset Debug
cmake --build --preset Debug
```

产物位于 `build/<Preset>/`：

- `magtime.elf` — 带调试符号的可执行文件
- `magtime.hex` — Intel HEX
- `magtime.bin` — 裸 Flash 镜像
- `magtime.map` — 链接映射

Release 构建使用 `--preset Release`（`-Os`）。

## 目录结构

```
Core/       应用源码（CubeMX USER CODE 区）
Drivers/    STM32 CMSIS + LL 驱动（勿修改）
cmake/      工具链与 CubeMX 生成的构建清单
magtime.ioc STM32CubeMX 工程配置
```

> 本工程由 STM32CubeMX 生成，修改外设配置请通过 `magtime.ioc` 重新生成，业务代码只写在 `USER CODE` 区或新增的 `Core/Src` 模块中。
