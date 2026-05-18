# 数学建模课程演示 — 工作日志

## 日期：2026-05-18

---

### 1. 会话恢复与项目定位

- 上次会话（104ac180）无法直接 resume，通过查找 `.claude/projects` 下的 session transcript 恢复上下文
- 定位到项目目录：`D:\projects\math-modeling-guide\`（HTML 动画）和 `D:\projects\math-modeling-video\`（口播稿+脚本）

### 2. 口播稿分段

- 从 `script.md`（78 行完整口播稿）按 `---` 分隔符拆分为 6 个章节：
  - chapter1 — 什么是数学建模
  - chapter2 — 六步建模法
  - chapter3 — 工具箱（线性规划、回归、微分方程、AHP、灰色预测等）
  - chapter4 — 论文写作
  - chapter5 — 学习建议
  - chapter6 — 总结与预告
- 分段文件保存至 `D:\projects\math-modeling-video\audio\chapter1~6.txt`

### 3. 配音生成（Voicebox API）

- 配音引擎：Voicebox API v0.5.0（本地 `http://127.0.0.1:17493`）
- 语音角色：赛yide（profile ID: 42842730-578c-467d-be94-df83defdd01c）
- TTS 引擎：Qwen3-TTS 1.7B（CPU 模式）
- 逐章提交生成 → 轮询状态 → 下载 WAV 音频
- 遇到问题：同时提交多个请求导致排队，改为顺序提交+等待解决
- 最终产出：
  | 文件 | 大小 | 时长（约） |
  |------|------|-----------|
  | chapter1.wav | 1.6 MB | 33s |
  | chapter2.wav | 2.8 MB | 58s |
  | chapter3.wav | 3.3 MB | 69s |
  | chapter4.wav | 2.2 MB | 46s |
  | chapter5.wav | 2.1 MB | 44s |
  | chapter6.wav | 1.1 MB | 24s |

### 4. 音频合并

- 使用 Python `wave` 模块将 6 个章节 WAV 合并为 `full_narration.wav`
- 章节间插入 1.5 秒静音间隔
- 总时长：286.5 秒（约 4 分 47 秒），大小 13.1 MB

### 5. HTML 演示开发（index.html）

**v1 — 基础横屏幻灯片**
- 6 页全屏横向 slide，flex 布局 + translateX 切换
- 底部圆点 + 页码 + 进度条
- 右上角音频控件（播放/暂停/静音/进度条）
- 键盘：空格/→ 下一页，← 上一页
- 音频跟随页面自动切换

**v2 — 逐步出现重构**
- 每页拆分为 3-4 个 reveal 步骤
- 点击/空格只显示当前页下一步，全部显示完才切页
- 左键返回上一步，到第一步再返回上一页
- 底部 HUD 弱化（45% 透明度，hover 提亮）
- 去掉"上一页/下一页"大按钮

**v3 — Apple Keynote 风格尝试（已回退）**
- 纯黑背景、极简排版、左侧列表式布局
- 用户反馈效果变差，回退到 v2

**v4 — 高级化微调（当前版本）**
- 在 v2 基础上调整 CSS，不改 HTML 结构
- 色彩：accent 改为 `#4e8cff`，背景加径向渐变，glow 降至 6%
- 排版：标题 700 字重，去掉渐变填色，kicker 去掉 pill 边框
- 卡片：从 gap+border 改为 1px 分割线融合风格
- 动效：统一 0.7s，translateY 32→20px
- HUD：透明度 35%，进度条 1px

### 6. 文件部署

- 音频文件复制到 `D:\projects\math-modeling-guide\` 与 HTML 同目录
- Python HTTP 服务器 `localhost:8080` 提供本地访问

### 7. GitHub 上传

- 仓库：https://github.com/windy341524-glitch/math-modeling-guide
- 可见性：Public
- 包含 11 个文件（index.html + 6 WAV + 4 HTML 章节页）

---

### 关键文件清单

| 路径 | 用途 |
|------|------|
| `D:\projects\math-modeling-guide\index.html` | 主演示（横屏幻灯片 + 配音） |
| `D:\projects\math-modeling-guide\chapter1~6.wav` | 6 章配音音频 |
| `D:\projects\math-modeling-guide\数学建模基础-动画演示.html` | 原始长滚动动画版 |
| `D:\projects\math-modeling-video\script.md` | 完整口播稿 |
| `D:\projects\math-modeling-video\audio\` | 分段文本 + 原始 WAV + 合并音频 |
| `D:\projects\math-modeling-video\generate_remaining.py` | 配音生成脚本 |
