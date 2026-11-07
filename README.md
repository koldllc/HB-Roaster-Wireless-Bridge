# HB Roaster Wireless Bridge ☕️

<p align="center">
  <b>Wireless bridge connecting HB Coffee Roasters to Artisan Roaster Scope via Wi-Fi</b><br>
  <b>让 Artisan 烘焙软件通过局域网 Wi-Fi 无线连接 HB 咖啡烘焙机</b>
</p>

<p align="center">
  <a href="#english">English</a> • <a href="#chinese">中文说明</a> • <a href="https://github.com/koldllc/HB-Roaster-Wireless-Bridge/releases">Releases</a>
</p>

---

<a name="english"></a>
## English

> **Wirelessly connect HB Coffee Roasters to Artisan Roasting Scope over Wi-Fi (macOS Menu Bar App)**  
> Comprehensive support for HB Model S series (M2S / M6S / L2S / L6S) and HB Standard series.

Eliminate physical USB serial cables! 100% compatible with your existing Artisan curves, colors, 4-channel configuration, events, and alarms for an effortless, instant wireless roasting experience.

### 🌟 Key Features

- ☕️ **Minimalist Menu Bar App**: Sits discreetly in your macOS top menu bar with an optional real-time `BT / ET` temperature readout (e.g. `162° / 183°`).
- 🖥️ **Modern Control Popover**: Click the menu bar flame icon to view all 4 temperature channels (BT, ET, Inlet, Exhaust), Artisan polling state, and network status at a glance.
- 🌐 **Bilingual & Localization Support**: Native English and Simplified Chinese support with automatic macOS system language detection, plus manual language switching in Settings.
- 🔀 **Built-in TC4 Protocol Emulation**: Automatically creates an in-memory virtual serial port at `/tmp/hb_roaster` adhering to standard Arduino TC4 protocol (`126 ARC_BTET` & `127 ARC_METIT`).
- 🛡️ **Machine Control & Safety Confirmations**: Control roaster power, heat, fan, and agitation directly from the popover with confirmation guards to prevent bean scorching.
- 🔄 **Auto-Discovery & Reconnect**: Built-in UDP 17888 network broadcast discovery with direct physical network interface binding (VPN/proxy-immune) and candidate IP memory.
- 🚀 **Launch at Login**: Enable `SMAppService` background launch at login via Settings.
- 🪶 **Native & Lightweight**: Built with 100% native Swift & SwiftUI, requiring no Python or external dependencies, with < 25 MB RAM usage and ~1 MB bundle size.

---

### 📥 Download & Installation

#### Requirements
- **OS**: macOS 13.0 (Ventura) or newer
- **Architecture**: Apple Silicon (M1/M2/M3/M4) & Intel Mac

#### Installation Steps
1. Download the latest `HB-Roaster-Wireless-Bridge-macos.zip` from [**Releases**](https://github.com/koldllc/HB-Roaster-Wireless-Bridge/releases).
2. Unzip and drag **`HB Roaster Wireless Bridge.app`** into your **`/Applications`** folder.
3. Double-click to launch the app.

> **💡 macOS Security Notice (First Launch)**:  
> If macOS displays *"cannot be opened because the developer cannot be verified"*:  
> 1. Go to **System Settings** -> **Privacy & Security**.  
> 2. Scroll down to the security section and click **"Open Anyway"**.  
> *(Alternatively, right-click `HB Roaster Wireless Bridge.app` in `/Applications` and select **"Open"**)*.

---

### 📐 Architecture

```
+-------------------+                 +--------------------------------+                 +-------------------+
|   HB Coffee       |   Wi-Fi (TCP)   | HB Roaster Wireless Bridge.app |   Virtual PTY   |   Artisan Roaster |
|   Roaster (ESP)   | <=============> |  (macOS Native Menu Bar App)   | <=============> |   Scope Software  |
|  Port: 16888/TCP  |   (1Hz 4-Ch)    |   Auto-discovery & Reconnect   | /tmp/hb_roaster |   Arduino TC4     |
+-------------------+                 +--------------------------------+                 +-------------------+
```

---

### ⚡ 3-Minute Quickstart

#### Step 1: Power on Roaster
1. Switch on mechanical power on the HB coffee roaster.
2. Confirm the Wi-Fi icon on the roaster screen changes from blinking to **solid** (indicating Wi-Fi connection).
3. Ensure your Mac and roaster are connected to the same local Wi-Fi router.

#### Step 2: Launch App
1. Open **HB Roaster Wireless Bridge.app**.
2. A flame icon ☕️ appears in your macOS menu bar. Click to expand the popover:
   - Status will show `Online (192.168.x.x)`.
   - All 4 temperature channels begin updating in real-time.
   - *(Optional: Click the ⚙️ gear icon, enable "Launch bridge automatically at login" and choose your preferred Language)*.

#### Step 3: Configure Artisan (One-time setup)
1. Open **Artisan**.
2. Go to **Config** -> **Device**: Select **HB Model S** (or **HB Standard**).
3. Go to **Config** -> **Ports**:
   - **Comm Port**: Enter `/tmp/hb_roaster`
   - **Baud Rate**: `115200`
   - **Parameters**: `8` data bits, `None (N)` parity, `1` stop bit
   - Click **OK**.
4. Click Artisan's top-left **ON** button:
   - All 4 curves will immediately start plotting in real-time!
   - The Artisan status indicator in the app popover turns green: **"Artisan Sampling (Active)"**.
5. Click **START** to record your roast profile.

---

### ⚙️ Channel Mapping

| Channel | Probe Location | Artisan Driver | Artisan Curve Name | Suggested Color | Description |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **Ch 3** | **Bean Temp (BT)** | `ARC_BTET` | **BT** | Green (`#00D084`) | Temperature inside drum contacting beans; used for RoR calculation |
| **Ch 4** | **Env / Drum Temp (ET)** | `ARC_BTET` | **ET** | Red (`#FF4757`) | Environmental air temperature inside roasting chamber |
| **Ch 1** | **Exhaust Temp (MET)** | `+ARC MET/IT` | **Exhaust** | Sky Blue (`#38BDF8`) | Exhaust flue and chaff collector air temperature |
| **Ch 2** | **Inlet Temp (IT)** | `+ARC MET/IT` | **Inlet** | Warm Ochre (`#FBBF24`) | Hot air temperature entering roasting drum |

---

<a name="chinese"></a>
## 中文说明

> **让 Artisan 烘焙软件通过局域网 Wi-Fi 无线连接 HB 咖啡烘焙机**  
> 全面支持 HB Model S 系列（M2S / M6S / L2S / L6S）及 HB Standard 系列。

无需物理 USB 串口线，告别线缆束缚！100% 沿用原先在 Artisan 中调校好的曲线颜色、四通道配置、事件与报警规则，实现开机即连、平滑无缝的无线烘焙体验。

### 🌟 核心特性

- ☕️ **极简菜单栏常驻**：顶部菜单栏轻量图标指示连接状态，并支持直接在菜单栏实时显示 `BT / ET` 温度（如 `162° / 183°`）。
- 🖥️ **现代精美控制面板**：点击菜单栏图标即可弹出高可读性仪表盘，四路温度一览无余，同时显示 Artisan 采集状态与连接时延。
- 🌐 **多语言界面支持**：原生支持简体中文与英文，默认跟随 macOS 系统语言，亦可在设置面板中自由切换。
- 🔀 **内置虚拟串口仿真**：自动在 `/tmp/hb_roaster` 建立符合标准 Arduino TC4 协议的内存管道，与 Artisan 原厂驱动 100% 兼容。
- 🛡️ **安全双重确认控制**：支持从面板直接控制烘焙机电源与电加热，关键操作提供二次确认防护，杜绝误触。
- 🔄 **局域网自动重连与发现**：内置 17888 UDP 自动扫描，并针对常见网络代理环境进行了底层物理网卡绑定，具备记忆 IP 闪连机制。
- 🚀 **开机静默自启**：面板设置中支持一键启用开机自启动（`SMAppService`），开机即连，无需每次手动打开。
- 🪶 **超轻量原生体验**：纯 Swift 原生打造，无任何 Python 或第三方运行库依赖，体积仅 ~1MB，内存占用极低。

---

### 📥 下载与安装

#### 系统要求
- **操作系统**：macOS 13.0 (Ventura) 及以上版本
- **架构支持**：Apple Silicon (M1/M2/M3/M4) 及 Intel Mac

#### 安装步骤
1. 前往 [**Releases 页面**](https://github.com/koldllc/HB-Roaster-Wireless-Bridge/releases) 下载最新版本的 `HB-Roaster-Wireless-Bridge-macos.zip`。
2. 双击解压后，将 **`HB Roaster Wireless Bridge.app`** 拖移到系统的 **`应用程序` (/Applications)** 文件夹中。
3. 双击打开应用即可。

> **💡 macOS 首次启动安全提示**：  
> 若系统提示「无法打开，因为来自未知的开发者」或「无法验证开发者」：  
> 1. 打开 **系统设置 (System Settings)** -> **隐私与安全性 (Privacy & Security)**。  
> 2. 向下滚动找到安全性提示，点击 **「仍要打开 (Open Anyway)」**。  
> 3. 或者在 `应用程序` 文件夹中按住 `Control` 键并右键点击该 App，选择 **「打开」** 即可。

---

### ⚡ 烘焙前 3 分钟极速上手

#### 步骤 1：开启烘焙机电源
1. 打开 HB 烘焙机机身机械电源总开关。
2. 观察机身触摸屏或数码管：数秒后 Wi-Fi 图标由闪烁变为**常亮**（表示已成功连上路由网络）。
3. 确保你的 Mac 电脑与烘焙机处于同一个 Wi-Fi 局域网下。

#### 步骤 2：启动应用
1. 打开 **HB Roaster Wireless Bridge.app**。
2. 菜单栏出现咖啡图标 ☕️，轻触展开面板：
   - 状态显示为 `已连接 (192.168.x.x)`。
   - 仪表盘四路温度（BT / ET / 出风 / 进风）开始实时更新。
   - *(可点击齿轮设置，切换语言或勾选「开机自动启动」)*。

#### 步骤 3：在 Artisan 中切换端口（仅需配置一次）
1. 打开 **Artisan** 烘焙软件。
2. 点击菜单 **配置 (Config)** -> **设备 (Device)**：选择 **HB Model S**（或 HB Standard）。
3. 点击菜单 **配置 (Config)** -> **端口 (Ports)**：
   - **Comm Port (通信端口)**：手动输入 `/tmp/hb_roaster`
   - **波特率 (Baud Rate)**：`115200`
   - **参数**：保持默认的 `8` 位、`None` 校验、`1` 停止位
   - 点击 **OK** 保存。
4. 点击 Artisan 左上角 **ON** 按钮：
   - 四路曲线立即开始实时绘制！
   - 应用面板中的 Artisan 状态同步转为绿色的 **「Artisan 采集中」**。
5. 点击 **START** 即可开始正式烘焙记录。

---

### ⚙️ Artisan 详细配置与四通道映射

| 硬件物理通道 | 探针测量位置 | Artisan 设备驱动 | Artisan 曲线名称 | 建议颜色 | 说明 |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **通道 3** | **豆温 (BT)** | `ARC_BTET` | **BT** | 亮绿色 (`#00D084`) | 锅炉内部接触咖啡豆的温度，计算 RoR 升温率的核心 |
| **通道 4** | **环境/炉温 (ET)** | `ARC_BTET` | **ET** | 鲜红色 (`#FF4757`) | 锅炉内部环境空气温度 |
| **通道 1** | **出风温 (Exhaust)** | `+ARC MET/IT` | **Exhaust** | 天蓝色 (`#38BDF8`) | 银皮收集桶 / 排气烟道废气温度 |
| **通道 2** | **进风温 (Inlet)** | `+ARC MET/IT` | **Inlet** | 明黄色 (`#FBBF24`) | 热源加热管送入滚筒前的热风温度 |

---

## 📚 详细使用文档 / Documentation

- 🚀 [**烘焙实战操作指南 (Pre-Roast 3 分钟速查)**](docs/QUICKSTART_ROAST_GUIDE.md)：下次烘焙时的标准操作流程（通电、连机、预热、下豆与归档）。
- ⚙️ [**Artisan 配置与四路曲线详解**](docs/ARTISAN_SETUP.md)：端口参数、Extra Devices、曲线色彩及高级控制选项。
- 🔍 [**常见问题排查与技术指南**](docs/TROUBLESHOOTING.md)：扫不到设备、VPN 代理冲突、热点直连模式与故障排查。

---

## ❓ 常见问题速查 / FAQ

<details>
<summary><b>1. 启动应用后提示「未连接」或未扫到烘焙机？ / "Disconnected" or roaster not found?</b></summary>

- **检查电源 (Power)**：确认机身机械开关已通电，屏幕 Wi-Fi 图标常亮。
- **确认同一 Wi-Fi (Same Network)**：Mac 与烘焙机必须连接在同一个无线路由器下。
- **代理软件干扰 (Proxy/VPN Conflicts)**：若电脑开启了 Clash / Surge / Loon 等代理，建议将代理切换为直连模式，或在应用设置中直接填入烘焙机局域网 IP（如 `192.168.88.88`）手动连接。
</details>

<details>
<summary><b>2. Artisan 点击 ON 报错「Serial Exception: Unable to open serial port」？</b></summary>

- 确认 **HB Roaster Wireless Bridge.app** 已经启动且处于运行状态（虚拟串口仅在应用运行时存在）。
- 确认 Artisan 端口路径输入为 `/tmp/hb_roaster`，前后无多余空格。
</details>

<details>
<summary><b>3. 如何切换界面语言或开机自启动？ / How to switch language or launch at login?</b></summary>

轻触顶部菜单栏咖啡图标展开面板 -> 点击右上角「⚙️ 齿轮」进入设置：
- **Language (界面语言)**：选择跟随系统 (System Default)、English 或 简体中文。
- **Launch at Login (开机启动)**：勾选「开机/登录时自动启动」即可。
</details>

---

## 📄 License

面向 HB Roaster 咖啡烘焙机用户免费发布与使用。
Free to use for all HB Roaster coffee roaster users.
