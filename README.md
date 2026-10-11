# BioBench Viewer — 静态演示

BioBench 查看器的前端部分，单独摘出来的版本。它是一个**纯静态页面**：没有
后端、没有 LLM、没有构建步骤，用浏览器打开就能看结构。

原始项目：[BioBench](https://gitee.com/LRC-bit/biobench) —— 把本地 GPU 上的
生物分子 AI 模型包装成 Agent Skills，用自然语言驱动，结果发布到这个查看器。

## 打开方式

**本地**：直接双击 `index.html`，或者

```bash
python3 -m http.server 8000
# → http://localhost:8000
```

**GitHub Pages**：仓库 Settings → Pages → 选 `master` 分支根目录，等一分钟，
访问 `https://<用户名>.github.io/biobench-static/`。

## 能看到什么

| 文件 | 内容 |
|------|------|
| `current.cif` | 当前显示的结构（mmCIF） |
| `current.json` | 结构元数据：链、角色、残基数、指标 |
| `status.json` | 任务进度历史 |
| `structures.json` | 本次会话发布过的所有结构 |

页面每 2 秒轮询一次 `current.json`，`updated` 字段变化就重新加载结构并渲染。
所以**替换这几个文件，页面内容就变了**——不需要改任何代码。

当前示例是流感病毒血凝素（1RU7）的三聚体，加上一条 12 残基环肽和三个
fpocket 找到的口袋。

## 交互

- **旋转/缩放**：拖拽旋转，滚轮缩放，触摸屏单指旋转双指缩放
- **切换结构**：左上角结构名点开，会话内发布过的每个结构一个标签页
- **链面板**：右侧链条目，可单独显隐、改样式（cartoon/surface/stick）
- **残基选择**：点击残基选中，左侧 Selection 面板列出所选
- **序列视图**：左侧序列面板，点击序列位置选中对应残基

## 和完整版的区别

这个仓库只有前端。完整版里查看器右侧还有一个 Agent 面板，能用自然语言让
模型跑对接、设计、模拟——那需要 `serve.py` 和 opencode 服务，不在这里。

所以演示版里：

- Agent 面板会显示"未安装"，发送按钮禁用——**这是预期的**，不是故障
- 系统监控（CPU/GPU）无数据，对应组件不渲染
- 结构发布、进度上报不可用——那几个文件是静态快照

## 文件说明

```
index.html          查看器全部代码（HTML + CSS + JS，单文件）
3Dmol-min.js        3Dmol.js 本地副本，无 CDN 依赖
current.cif         当前结构
current.json        结构元数据
status.json         任务进度
structures.json     结构索引
stage-bg.png        背景图（可删，删掉用纯色）
```

`index.html` 约 6200 行，包含 3D 渲染、链面板、序列视图、残基选择、任务
进度条等全部逻辑。改 `current.cif` 和 `current.json` 就能换成任意结构。

## 许可

与 BioBench 主仓库一致，MIT。
