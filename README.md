# OpenLapse

OpenLapse is a DIY, low-cost, open-source in-incubator time-lapse imaging platform that combines 3D-printed mechanics, custom PCBs, and Python control software on both a Raspberry Pi and a desktop GUI.

## English

### Overview
- Reproducible, open-source platform for live-cell and embryo time-lapse imaging inside standard incubators.
- Modular hardware (controller PCB, illumination PCB, 3D-printed frame) plus two software layers: Raspberry Pi control (embedded) and desktop GUI (controller).
- Supports bright-field and oblique illumination and multiple well-plate formats (6/12/24/96).

### System Architecture
- PC GUI (OpenLapse-Controller) orchestrates experiment setup, well selection, scheduling, and monitoring.
- Raspberry Pi (OpenLapse-Embedded) drives motion, illumination, focus, and image capture/streaming.
- Communication over LAN: UDP broadcast for discovery/heartbeat, TCP for commands and acknowledgements.

```
PC (Controller) ⇄ TCP commands + preview ⇄ Raspberry Pi (Embedded)
                                 ⇵ UDP discovery/heartbeat
                          Motors / LEDs / Camera inside incubator
```

### Repository Structure
- [Controller/](Controller/) – Desktop GUI (Python + CustomTkinter); entry point [Controller/app.py](Controller/app.py); helper modules in [Controller/libs/](Controller/libs/); plate backgrounds in [Controller/plates/](Controller/plates/).
- [Raspi/](Raspi/) – Raspberry Pi embedded control software; entry point [Raspi/app.py](Raspi/app.py); device drivers and controllers in [Raspi/libs/](Raspi/libs/); configuration in [Raspi/config.py](Raspi/config.py); supplemental notes in [Raspi/doc/](Raspi/doc/).
- [PCB/](PCB/) – Controller board and illumination array board design/manufacturing files (Gerber, BOM, etc.).
- [STL_files/](STL_files/) – 3D-printable mechanical parts; use provided STLs (and optional slicing files if present) to fabricate the frame and fixtures.
- [imgs/](imgs/) – Photos, renders, and assembly/usage illustrations for documentation.

### Quick Start (print → assemble → install → run)
1) **Print**: Fabricate mechanical parts from [STL_files/](STL_files/) using PLA/PETG per your incubator environment; verify tolerances for motion axes.
2) **PCBs**: Order/fabricate controller and LED array boards from [PCB/](PCB/); gather BOM components and headers; follow hardware images in [imgs/](imgs/).
3) **Assemble**: Mount steppers, camera, and LED array to the printed frame; wire motors, UART, and LED data to the PCB; ensure stable 5 V supply; place the assembly in the incubator.
4) **Install Software**:
   - Desktop GUI: Python 3.9+, install dependencies from [Controller/README.md](Controller/README.md), then run [Controller/app.py](Controller/app.py).
   - Raspberry Pi: Raspberry Pi OS Lite, enable camera and UART, install dependencies from [Raspi/README.md](Raspi/README.md), then run [Raspi/app.py](Raspi/app.py).
5) **Run**: Connect PC and Pi to the same LAN; start the Pi service, then launch the GUI, auto-discover/connect, home axes, configure wells/illumination, and start time-lapse acquisition.

### Hardware
- **PCBs**: Controller board for TMC2209 stepper drivers, UART/GPIO, and LED array control; illumination array board (WS2812-based). Manufacturing files and BOMs are under [PCB/](PCB/). Verify motor current, stallguard homing, and thermal management inside incubators.
- **3D Printing**: Mechanical STLs in [STL_files/](STL_files/) for stage, camera mount, and structural parts; print with sufficient rigidity and heat tolerance. Optional slicing/3MF files may be included alongside STLs when available.

### Software
- **OpenLapse-Controller (Desktop)**: Python GUI for experiment setup, preview, manual XYZ control, well selection, scheduling, lighting, and autofocus. See [Controller/app.py](Controller/app.py) and supporting modules in [Controller/libs/](Controller/libs/).
- **OpenLapse-Embedded (Raspberry Pi)**: Headless control loop for motion, camera capture/streaming, illumination, and network commands. See [Raspi/app.py](Raspi/app.py), drivers in [Raspi/libs/](Raspi/libs/), and configuration in [Raspi/config.py](Raspi/config.py).

### Documentation and Images
- Photos, diagrams, and assembly references are in [imgs/](imgs/).
- Additional device notes and driver references reside in [Raspi/doc/](Raspi/doc/).

### Licensing Overview
- Hardware (PCBs, mechanical designs) is released under CERN Open Hardware License v2 Strongly Reciprocal (CERN-OHL-S-2.0). If you distribute modified hardware designs or manufactured hardware, you must also share the corresponding design source under the same license.
- Software (controller + embedded code) is released under the MIT License. You may use, modify, and redistribute the software, including in proprietary products, provided you keep the copyright notice and license terms.

### License
- **Hardware scope**: PCB design files (Controller Board, Illumination Array Board) and 3D models (STL/printing assets) are covered by CERN-OHL-S-2.0. See the full text: https://ohwr.org/cern_ohl_s_v2.txt
- **Software scope**: OpenLapse-Controller (desktop GUI) and OpenLapse-Embedded (Raspberry Pi backend) are covered by the MIT License. See the full text: https://opensource.org/license/mit
- **Reuse encouragement**: Please fork, adapt, and iterate. For hardware derivatives, publish your design source; for software, retain the copyright and license notice.

### Citation / Attribution
- For academic use, please cite the associated JoVE article (link forthcoming) and reference this repository (OpenLapse). Attribution helps trace reproducibility and improvements.

### Reproducibility & Open Science
- Designs, code, and documentation are open to enable replication and modification. Contributions should include build parameters, calibration steps, and configuration changes to help others reproduce results.

### Publication
- Associated Journal of Visualized Experiments (JoVE) article: add link here when available.

### Contributing
- Issues and pull requests are welcome. Please include reproducible build/assembly notes, calibration steps, and configuration changes so others can replicate results.

---

## 中文

### 项目概述
- 开源、低成本的培养箱内长时程成像平台，整合自制 PCB、3D 打印结构和树莓派 + 桌面端的 Python 控制软件。
- 支持多孔板格式（6/12/24/96），提供明场与斜射照明，可在培养箱内长期采集。

### 系统架构
- 桌面端 GUI（OpenLapse-Controller）负责实验配置、孔板选择、调度与监控。
- 树莓派端（OpenLapse-Embedded）负责电机、光源、对焦与图像采集/推流。
- 局域网通信：UDP 广播用于发现与心跳，TCP 传输命令与确认。

```
PC（上位机） ⇄ TCP 命令/预览 ⇄ 树莓派（嵌入端）
                          ⇵ UDP 发现与心跳
                 电机 / LED / 相机（位于培养箱内）
```

### 仓库结构
- [Controller/](Controller/) – 桌面端 GUI（Python + CustomTkinter）；入口 [Controller/app.py](Controller/app.py)；功能模块位于 [Controller/libs/](Controller/libs/)；孔板背景在 [Controller/plates/](Controller/plates/)。
- [Raspi/](Raspi/) – 树莓派嵌入式控制软件；入口 [Raspi/app.py](Raspi/app.py)；驱动与控制在 [Raspi/libs/](Raspi/libs/)；配置在 [Raspi/config.py](Raspi/config.py)；补充文档在 [Raspi/doc/](Raspi/doc/)。
- [PCB/](PCB/) – 控制板与照明阵列板的设计与生产文件（Gerber、BOM 等）。
- [STL_files/](STL_files/) – 3D 打印零件（结构、相机/电机安装件等）；如有附带切片文件可直接使用。
- [imgs/](imgs/) – 照片、渲染与装配示意图。

### 快速开始（打印 → 装配 → 安装 → 运行）
1) **打印**：使用 [STL_files/](STL_files/) 中的模型打印结构件（建议 PLA/PETG，依据培养箱环境选择），确认运动部件配合公差。
2) **制板**：从 [PCB/](PCB/) 获取 Gerber/BOM 进行下单，准备元件与接插件，结合 [imgs/](imgs/) 进行焊接与检查。
3) **装配**：安装步进电机、相机与 LED 阵列至打印结构，连接电机、UART 与 LED 数据线，确保稳定 5 V 供电后放入培养箱。
4) **安装软件**：
   - 桌面端：按 [Controller/README.md](Controller/README.md) 安装 Python 依赖，运行 [Controller/app.py](Controller/app.py)。
   - 树莓派：按 [Raspi/README.md](Raspi/README.md) 配置系统与依赖，运行 [Raspi/app.py](Raspi/app.py)。
5) **运行**：让 PC 与树莓派处于同一局域网，先启动树莓派服务，再启动 GUI，自动发现并连接后完成回零、孔板/光照配置并开始时间序列采集。

### 硬件
- **PCB**：控制板集成 TMC2209 步进驱动、UART/GPIO 与 LED 阵列控制；照明阵列板基于 WS2812。生产文件在 [PCB/](PCB/)；请在培养箱环境下关注电流设定、stallguard 归零与散热。
- **3D 打印**：结构与安装件位于 [STL_files/](STL_files/)；选择足够刚性的材料并考虑温度耐受性；如有提供切片文件可直接使用。

### 软件
- **OpenLapse-Controller（桌面端）**：实验配置、预览、手动 XYZ 控制、孔板选择、调度、光源与自动对焦。参考 [Controller/app.py](Controller/app.py) 及 [Controller/libs/](Controller/libs/)。
- **OpenLapse-Embedded（树莓派）**：电机、相机采集/推流、照明与网络命令处理。参考 [Raspi/app.py](Raspi/app.py)、[Raspi/libs/](Raspi/libs/) 以及 [Raspi/config.py](Raspi/config.py)。

### 文档与图片
- 装配与使用图片位于 [imgs/](imgs/)。
- 设备说明与驱动参考在 [Raspi/doc/](Raspi/doc/)。

### 许可证
- **硬件**（PCB、机械设计）：CERN-OHL-S-2.0。
- **软件**（上位机与嵌入式代码）：MIT License。

### 论文
- 相关 JoVE 论文：发布后在此添加链接。

### 贡献
- 欢迎提交 Issue/PR，并附带可复现的装配、校准与配置说明，便于社区复现与改进。
