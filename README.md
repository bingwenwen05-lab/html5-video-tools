# 🎬 HTML5 视频播放工具 · HTML5 Video Playing Tools

> 📺 一款面向网页端 HTML5 播放器的**增强**用户脚本（Tampermonkey / 篡改猴）
> 🧩 在站点**原有播放器**之上叠加功能，**不替换、不接管**原生播放器，安全无侵入

[![License](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/Version-3.0.0-orange.svg)](CHANGELOG.md)
[![Platform](https://img.shields.io/badge/Platform-Tampermonkey-green.svg)](https://www.tampermonkey.net/)

---

## 📖 简介

本项目在原作（GreasyFork 用户 7036，已停更 3 年以上）基础上维护更新。除保留原作全部功能外，**v3.0 新增了 HEVC(H.265) / AV1 编解码能力探测与增强**，让网页端视频更好地利用现代浏览器的编解码能力（原生 / WebCodecs 硬解 / WASM 软解）。

脚本采用**增强而非替换**的设计原则：不接管站点播放器，只在原生 `<video>` 元素之上叠加快捷键、截图、画中画、全屏、缓存、编解码探测等能力，兼容性与稳定性更好。

### 🚀 主要特点

- **零侵入** —— 不改动站点播放器，仅在原生元素上叠加功能
- **全局快捷键** —— 一套按键通吃所有网站
- **编解码增强** —— HEVC/AV1 能力探测 + 非侵入式增强
- **多站点适配** —— 覆盖国内外主流视频 / 直播平台
- **多语言** —— 中文 / English / Italiano

---

## ✨ 功能特性

### 🎮 播放控制
- ⏯️ 暂停 / 播放
- ⏩ 快进 / 快退（5 秒 / 20 秒）
- 🔊 音量调节
- ⏭️ 播放下一集
- 🎚️ 倍速播放（0.1 倍步进，支持 0.1× ~ 16×）

### 🖥️ 画面与显示
- 🖥️ 全屏 / 网页全屏（万能网页全屏）
- 🖼️ 画中画（PiP）
- 🔍 逐帧播放（上一帧 / 下一帧）
- 📷 视频截图（一键保存 PNG）

### 💾 网络与缓存
- 💾 视频缓存（M 键，缓冲全部或按默认大小）
- 🛡️ 卡顿修复（部分站点）

### 🎞️ 编解码增强（v3.0 新增）
- 🔬 精确探测 H.264 / HEVC / AV1 / VP9 支持情况
- 🎛️ 能力上报修正（让站点正确下发 HEVC/AV1 流，而非降级 H.264）
- 🧪 WASM 软解引擎（[libde265](https://github.com/strukturag/libde265.js) 软解 HEVC，按需加载）

---

## ⌨️ 快捷键

| 按键 | 功能 |
| --- | --- |
| `←` / `→` | 快退 / 快进 5 秒 |
| `Shift` + `←` / `→` | 快退 / 快进 20 秒 |
| `↑` / `↓` | 音量加 / 减 |
| `空格` | 暂停 / 播放 |
| `回车` | 切换全屏 |
| `Shift` + `回车` | 切换网页全屏 |
| `Esc` | 退出全屏 |
| `P` | 视频截图 |
| `I` | 进入 / 退出画中画 |
| `M` | 开始 / 停止缓存视频 |
| `N` | 播放下一集 |
| `C` / `X` / `Z` | 加速 / 减速 0.1 倍 / 切换加速状态 |
| `D` / `F` | 上一帧 / 下一帧 |
| 双击播放器 | 切换（网页）全屏 |
| 鼠标中键 | 快进 5 秒 |

> 💡 **站点按键差异**：YouTube 用 `V` 加速、`E` 下一帧；抖音用 `S` 减速、`A` 切换加速。详见脚本内帮助菜单。

---

## 🎞️ 编解码增强（v3.0 新增）

### 背景：为什么需要它？

| 编码 | Safari | Chrome / Edge | Firefox |
| --- | --- | --- | --- |
| **AV1** | 17+ 原生 ✅ | 70+ / 79+ 原生 ✅ | 67+ 原生 ✅ |
| **HEVC (H.265)** | 原生 ✅ | 仅 WebCodecs 硬解（107+，取决于硬件）⚠️ | ❌ |

- **AV1** 在现代浏览器已普遍原生支持；
- **HEVC** 是主要痛点：Chrome 的 `<video>` 标签原生不支持，只能借助 WebCodecs 硬解或 WASM 软解。

### 三条互补路径（均为非侵入式，不替换播放器）

1. 🔬 **精确探测** —— 同时使用 `canPlayType`、`MediaCapabilities`（区分硬/软解）、WebCodecs `VideoDecoder.isConfigSupported` 判断真实能力；
2. 🎛️ **能力上报修正** —— 修正 `canPlayType` / `MediaSource.isTypeSupported` 的上报，使站点正确下发 HEVC/AV1 流而非降级 H.264（**默认关闭，绝不误报**）；
3. 🧪 **WASM 软解引擎** —— 缺少原生支持时，通过 Web Worker 动态加载 [libde265](https://github.com/strukturag/libde265.js) 软解 HEVC，失败自动降级回原生路径。

### 新增菜单项

- 📊 **编解码能力报告** —— 弹窗 + 控制台展示 H.264 / HEVC / AV1 / VP9 完整支持情况；
- 🧪 **启用 HEVC/AV1 WASM 软解**；
- 🎛️ **实验性：上报 HEVC/AV1 支持**（默认关闭）。

---

## 🌐 支持的网站

### 🎬 视频平台
- **YouTube**、**TED**
- **优酷**、**腾讯视频 (QQ)**、**B 站 (bilibili)**、**西瓜视频 (ixigua)**、**爱奇艺 (iQiyi)**、**A 站 (AcFun)**、**PPTV**、**芒果 TV (MGTv)**、**咪咕视频**
- **新浪视频**、**微博**、**网易**（娱乐 / 云课堂 / 新闻）、**搜狐**、**风行**、**凤凰视频**、**时光网**
- **百度网盘**、**腾讯微云**、**头条**、**豆瓣电影**

### 📡 直播平台
- **Twitch**、**斗鱼**、**YY**、**虎牙**、**龙珠**、**战旗**、**抖音直播**

> ➕ 支持自定义站点扩展（见下文「自定义与站点适配」）。

---

## 📦 安装

### 方式一：GreasyFork 安装（推荐）

1. 安装浏览器扩展 [Tampermonkey（篡改猴）](https://www.tampermonkey.net/)；
2. 在 GreasyFork 脚本页点击「安装此脚本」。

### 方式二：手动安装

1. 安装 [Tampermonkey](https://www.tampermonkey.net/)；
2. 打开 Tampermonkey 控制台 →「新建脚本」；
3. 将 [`HTML5视频播放工具.user.js`](HTML5视频播放工具.user.js) 内容整体粘贴进去并保存（或直接将该文件拖入浏览器）。

---

## 🛠️ 自定义与站点适配

脚本通过 `router` 对象按主域名适配各站点。要**新增一个站点**，只需在 `router` 中按主域名增加配置项：

```js
router.mySite = function() {
    cfg.fullCSS = '.fullscreen-btn';   // 全屏按钮选择器
    cfg.nextCSS = '.next-btn';         // 下一集按钮选择器
    cfg.playCSS = '.play-btn';         // 播放按钮选择器
    // cfg.shellCSS = '#player';       // 播放器容器（可选）
};
```

主域名取自 `location.hostname`（如 `bilibili.com` → 用 `bilibili` 作为键名）。详细的站点配置可参考脚本内 `router` 对象的现有实现。

---

## 🤝 开源与贡献

本项目以 [MIT](./LICENSE) 协议开源。欢迎提交 Issue / Pull Request 🙌

- 🐛 报告 Bug：请在 [Issues](../../issues) 中描述问题、附上浏览器版本与站点；
- ✨ 新增站点适配：按上文在 `router` 对象中增加配置项；
- 🧩 新增功能：遵循「增强而非替换」原则，尽量在原生播放器之上叠加。

---

## 🙏 贡献者

| 贡献者 | 贡献 |
| --- | --- |
| [bingwenwen05-lab](https://github.com/bingwenwen05-lab) | 项目维护者、开源发布 |
| DeepSeek AI 助手 | v3.0 编解码增强模块开发、README 文档、开源发布协助 |
| 原作者（GreasyFork 用户 7036） | 原版脚本（v2.0.2 及之前） |
| [Dario Costa](https://greasyfork.org/) | 英语 / 意大利语翻译 |

---

## 📄 许可证

本项目采用 [MIT License](./LICENSE) 开源协议。

## 📝 更新日志

详细的版本变更记录见 [CHANGELOG.md](./CHANGELOG.md)。

---

## 🌍 English

A non-invasive **enhancement** userscript for web HTML5 players (Tampermonkey). It layers features on top of the site's existing player without replacing or hijacking it. This fork adds **HEVC(H.265)/AV1 codec detection & enhancement** on top of the original (unmaintained for 3+ years) script.

- ✨ **Features**: global hotkeys, screenshot, picture-in-picture, video caching, universal web fullscreen, and codec detection/reporting.
- 🎞️ **Codec enhancement (v3.0)**: accurate detection via `canPlayType` + `MediaCapabilities` + WebCodecs `VideoDecoder`, safe capability reporting (opt-in), and on-demand WASM software decoding ([libde265](https://github.com/strukturag/libde265.js)) with graceful fallback.
- 📄 **License**: [MIT](./LICENSE)
