# HTML5 视频播放工具 · HTML5 Video Playing Tools

> 网页端 HTML5 播放器**增强**脚本（Tampermonkey / 篡改猴 用户脚本）。
> 在站点原有播放器之上叠加功能，**不替换、不接管**原生播放器。

本项目在原作（已停更 3 年以上）基础上维护更新，新增 **HEVC(H.265) / AV1 编解码能力探测与增强**，让网页端视频更好地利用现代浏览器（WebCodecs 硬解等）的能力。

[English](#english) · [特性](#特性) · [快捷键](#快捷键) · [编解码增强](#编解码增强-v30) · [安装](#安装) · [支持的网站](#支持的网站) · [开源与贡献](#开源与贡献)

---

## 特性

- 🎹 **全局快捷键**：快进/快退、暂停/播放、音量、下一集、全屏、逐帧、倍速
- 📷 **视频截图**（P 键）
- 🖼️ **画中画**（I 键）
- 💾 **视频缓存**（M 键）
- 🖥️ **万能网页全屏**
- 🎞️ **HEVC / AV1 编解码能力探测与增强**（v3.0 新增）
- 🌐 支持国内外主流视频站点与直播平台（详见 [支持的网站](#支持的网站)）
- 📝 三语言界面：中文 / English / Italiano

---

## 快捷键

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

> 抖音/YouTube 等站点的按键有单独映射，详见脚本内帮助菜单。

---

## 编解码增强（v3.0）

### 背景

| 编码 | Safari | Chrome / Edge | Firefox |
| --- | --- | --- | --- |
| **AV1** | 17+ 原生 | 70+ / 79+ 原生 | 67+ 原生 |
| **HEVC (H.265)** | 原生 | 仅 WebCodecs 硬解（107+，取决于硬件/系统） | ❌ |

- **AV1** 在现代浏览器已普遍原生支持；
- **HEVC** 是主要痛点：Chrome 的 `<video>` 标签原生不支持，只能借助 WebCodecs 硬解或 WASM 软解。

### 三条互补路径（均为非侵入式，不替换播放器）

1. **精确探测** —— 同时使用 `canPlayType`、`MediaCapabilities`（区分硬/软解）、WebCodecs `VideoDecoder.isConfigSupported` 判断真实能力；
2. **能力上报修正** —— 修正 `canPlayType` / `MediaSource.isTypeSupported` 的上报，使站点正确下发 HEVC/AV1 流而非降级 H.264（**默认关闭，绝不误报**）；
3. **WASM 软解引擎** —— 缺少原生支持时，通过 Web Worker 动态加载 [libde265](https://github.com/strukturag/libde265.js) 软解 HEVC，失败自动降级回原生路径。

### 新增菜单项

- **编解码能力报告** —— 弹窗 + 控制台展示 H.264 / HEVC / AV1 / VP9 完整支持情况；
- **启用 HEVC/AV1 WASM 软解**；
- **实验性：上报 HEVC/AV1 支持**（默认关闭）。

---

## 安装

1. 安装浏览器扩展 [Tampermonkey（篡改猴）](https://www.tampermonkey.net/)；
2. 安装本脚本：
   - **GreasyFork**：在脚本页点击「安装此脚本」；
   - **手动**：Tampermonkey 控制台 →「新建脚本」→ 粘贴 `HTML5视频播放工具.user.js` 内容并保存。

---

## 支持的网站

- **视频**：YouTube、TED、优酷、腾讯视频(QQ)、B站、西瓜视频、爱奇艺、A站、PPTV、芒果TV、咪咕视频、新浪、微博、网易(娱乐/云课堂/新闻)、搜狐、风行、百度网盘、腾讯微云、头条、豆瓣电影等
- **直播**：Twitch、斗鱼、YY、虎牙、龙珠、战旗、抖音等
- 支持自定义站点扩展

---

## 开源与贡献

本项目以 [MIT](./LICENSE) 协议开源。欢迎提交 Issue / Pull Request。

- 新增站点适配：在 `router` 对象中按主域名增加配置项；
- 新增功能：遵循「增强而非替换」原则，尽量在原生播放器之上叠加。

### 致谢

- 原作者（GreasyFork 用户 7036）及 [原发布帖](https://bbs.kafan.cn/thread-2093014-1-1.html)
- 英语/意大利语翻译：[Dario Costa](https://greasyfork.org/)
- 万能网页全屏思路参考：[gooyie/ykh5p](https://github.com/gooyie/ykh5p)
- HEVC 软解：[strukturag/libde265.js](https://github.com/strukturag/libde265.js)

---

<a name="english"></a>
## English

A non-invasive **enhancement** userscript for web HTML5 players (Tampermonkey). It layers features on top of the site's existing player without replacing or hijacking it. This fork adds **HEVC(H.265)/AV1 codec detection & enhancement** on top of the original (unmaintained for 3+ years) script.

- **Features**: global hotkeys, screenshot, picture-in-picture, video caching, universal web fullscreen, and codec detection/reporting.
- **Codec enhancement (v3.0)**: accurate detection via `canPlayType` + `MediaCapabilities` + WebCodecs `VideoDecoder`, safe capability reporting (opt-in), and on-demand WASM software decoding ([libde265](https://github.com/strukturag/libde265.js)) with graceful fallback.
- **License**: [MIT](./LICENSE)

## License

MIT © contributors
