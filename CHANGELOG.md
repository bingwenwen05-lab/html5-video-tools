# Changelog

本项目的版本变更记录。遵循 [Keep a Changelog](https://keepachangelog.com/zh-CN/) 与 [语义化版本](https://semver.org/lang/zh-CN/) 规范。

## [3.0.0] - 未发布

### 新增

- **编解码增强模块**：HEVC(H.265) / AV1 解码能力探测与增强
  - 精确探测：`canPlayType` + `MediaCapabilities`（区分硬/软解）+ WebCodecs `VideoDecoder.isConfigSupported`
  - 能力上报修正：`canPlayType` / `MediaSource.isTypeSupported`（默认关闭，绝不误报）
  - WASM 软解引擎：按需从 CDN 加载 [libde265](https://github.com/strukturag/libde265.js) 软解 HEVC，失败自动降级
  - 新增菜单：「编解码能力报告」「启用 HEVC/AV1 WASM 软解」「实验性：上报 HEVC/AV1 支持」
- 新增权限声明：`GM_xmlhttpRequest`、`@connect cdn.jsdelivr.net / unpkg.com / cdnjs.cloudflare.com`

### 变更

- `@version` 2.0.2 → 3.0.0
- 更新脚本 `@description`（中英文）说明新增的编解码增强能力

## [2.0.2] - 原版（停更）

- 原作者发布的最终版本，已停更 3 年以上。
- 功能：视频截图、画中画、视频缓存、万能网页全屏、快捷键（快进/快退/暂停/音量/下一集/全屏/逐帧/倍速）、多站点适配。
