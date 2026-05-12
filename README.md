# 🎈 Gesture Balloon Game | 手势抓气球游戏

基于摄像头手势识别的浏览器小游戏。张开手掌移动，握拳抓取飘动的气球！

使用 [MediaPipe Hands](https://developers.google.com/mediapipe/solutions/vision/hand_landmarker) 进行实时手势识别，纯前端实现，无需安装任何依赖。

## ✨ 游戏特性

### 核心玩法
- ✋ **张开手掌** 移动到气球位置
- ✊ **握拳** 抓取气球（必须完成完整动作才触发）
- 60 秒时间限制，分数越高越好

### 气球类型

| 气球 | 效果 | 出现率 |
|------|------|--------|
| 🔴 红色 | +10 分 | 38% |
| 🟢 青色 | +15 分 | 25% |
| 🟡 黄色 | +20 分 | 18% |
| ⭐ 星星 | +50 分 + 金色特效 | 6% |
| ❄ 冰冻 | 所有气球减速 5 秒 | 5% |
| ×2 双倍 | 8 秒内分数翻倍 | 4% |
| ⏰ 时间 | +5 秒游戏时间 | 2% |
| 💣 炸弹 | -30 分 + 屏幕震动 + 连击清零 | 2% |

### 趣味机制
- 🔥 **连击系统** - 2 秒内连续抓取累积 Combo，3 连击起每次额外加分
- 📈 **难度递增** - 每 15 秒提升等级，气球出现更频繁、上升更快
- 💫 **视觉特效** - 屏幕震动、闪光、粒子、冰冻蒙版

## 🚀 在线游玩

[👉 点这里开始游戏](https://trsoliu.github.io/gesture-balloon-game/)


## 💻 本地运行

由于浏览器的安全策略，摄像头访问需要通过 `http://` 或 `https://` 协议，不能直接用 `file://` 打开。

### 方法 1: Python 简易服务器

```bash
cd gesture-balloon-game
python3 -m http.server 8000
```

然后浏览器访问 `http://localhost:8000`

### 方法 2: Node.js serve

```bash
npx serve gesture-balloon-game
```

### 方法 3: VS Code Live Server

安装 Live Server 扩展，右键 `index.html` → "Open with Live Server"。

## 🎮 操作提示

- 光线充足的环境识别效果最好
- 保持手在摄像头视野中央
- 握拳需要至少 3 根手指明显弯曲，并且指尖要聚拢
- 想看实时识别数据可在浏览器控制台执行 `gameState.debugMode = true`

## 🧠 技术实现

### 手势识别算法

使用双重判定确保只有真正的握拳动作才触发：

1. **手指弯曲角度检测**: 计算每根手指 tip-pip-mcp 三点构成的夹角，角度 < 100° 判定为弯曲
2. **指尖聚拢度验证**: 4 个指尖到手掌中心的平均距离必须小于手掌大小的 1.3 倍

必须同时满足：至少 3 根手指弯曲 + 指尖聚拢。

### 抓取触发机制

仅在 "张开 → 握拳" 状态切换的那一帧检测碰撞。这意味着：
- 持续握拳移动不会连续抓到气球
- 必须做出 "抓" 这个明确动作
- 避免光标一碰到气球就被抓走的问题

### 项目结构

```
gesture-balloon-game/
├── index.html        # 游戏主文件（包含所有代码）
├── README.md         # 本文件
└── LICENSE           # MIT 许可证
```

所有代码都在单个 HTML 文件中，包括：
- MediaPipe Hands 集成
- Canvas 渲染（气球、手部骨架、粒子、特效）
- 游戏循环和状态管理
- UI 和事件处理

## 📦 依赖

通过 CDN 加载，无需 npm install：
- [@mediapipe/hands](https://www.npmjs.com/package/@mediapipe/hands) - 手部关键点识别
- [@mediapipe/camera_utils](https://www.npmjs.com/package/@mediapipe/camera_utils) - 摄像头工具

## 🌐 浏览器兼容性

- ✅ Chrome / Edge (推荐)
- ✅ Safari 14+
- ✅ Firefox 90+
- ❌ 不支持摄像头 API 的浏览器

## 📄 License

[MIT](./LICENSE)
