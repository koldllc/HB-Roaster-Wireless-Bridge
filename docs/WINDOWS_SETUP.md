# HB Roaster Wireless Bridge · Windows 版配置与使用指南 ☕️

本文档指导在 **Windows 10 / 11** 系统下使用 **HB Roaster Wireless Bridge** 并将数据接入 **Artisan 烘焙软件**。

---

## 🌟 核心特性 (Windows 原生体验)

- 🖥️ **Windows 原生图形界面**：深色现代仪表盘，实时呈现豆温 (BT)、炉温 (ET)、出风温 (MET) 与进风温 (IT) 四路数据。
- 🛡️ **机身状态安全控制**：支持机械电源、搅拌臂、排风扇与加热管开关控制，加热操作具备防误触二次确认弹窗。
- 🔔 **系统托盘常驻 (System Tray)**：关闭窗口时自动最小化至任务栏右下角托盘，鼠标悬浮即可预览即时豆温与炉温。
- 🚀 **开机自启**：支持写入 Windows 当前用户启动项，电脑开机后后台静默就绪。
- 🌐 **双语界面**：支持一键切换简体中文与英文。
- ⚡ **双模式 Artisan 通信支持**：
  - **模式 1 (强烈推荐 · 免驱动直连)**：Artisan 原生 WebSocket 模式，无需安装任何虚拟串口驱动！
  - **模式 2 (传统模式 · 串口兼容)**：通过 com0com 虚拟串口对，100% 沿用 Artisan "HB Model S" (TC4) 配置。

---

## 🚀 极速起步 (只需两步)

### 第一步：启动 Windows 桥接程序

1. 从 [Releases 页面](https://github.com/koldllc/HB-Roaster-Wireless-Bridge/releases) 下载最新版 `HB-Roaster-Wireless-Bridge-windows.zip` 并解压。
2. 双击运行 `HB-Roaster-Wireless-Bridge.exe`（无需安装 Python 或任何依赖）。
3. 首次启动时若弹出 Windows 防火墙提示，请勾选 **「专用网络」** 并点击 **「允许访问」**。
4. 程序将自动进行局域网 UDP 广播并连接烘焙机：
   - 顶部状态变为绿色的 **「已连接 (192.168.xx.xx)」**。
   - 四张大字温度卡片开始实时滚动更新。

---

### 第二步：在 Artisan 中配置数据源 (推荐方案 1)

#### 方案 1：WebSocket 极速直连 (零驱动 · 2 分钟搞定 · 强烈推荐)

Artisan 软件本身内置了 WebSocket 客户端，无需安装任何第三方虚拟串口工具！

1. 打开 **Artisan** 软件。
2. 菜单栏选择 **配置 (Config)** -> **设备 (Device)**：
   - 在主设备下拉列表中，选择 **WebSocket**。
3. 菜单栏选择 **配置 (Config)** -> **端口 (Ports)**：
   - 点击最右侧的 **Web Sockets** 标签页。
   - 在端点配置处：
     - **Host (主机)**：`127.0.0.1`
     - **Port (端口)**：`8088`
     - **Path (路径)**：`/ws`
     - （完整地址对应 `ws://127.0.0.1:8088/ws`）
   - 点击右下角 **OK** 保存。
4. 点击 Artisan 左上角 **ON** 按钮开始采集：
   - 曲线立即开始绘制，桥接软件界面的 Artisan 状态会同步变为绿色的 **「Artisan 采集中 (活跃)」**！

---

#### 方案 2：传统虚拟串口模式 (Arduino TC4 协议)

如果你希望继续沿用原有的 `artisan设置.aset` 或原厂 "HB Model S" 驱动配置：

1. 下载并安装开源的 Windows 虚拟串口驱动对工具 [com0com](https://sourceforge.net/projects/com0com/)。
2. 在 com0com 中创建一对互联的虚拟串口（例如：`COM10` 与 `COM11`）。
3. 在本桥接软件的「Artisan 连接设置」->「串口模式」中填入：
   ```text
   COM11
   ```
4. 打开 Artisan：
   - 菜单选择 **配置 (Config)** -> **设备 (Device)**：保持选择 **HB Model S**。
   - 菜单选择 **配置 (Config)** -> **端口 (Ports)**：
     - **Comm Port**：选择或输入配对的另一端：
       ```text
       COM10
       ```
     - **波特率 (Baud Rate)**：`115200`
     - **数据位 / 校验 / 停止位**：`8` / `None` / `1`
5. 点击 Artisan 左上角 **ON** 开启监控。

---

## 🔍 Windows 常见问题排查 (Troubleshooting)

### 1. 扫描不到烘焙机？
- 确认电脑与烘焙机处于同一局域网 Wi-Fi 下（例如都连接了工作间的路由器）。
- 若烘焙机为出厂 AP 热点模式，请在电脑 Wi-Fi 列表中直接连接烘焙机发出的热点，桥接程序会自动连接 `192.168.4.1`。
- 如果电脑开启了 Clash / v2rayN 等代理软件的 **TUN 虚拟网卡模式**，可能会拦截局域网广播。可在代理软件中将局域网段（`192.168.0.0/16`）加入直连旁路名单，或在桥接软件的「烘焙机网络」标签页中直接手动输入烘焙机 IP。

### 2. Artisan 显示通信超时或 Unable to open serial port？
- **WebSocket 模式下**：请确认 Artisan 端口中的 Host 为 `127.0.0.1`，Port 为 `8088`，Path 为 `/ws`。
- **串口模式下**：同一串口不能被多个程序同时占用。确认没有其他串口调试助手正在占用 `COM10` 或 `COM11`。

### 3. 如何在手机或平板上同步查看温度？
桥接程序已内置跨平台网页控制台：
- 在 Windows 电脑或局域网内任意手机/平板浏览器中打开：
  ```text
  http://<你的电脑IP>:8080
  ```
- 即可在浏览器中实时查看四路温度大表盘及机身控制开关。

---

## 🛠️ 从源码运行或自主打包 (针对开发者)

如果你在 Windows 上安装了 Python 3.9+：

```cmd
# 克隆仓库
git clone https://github.com/koldllc/HB-Roaster-Wireless-Bridge.git
cd HB-Roaster-Wireless-Bridge

# 安装依赖
pip install -r requirements_windows.txt

# 运行图形界面
python hb_windows_app.py

# 或一键打包生成单个独立 .exe
build_windows.bat
```
编译产物将保存在 `dist/HB-Roaster-Wireless-Bridge.exe`。
