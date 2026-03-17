# AI 键盘 Demo

> **在线体验：** <https://ai-keyboard-demo.vercel.app/>

面向年轻用户（18-30 岁）的 AI 增强输入法交互原型。产品主张「每个人的 AI 键盘」，核心差异化在于**个性化定制能力**——不仅是装扮维度，更是键盘功能的自定义能力。

本仓库是一个**纯前端单页 Demo**，用于演示产品的关键交互概念，不涉及后端服务或真实 AI 模型。

---

## 项目背景

### 产品架构（与 docs/specs 一致）

```
┌─────────────────────────────────────────────────────┐
│              个性化与定制化（横切层）                    │
│   AI服务组件化 · 工具栏自定义 · 场景智能推荐            │
│   AI助手与装扮系统                                     │
├───────────────┬───────────────┬─────────────────────┤
│  前端交互能力   │   模型能力     │     输入功能         │
│ · AI键交互     │ · 端云协同模型  │ · 智能联想与补全      │
│ · 智能体使用   │ · 主Agent路由  │ · 语音输入增强       │
│ · 问AI界面     │ · 智能体调度   │ · 个人信息快速调用    │
│ · 结果展示编辑  │ · 场景意图识别  │ · 智能提醒          │
│ · 场景推荐UI   │ · 信息提取     │                     │
└───────────────┴───────────────┴─────────────────────┘
```

合计 21 个功能模块，按四个工作组组织：前端交互（6）、模型能力（5）、输入能力（6）、装扮能力（4）。Demo 侧重演示**个性化与定制化**横切层和**前端交互能力**纵向维度。

### Demo 演示的功能范围

| 功能分类 | 具体功能 | 交互方式 |
|---------|---------|---------|
| 工具栏定制 | 拖拽排序、添加/移除工具、大小切换 | 长按工具栏进入定制模式 |
| 问 AI | 文本输入、语音输入（模拟） | 点击/长按工具栏光球按钮 |
| 场景智能推荐 | 切换场景后推荐内容条自动展开 | 左侧场景按钮切换 |
| AI 键 | 联想增强、AI 解码、智能体推荐、直接生成 | 键盘 AI 键点击 |
| 智能提醒 | 语音添加日程、日程提醒通知 | 长按光球语音输入 / 侧栏按钮 |
| 上屏能力 | 推荐内容、生成结果、问 AI 回答可上屏到输入框 | 点击对应内容 |

---

## 项目结构

```
AIkeyboardNew/
├── index.html                  # 核心 Demo 文件（~2850 行，HTML+CSS+JS 全内联）
├── CLAUDE.md                   # AI 编码助手上下文指引
├── README.md                   # 本文件
├── AI键盘目标人群及产品功能规划.pdf  # 产品规划原始文档（PDF）
├── 项目需求文档.md               # 21 模块需求清单（按工作组，与 specs 一致）
└── docs/
    ├── 项目启动会.md             # 产品定位、品牌、四个工作组、里程碑
    ├── plans/
    │   ├── 2026-03-06-ai-keyboard-feature-breakdown-design.md  # 功能架构设计
    │   └── 2026-03-06-ai-key-demo-design.md                     # AI 键 Demo 设计
    └── specs/                    # 按工作组的详细功能描述
        ├── pm-a-frontend-interaction.md   # 前端交互（6 模块）
        ├── pm-b-model-capabilities.md     # 模型能力（5 模块）
        ├── pm-c-input-capabilities.md     # 输入能力（6 模块）
        ├── pm-d-appearance-system.md      # 装扮能力（4 模块）
        └── pm-ui-ux-design-scope.md        # UI/交互设计参与范围
```

### 各文件说明

#### `index.html` — 核心 Demo（自包含单文件）

直接在浏览器中打开即可运行，无需构建或依赖。约 2850 行，结构如下：

| 行范围（约） | 内容 | 说明 |
|-------------|------|------|
| 1-1000 | CSS 样式 | 手机框架、聊天 UI、键盘布局、工具栏、定制面板、问 AI（输入/语音/结果抽屉）、候选栏、生成卡片、场景推荐条、提醒气泡、提示面板 |
| 1000-1220 | HTML 结构 | 手机框架（状态栏→App 栏→聊天区→模拟输入框→键盘区→工具栏→按键）、场景侧边栏、功能提示面板 |
| 1220-1500 | JS：数据与工具栏 | `TOOLS` 工具定义、`selectedIds`/`sizeConfig` 配置管理、`renderToolbar()`/`renderCtToolbar()` 渲染逻辑、拖拽排序 |
| 1500-1700 | JS：定制面板 | `openCustomize()`/`closeCustomize()` 定制模式、工具网格渲染、添加/移除/大小切换 |
| 1700-1900 | JS：手势与问 AI | `setupGesture()` 统一手势管理、`setAskState()` 状态机、语音录制模拟 |
| 1900-2100 | JS：问 AI 结果 | `submitQuestion()` 模拟 AI 响应、结果抽屉渲染、追问功能 |
| 2100-2500 | JS：场景与 AI 键 | `SCENES`/`AI_KEY_SCENES`/`REMINDER_SCENES` 场景数据、`switchScene()` 场景切换、`showAIKeyResult()` AI 键效果、场景推荐、提醒气泡 |
| 2500-2850 | JS：键盘与初始化 | 键盘按键事件、AI 键交互、上屏逻辑、`sendToChat()`、初始化流程 |

**关键全局状态：**
- `currentScene` — 当前激活的场景 ID
- `askState` — 问 AI 状态机（`idle` / `text-input` / `voice-recording` / `loading` / `result`）
- `aiKeyTriggered` — AI 键是否已触发
- `reminderState` — 提醒气泡状态（`idle` / `added` / `notify`）
- `selectedIds` / `sizeConfig` — 工具栏配置（持久化到 localStorage）

**关键函数：**
- `renderToolbar()` — 渲染主工具栏（分页 + 光球 + 推荐条/提醒气泡）
- `switchScene(sceneId)` — 切换场景（更新主题、聊天内容、输入框、AI 键状态）
- `setAskState(state)` — 问 AI 状态转换（控制工具栏/输入栏/语音栏/结果抽屉的可见性）
- `showAIKeyResult(result)` — 展示 AI 键结果（候选栏/智能体/生成卡片）
- `showReminderBubble(type)` — 在工具栏内展开提醒气泡
- `sendToChat(text, append)` — 上屏内容到模拟应用输入框
- `setupGesture(container, opts)` — 统一手势管理（长按/滑动翻页，区分各种交互区域）

**关键 DOM 节点：**
- `#toolbar` — 主工具栏容器
- `#askInputBar` / `#askVoiceBar` — 问 AI 文本/语音输入栏（绝对定位在 toolbar 内部覆盖）
- `#askOverlay` — 问 AI 结果半屏抽屉
- `#candidateBar` — AI 键候选栏
- `#aiGenCard` — AI 生成结果卡片
- `#simInputBox` — 模拟第三方应用输入框（上屏目标）
- `#chatArea` — 聊天消息区域

#### `CLAUDE.md` — AI 编码助手上下文

供 Claude Code 等 AI 编码工具使用的项目指引，包含架构概述、Demo 结构说明、关键 DOM/变量索引和语言约定。

#### `AI键盘目标人群及产品功能规划.pdf`

产品经理输出的原始规划文档，包含目标人群分析、产品愿景、功能规划。是整个项目的起点。

#### `项目需求文档.md`

按四个工作组组织的需求清单，与 `docs/specs/pm-a~pm-d` 及 `docs/项目启动会.md` 一一对应。含 21 模块的 checklist、模块依赖关系。

#### `docs/项目启动会.md`

产品与品牌定位、产品架构、四个工作组分工、里程碑、Demo 现状、团队沟通机制。

#### `docs/specs/` — 按工作组的详细功能描述

| 文件 | 内容 |
|------|------|
| `pm-a-frontend-interaction.md` | 前端交互（6 模块）：AI 服务组件化、工具栏自定义、场景推荐 UI、AI 键、智能体使用、问 AI |
| `pm-b-model-capabilities.md` | 模型能力（5 模块）：主 Agent 路由、智能体调度、信息提取、场景推荐策略、装扮性格与模型联动 |
| `pm-c-input-capabilities.md` | 输入能力（6 模块）：端云协同、场景识别、智能联想、语音增强、个人信息、智能提醒 |
| `pm-d-appearance-system.md` | 装扮能力（2 模块）：AI 助手与装扮系统（换装，性格与形象绑定）、键盘皮肤 |
| `pm-ui-ux-design-scope.md` | UI/交互设计参与范围（从四维度提炼） |

#### `docs/plans/` — 设计文档

| 文件 | 内容 |
|------|------|
| `2026-03-06-ai-keyboard-feature-breakdown-design.md` | 功能架构设计——三纵两横架构图、涉及维度、模块依赖 |
| `2026-03-06-ai-key-demo-design.md` | AI 键 Demo 交互设计——四种场景（联想/解码/智能体/生成）的交互流程和 UI 规格 |

---

## 运行方式

```bash
# 无需安装任何依赖，直接打开即可
open index.html        # macOS
start index.html       # Windows
xdg-open index.html    # Linux
```

或访问在线版本：<https://ai-keyboard-demo.vercel.app/>

---

## 部署

Demo 通过 [Vercel](https://vercel.com) 部署。推送代码到 [HawkkZhang/ai-keyboard-demo](https://github.com/HawkkZhang/ai-keyboard-demo) 仓库后，Vercel 会自动重新部署。

---

## 技术栈

- **纯 HTML/CSS/JS**，无框架依赖
- CSS：Flexbox 布局、CSS 动画（`@keyframes`）、`transition` 过渡、`linear-gradient` 主题
- JS：原生 DOM 操作、Pointer Events 手势管理、`localStorage` 配置持久化
- 所有内容内联在单个 HTML 文件中，便于分享和部署

---

## 语言约定

- 文档和 UI 文本：**中文（zh-CN）**
- 代码变量名和注释：**英文**
