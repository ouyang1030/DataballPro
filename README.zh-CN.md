# DataballPro

**面向足球运动科学的视频分析桌面应用 —— 事件标注、转播级战术绘图与追踪数据，集于一体。**

[![最新版本](https://img.shields.io/github/v/release/ouyang1030/DataballPro)](https://github.com/ouyang1030/DataballPro/releases/latest)
[![下载量](https://img.shields.io/github/downloads/ouyang1030/DataballPro/total)](https://github.com/ouyang1030/DataballPro/releases)
[![使用文档](https://img.shields.io/badge/docs-Help%20Center-blue)](https://ouyang1030.github.io/DataballPro/)

[English](README.md) · **简体中文** · [Deutsch](README.de.md)

> 本仓库用于分发已发布的安装包，应用源码在私有仓库中开发。

![DataballPro 主界面](pics/main_dashboard.png)

---

## 下载

请从 [**Releases**](https://github.com/ouyang1030/DataballPro/releases/latest) 页面获取最新版本。

| 平台                    | 文件                                                                   | 系统要求                                                     |
| ----------------------- | ---------------------------------------------------------------------- | ------------------------------------------------------------ |
| **macOS**（Apple 芯片） | `DataballPro_<版本号>_aarch64.dmg`                                     | macOS 13 Ventura 及以上，仅支持 M 系列芯片，不支持 Intel Mac |
| **Windows**（x64）      | `DataballPro_<版本号>_x64-setup.exe`（安装程序）或 `..._x64_en-US.msi` | Windows 10 或 11                                             |
| **Linux**（x64）        | `DataballPro_<版本号>_amd64.AppImage`                                  | glibc 2.38 及以上（Ubuntu 24.04+、Debian 13+、Fedora 39+）   |

### 必须安装 FFmpeg

DataballPro 依赖 FFmpeg 完成视频解码、代理文件生成、录制与导出。首次使用前请先安装：

```bash
# macOS
brew install ffmpeg

# Windows（PowerShell）
winget install Gyan.FFmpeg

# Debian / Ubuntu
sudo apt install ffmpeg
```

应用会自动在常见安装位置和 `PATH` 中查找。如果你的 FFmpeg 装在特殊位置，可用环境变量 `DATABALLPRO_FFMPEG` 指定可执行文件的完整路径。

如果视频加载失败并提示无法解析视频尺寸，通常是 FFmpeg 安装本身出了问题 —— 重新安装（macOS 上执行 `brew reinstall ffmpeg`）后重启应用即可。

---

## 首次启动

**macOS** —— 安装包为 ad-hoc 签名且未做公证，因此首次打开会被 Gatekeeper 拦下。可以右键点击应用选择**打开**，或清除隔离标记：

```bash
xattr -dr com.apple.quarantine /Applications/DataballPro.app
```

**Windows** —— SmartScreen 可能提示"Windows 已保护你的电脑"，点击**更多信息 → 仍要运行**。

**Linux** —— 先给 AppImage 加上可执行权限；如果你的发行版只自带 FUSE 3，还需要安装 FUSE 2：

```bash
chmod +x DataballPro_*_amd64.AppImage
sudo apt install libfuse2t64      # Ubuntu 24.04+；更早的版本用 libfuse2
./DataballPro_*_amd64.AppImage
```

---

## 功能概览

### 视频标注

逐帧精确的定位与步进、0.25×–2.0× 变速播放，以及由 JSON 编码方案（阶段 → 子阶段 → 事件/阵型）驱动的 Code Window。比赛播放过程中用单键快捷键打点，之后在支持拖放编辑的时间轴上精修。

### 战术绘图（Telestration）

直接在画面上绘制转播级图形：箭头、光环、连线圆环、区域、阵型形状、盯人连线、视野锥、测量、文字与计时器。特效可以跟随被追踪的球员，定格保持会在时间轴中插入一段静止片段，全部内容都能烧录进导出的视频。

### 实时比赛采集

接入采集卡、摄像头或网络流（RTSP/HTTP）。原生 FFmpeg 后端一边录制为 `.mp4`，一边提供低延迟预览，因此无需等待完整视频文件即可实时打点。

### 追踪数据

支持导入处理后的 CSV（Metrica、databallpy）、Opta F24/F7 + 25 Hz TRACAB，以及 Opta Match XML + 10/25 Hz TGV 数据包，并提供导入前检查与质量校验。借助开球等视觉线索将追踪时间与视频时间对齐后，即可查看同步的 2D 战术视图：速度轨迹、Voronoi 空间控制、凸包、球队重心、随比赛阶段变化的阵型识别，以及盯人关系网络。速度、加速度与跑动距离均由原生代码计算。

### 分析

球员热区图（2D 核密度估计，带宽可调）、标签分布与事件流的旭日图与桑基图、示意球场上的事件分布，以及评分者信度分析（Cohen's κ、带 bootstrap 置信区间的 Fleiss' κ、混淆矩阵、时间 IoU）。融合后的数据集 —— 视频事件、派生标注与追踪数据 —— 可导出为 CSV/JSON，供 Python 或 R 进一步分析。

### AI 辅助

球员检测与追踪、球场自动标定。全部计算都在本机完成 —— 无需账号、不上传数据、不联网。检测到的球员可直接与战术绘图特效绑定。

### 界面语言

支持 English、简体中文、Deutsch、Español、Français、日本語、한국어 与 العربية（含从右至左布局）。

---

## 使用文档

[**帮助中心**](https://ouyang1030.github.io/DataballPro/)（英文）覆盖完整工作流：

- [快速上手](https://ouyang1030.github.io/DataballPro/fundamentals/getting-started/) —— 从视频文件或实时信号源创建项目
- [工作区介绍](https://ouyang1030.github.io/DataballPro/fundamentals/workspace/)与[视频播放](https://ouyang1030.github.io/DataballPro/fundamentals/video-playback/)
- [Code Window](https://ouyang1030.github.io/DataballPro/annotation/code-window/)、[事件标注](https://ouyang1030.github.io/DataballPro/annotation/annotating/)、[时间轴编辑](https://ouyang1030.github.io/DataballPro/annotation/timeline/)
- [特效库](https://ouyang1030.github.io/DataballPro/telestration/effects/)、[球员追踪](https://ouyang1030.github.io/DataballPro/telestration/player-tracking/)、[定格画面](https://ouyang1030.github.io/DataballPro/telestration/freeze-frame/)
- [导入追踪数据](https://ouyang1030.github.io/DataballPro/tracking/importing/)与 [2D 球场面板](https://ouyang1030.github.io/DataballPro/tracking/pitch-panel/)
- [分析工具](https://ouyang1030.github.io/DataballPro/analysis/tools/)与[数据导出](https://ouyang1030.github.io/DataballPro/sharing/export/)
- 参考：[快捷键](https://ouyang1030.github.io/DataballPro/reference/keyboard-shortcuts/)、[CSV 格式](https://ouyang1030.github.io/DataballPro/reference/csv-format/)、[偏好设置](https://ouyang1030.github.io/DataballPro/reference/preferences/)

---

## 更新

DataballPro 会检查本仓库的 Releases 并可原地安装更新，因此只需手动下载一次。

## 你的数据

项目就是磁盘上的普通文件夹 —— 视频旁边放着一个存储标注的 SQLite 数据库和若干配置文件。没有任何数据被上传：应用只在检查更新，以及读取你自己配置的流地址时联网。

## 反馈问题

欢迎提交 [issue](https://github.com/ouyang1030/DataballPro/issues)，请附上操作系统与应用版本、你的操作步骤以及实际结果。附上日志文件会很有帮助：

- macOS：`~/Library/Logs/com.databallpro.app/`
- Windows：`%APPDATA%\com.databallpro.app\logs\`
- Linux：`~/.local/share/com.databallpro.app/logs/`

## 许可证

DataballPro 采用 [PolyForm Noncommercial License 1.0.0](LICENSE)：可免费用于科研、教学、个人项目等一切非商业用途，高校与公立研究机构的使用同样在许可范围内。商业用途需要单独授权，请通过 issue 与作者联系。
