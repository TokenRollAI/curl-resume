# curl-resume

一个炫酷的终端简历项目，通过 `curl` 命令即可在终端中展示带动画效果的个人简历。

```bash
curl -N me.pdjjq.org
```

## Demo

![Demo](assets/demo.mp4)

> 如果视频无法播放，请查看 [assets/demo.mp4](assets/demo.mp4)

## 特性

- 流式动画效果（打字机、光波、故障切换等）
- 多页面系统（启动页、Logo页、内容页）
- 高度可配置（只需修改 `config.js`）
- 支持自定义 ASCII Logo
- 基于 Cloudflare Workers，全球边缘部署

## 快速开始

### 1. 安装依赖

```bash
npm install
```

### 2. 本地开发

```bash
npm run dev
```

然后在另一个终端测试：

```bash
curl -N http://localhost:8787
```

> `-N` 参数禁用缓冲，确保流式动画效果正常显示

### 3. 部署到 Cloudflare

```bash
npm run deploy
```

## 配置说明

所有配置都在 `src/config.js` 文件中，按需修改即可。

### 基本信息

```javascript
export const config = {
  name: "DJJ",                           // 你的名字/代号
  title: "Department of Joke Justice",   // 标题
  subtitle: "说烂笑话必遭审判",            // 副标题
  // ...
};
```

### 页面配置

项目使用页面系统，支持三种页面类型：

#### Boot 页面（启动动画）

```javascript
{
  type: "boot",
  content: {
    messages: ["> SYSTEM BOOT", ".", ".", ".", " OK"],
    progressBar: true,
  },
  transition: "glitch",  // 切换效果
}
```

#### Logo 页面

```javascript
{
  type: "logo",
  content: {
    customAscii: null,      // 设为 null 使用默认 Logo
    showLightWave: true,    // 是否显示光波效果
  },
  transition: "none",
}
```

#### Content 页面（正文）

```javascript
{
  type: "content",
  content: {
    about: "你的自我介绍...",
    contacts: [
      { label: "GitHub", value: "https://github.com/yourname" },
      { label: "Email ", value: "your@email.com" },
    ],
    slogan: "你的座右铭",
  },
  transition: "none",
}
```

### 转场效果

支持以下转场效果：

- `glitch` - 故障/乱码效果
- `scanline` - 扫描线效果
- `fade` - 渐隐效果
- `none` - 无效果

### 动画速度

```javascript
speed: {
  typing: 12,        // 打字速度 (ms)
  typingPause: 60,   // 标点停顿 (ms)
  transition: 40,    // 切换动画速度 (ms)
  logoDisplay: 800,  // Logo 显示时长 (ms)
}
```

### 自定义 ASCII Logo

在 `config.js` 底部修改 `customLogo`：

```javascript
export const customLogo = `
██████╗      ██╗     ██╗
██╔══██╗     ██║     ██║
██║  ██║     ██║     ██║
██║  ██║██   ██║██   ██║
██████╔╝╚█████╔╝╚█████╔╝
╚═════╝  ╚════╝  ╚════╝
`;
```

你可以使用在线工具生成 ASCII 艺术字：
- [patorjk.com/software/taag](https://patorjk.com/software/taag/)
- [ascii-art-generator.org](https://ascii-art-generator.org/)

## 项目结构

```
src/
├── index.js           # Worker 入口
├── config.js          # 用户配置文件 (修改这里!)
├── streamHandler.js   # 流式输出主入口
├── pageRenderer.js    # 页面渲染器
├── effects.js         # 动画效果库
├── asciiGenerator.js  # ASCII 艺术字生成器
└── utils.js           # 工具函数和 ANSI 码
```

## 自定义域名

### 1. 修改 wrangler.toml

```toml
routes = [{ pattern = "your.domain.com/*", zone_name = "domain.com" }]
```

### 2. 在 Cloudflare Dashboard 配置 DNS

添加 A 记录或 CNAME 记录，并开启 Proxy（小黄云）。

### 3. 部署

```bash
npm run deploy
```

## 动画效果 API

`effects.js` 提供了多种可复用的动画效果：

```javascript
import * as effects from './effects.js';

// 打字机效果
await effects.typewriter(push, "Hello World", { speed: 20 });

// 进度条
await effects.progressBar(push, { width: 40 });

// 光波扫过
await effects.lightWaveEffect(push, logoLines);

// 闪烁光标
await effects.blinkingCursor(push, 3);

// 故障转场
await effects.glitchTransition(push);
```

## 许可证

MIT
