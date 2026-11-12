# HB Roaster & Artisan 无线桥接中间件 (Wireless Bridge) ☕️

<p align="center">
  <a href="README_EN.md">English Documentation</a> • <a href="README.md">中文说明</a> • <a href="https://github.com/koldllc/HB-Roaster-Wireless-Bridge/releases">公开版本下载 (Releases)</a>
</p>

无需连接物理 USB 串口线，让 **Artisan 烘焙软件** 通过局域网 Wi-Fi 无线连接 **HB 咖啡烘焙机**（全面支持 HB Model S 系列：M2S / M6S / L2S / L6S 以及 HB Standard 系列）。

> **💡 项目目标**：告别烘焙机与电脑之间的有线连接束缚，100% 沿用你原先在 Artisan 调校好的曲线颜色、四通道配置、事件与报警规则，实现开机即连、平滑无缝的无线烘焙体验。

---

## 🌟 核心运行模式

本项目提供 **macOS 原生菜单栏 App** 与 **Windows 原生桌面 App**，开箱即用，无需打开终端、无需预装 Python 环境！

### 🍎 macOS 原生菜单栏应用 (`HB Roaster Wireless Bridge.app`)
- ☕️ **菜单栏常驻图标**：在顶部菜单栏显示烘焙状态极简火苗图标，支持实时显示 `BT / ET` 温度（如 `162° / 183°`）。
- 🖥️ **精美弹出面板 (Popover)**：展开现代稳重低饱和度表盘，实时显示四路温度、Artisan 采集状态，支持机身开关安全控制。
- 🔄 **自动在后台静默运行**：无 Dock 栏图标打扰（`LSUIElement`），支持开机自启（`SMAppService`）。
- 🔀 **内置虚拟串口仿真**：自动在 `/tmp/hb_roaster` 建立 Arduino TC4 协议桥梁。

### 🪟 Windows 原生桌面应用 (`HB-Roaster-Wireless-Bridge.exe`)
- 🖥️ **Windows 原生仪表盘**：深色现代仪表盘，呈现四路大字温度与设备连接指示。
- 🔔 **系统托盘驻留**：关闭主界面时自动收拢到右下角通知区域，鼠标悬停即时预览豆温与炉温。
- ⚡ **Artisan 双模式支持**：
  - **模式 1 (推荐 · 免驱动直连)**：Artisan 原生 WebSocket 直连（端点 `ws://127.0.0.1:8088/ws`），无需安装任何虚拟串口驱动！
  - **模式 2 (传统模式 · TC4 兼容)**：支持配合 com0com 虚拟串口对，无缝沿用原有 "HB Model S" 设备配置。
- 🚀 **开机自启**：支持开机静默常驻，双击独立 `.exe` 即开即用。

---

## 📚 完整文档导航

- 🪟 [**Windows 版配置与使用指南**](docs/WINDOWS_SETUP.md)：Windows 下的免驱动 WebSocket 直连与 com0com 虚拟串口配置全解。
- 🚀 [**烘焙实战操作指南 (烘焙前 3 分钟速查)**](docs/QUICKSTART_ROAST_GUIDE.md)：下次烘焙时的标准操作流程（开机、检查、点火、记录）。
- ⚙️ [**Artisan 配置与四路曲线详解**](docs/ARTISAN_SETUP.md)：端口配置、四通道（BT / ET / 出风 / 进风）映射、采样率推荐。
- 🔍 [**常见问题与网络排查指南**](docs/TROUBLESHOOTING.md)：扫不到烘焙机、Loon / Clash 代理冲突、热点直连模式、端口错误解决办法。

---

## 🌟 核心特性与架构

```
+-------------------+                 +--------------------------+                 +-------------------+
|   HB 咖啡烘焙机    |   Wi-Fi (TCP)   |   HB Roaster Wireless Bridge.app  |   Virtual PTY   |   Artisan 软件    |
| (ESP Wi-Fi 芯片)  | <=============> | (macOS 菜单栏原生应用/后台)| <=============> |  (HB Model S 设备) |
|  Port: 16888/TCP  |   (1Hz 4通道)   |   Swift 原生 / 797KB 体积 | /tmp/hb_roaster |   Arduino TC4 协议|
+-------------------+                 +--------------------------+                 +-------------------+
```

1. **零配置迁移（100% 兼容现有 Artisan 设置）**：
   - 软件在 macOS 上创建虚拟串口（PTY，默认路径 `/tmp/hb_roaster`），完全仿真 Artisan 原厂驱动中的 **Arduino TC4** 通信协议（对应设备号 `126 ARC_BTET` 与 `127 ARC_METIT`）。
   - 你在 Artisan 中保存的 `.aset` 配置文件、曲线布局、报警设置、PID 参数均无需重设，只需将端口由物理串口改为 `/tmp/hb_roaster`。

2. **多重发现与 VPN 绕过机制**：
   - 支持 UDP 局域网广播自动发现（端口 `17888`）。
   - **内置 VPN / 代理免疫能力**：针对 macOS 上运行的 Loon、Clash、Surge 等接管路由的代理工具，采用底层 `IP_BOUND_IF` 套接字绑定，确保发现广播直接从物理网卡 `en0` 发出。
   - **记忆上次连接 IP**：自动缓存（如 `192.168.88.88`），开机毫秒级直连。

3. **四路温度 1 Hz 高精度实时采集**：
   - 通道 1：**出风温 / 排烟废气温 (Exhaust / MET)**
   - 通道 2：**进风温 (Inlet / IT)**
   - 通道 3：**豆温 (Bean Temperature / BT)**
   - 通道 4：**炉温 / 鼓风环境温 (Environment Temperature / ET)**

4. **极速轻巧，纯原生实现**：
   - 纯 Swift + SwiftUI 原生编译，整个应用体积仅不到 1 MB，内存占用低于 25 MB。

---

## ⚡ 极速起步（下次烘焙实战）

### 步骤 1：打开烘焙机电源并启动 App

1. 确认烘焙机开启机械电源，屏幕 Wi-Fi 保持连接。
2. 打开 Mac 上的 **HB Roaster Wireless Bridge.app**（在「应用程序」或 Spotlight 聚焦搜索中直接启动）。
3. 你的 macOS 顶部菜单栏将出现一个咖啡杯图标 ☕️：
   - 点击图标展开面板，显示 `已连接 (192.168.88.88)`。
   - 面板四路温度开始实时更新。
   - 可在面板设置中勾选 **「开机自动启动」**，以后开机无需手动打开！

---

### 步骤 2：在 Artisan 中切换端口

1. 打开 **Artisan** 软件。
2. 菜单栏选择 **配置 (Config)** -> **设备 (Device)**：
   - 确认选择的是 **HB Model S**（或你原先用的 HB Standard）。
3. 菜单栏选择 **配置 (Config)** -> **端口 (Ports)**：
   - **Comm Port (串口)**：直接填入虚拟串口路径：
     ```text
     /tmp/hb_roaster
     ```
   - **波特率 (Baud Rate)**：保持 `115200`（虚拟串口任意波特率均通畅）。
   - **参数**：保持默认的 `8` 位、`N` 校验、`1` 停止位。
   - 点击 **OK** 保存。
4. 点击 Artisan 左上角 **ON** 按钮开始采集：
   - 绿色的 BT 曲线、红色的 ET 曲线、进风与出风温度将立即开始实时绘制！
   - 菜单栏面板上的 Artisan 状态将变为绿色的 **「Artisan 采集中 (活跃)」**。

---

## 🛠️ 重新打包 / 编译说明 (针对开发者)

### macOS 平台打包
```bash
# 编译并打包为 HB Roaster Wireless Bridge.app
./build_app.sh

# 编译并直接覆盖安装到 /Applications
./build_app.sh --install
```

### Windows 平台打包
在 Windows 终端中运行：
```cmd
# 运行一键打包脚本（自动安装依赖并生成 .exe 与 zip 发布包）
build_windows.bat

# 或通过 Python 脚本运行打包
python build_windows.py
```
编译生成的单文件绿色版程序将输出至 `dist/HB-Roaster-Wireless-Bridge.exe`。

---

## 💻 备用选项：Python CLI 命令行与系统服务

除 Mac App 外，仓库中依然保留了 Python 版本供命令行或无图形界面环境使用：

```bash
# 命令行前台运行
python3 main.py --ip 192.168.88.88

# 或作为 launchd 用户服务常驻
./service.sh install
```

---

## 🔍 一键故障排查 (`diagnose.py`)

烘焙前如果 Artisan 没有读数或找不到机器，可随时在终端执行：

```bash
python3 diagnose.py
```
或直接在 Mac App 菜单栏面板中点击 **齿轮设置 -> 运行快速测试**。


## 通信与控制保护

- 虚拟串口只接受绝对路径。已有文件、目录不会被覆盖；修改路径即时生效，并保留现有串口会话。创建失败会在界面提示。
- 尚未取得温度、断线或温度超过 3 秒未更新时，不向 Artisan 返回数值，避免把旧温度当成新采样。
- 控制操作先发送指令，再通过匹配请求序号的状态读回确认；3 秒未确认或发送失败会提示错误，不将目标值当成实际状态。状态未知期间暂停相关按钮，收到状态后恢复。
- Python 网页仅监听 `127.0.0.1`。网页控制请求须携带当前页面自动附带的随机令牌，并校验 Host 和 Origin；不再允许局域网其他设备直接访问。
- macOS 快速诊断的 TCP 连接检查最多等待 1 秒。

### 离线回归验证

```bash
python3 -B -m unittest discover -s tests -v
bash tests/run_swift_regressions.sh
```

这些测试使用模拟连接、临时文件和本机假设备，不会连接或控制真实烘焙机；不替代完整应用构建和实机验收。


### 后续可靠性修复

- 网卡遍历跳过空地址；已知固定长度消息校验长度，校验失败逐字节重新定位帧。变长数据保留协议完整长度范围，损坏的未知变长消息仍由连接看门狗超时恢复。
- 正常退出时取消重连与发现任务，清理虚拟串口；桥接读写、活跃状态和启停操作同步，并隔离旧读取线程。
- 保留具体连接失败原因；重连计时器使用 Common RunLoop。在线快速诊断复用现有连接与最近温度时间，不另建 TCP 连接。
- 保留已验证的 `3400`、`1200`、`1300`、`2400`、`1234` 输出，其他四位通道按请求映射；无效通道不改变当前选择。
- 优先通过本机 Artisan 的 Bundle ID `org.artisan-scope.artisan` 查找软件，保留默认路径作为兜底。

本次不改变温度解码含义，不改动探针故障值/负温度解释。
