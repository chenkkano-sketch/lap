# 暖暖相册

一个面向《无限暖暖》截图的本地相册工具，基于 [Lap](https://github.com/julyx10/lap) 二次开发。

## 作者声明

- 作者：盔盔
- 博客：https://www.kkano.cc/
- 博客留言板：https://www.kkano.cc/#/tavern
- 新的项目地址：https://github.com/chenkkano-sketch/lap
- 旧项目地址：https://github.com/julyx10/lap

## 二开说明

暖暖相册由盔盔基于开源项目 Lap 二次开发，主要面向《无限暖暖》截图整理使用。感谢 Lap 原作者 julyx10 与社区贡献者，本项目继续保留原项目来源说明，并遵循 GPL-3.0-or-later 协议。

## 目标

- 默认管理无限暖暖截图目录：
  `E:\game\nikiki\InfinityNikki Launcher\InfinityNikki\X6Game\ScreenShot`
- 打开就是中文界面和粉色可爱主题。
- 保留原图位置，不搬走、不上传。
- 用简单入口完成浏览、收藏、图册、标签和按日期查看。
- 隐藏偏专业的摄影管理入口，让女生和小朋友也能轻松使用。

## 当前改造

- 应用名改为“暖暖相册”。
- 默认语言改为中文，默认主题改为暖暖粉。
- 首次启动会尝试自动加入“无限暖暖截图”图册。
- 预置标签：搭配、风景、剧情、自拍、合影、活动、拍照点、滤镜、动作、灵感、收藏候选。
- 简化侧栏，仅保留图册、按日期、小标签、找照片和设置。

## 开发

要求：

- Node.js 20+
- pnpm
- Rust stable

```powershell
git clone --recursive https://github.com/chenkkano-sketch/lap.git
cd lap
git submodule update --init --recursive
cd src-vite
pnpm install
cd ..
cargo install tauri-cli --version "^2.0.0" --locked
.\scripts\download_models.ps1
.\scripts\download_ffmpeg_sidecar.ps1
cargo tauri dev
```

只验证前端：

```powershell
cd src-vite
pnpm install
pnpm build
```

## Windows 打包

需要先准备：

- Visual Studio 2022 Build Tools，包含 C++ 生成工具
- CMake
- NASM
- Rust stable
- pnpm

首次打包前下载本地资源：

```powershell
.\scripts\download_models.ps1
.\scripts\download_ffmpeg_sidecar.ps1
```

生成安装包：

```powershell
cargo tauri build
```

输出目录：

- `src-tauri\target\release\bundle\msi`
- `src-tauri\target\release\bundle\nsis`

默认不生成 updater artifacts，因为 Tauri 自动更新包需要 `TAURI_SIGNING_PRIVATE_KEY` 私钥签名。正式发布自动更新时，再开启 `bundle.createUpdaterArtifacts` 并配置签名环境变量。

## 协议

本项目基于 Lap 二次开发，继续使用 GPL-3.0-or-later。公开分发修改版时，需要同时提供对应源码和许可证说明。
