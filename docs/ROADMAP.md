# aquascape-studio ROADMAP

> 维护者：orchestrator（兼产品经理）。每次迭代结束时更新。最后更新：2026-11-12。

## 核心差异点
"看着自己设计的缸长起来" —— 由真正的生长模拟 (growth simulation) 引擎驱动，而不是预渲染动画。

## 优先级与状态

| # | 方向 | 状态 | 备注 |
|---|------|------|------|
| 1 | 生长延时 (growth time-lapse)：0–90 天时间轴、种间竞争、缺光区域；TS 引擎在 Web Worker 中运行 | 🟡 进行中 | 11-12：sim-core TS 引擎 + /studio/sim 时间轴（功能开关 `growthTimelapse` 后，默认关，等视觉确认） |
| 2 | 3D 编辑器和渲染重做：拖拽石头/沉木/水草，Iwagumi / Nature / Dutch 模板，构图辅助线 | ⚪ 未开始 | 下一步：正面视角风格化渲染（instancing 叶片 + MeshPhysicalMaterial transmission） |
| 3 | AI 功能：拍照识别水草 + 改造建议；按缸尺寸和风格自动布局 | ⚪ 未开始 | |
| 4 | 扩充水草库到 150+ 种 | 🟡 进行中 | 11-12：36 → ≥70 种 |
| 5 | 降低基础设施成本（去 NAT、精简 Fargate） | ⏸ 需 Xiaopu 批准 | 涉及删除/替换 AWS 资源，必须先问 |

## 视觉风格（Xiaopu 于 2026-11-11 选定）

- **页面风格：Apple 官网感**。白色背景，留白多，大字号粗体标题（字距收紧），产品（水缸）是绝对主角，滚动时有流畅的动画，磨砂玻璃导航和控制条，按钮是胶囊形。整体精准、高级、克制。
- **水缸本身：好看但风格化，不追求照片级写实**。像高质量游戏或 Apple 自己的 3D 插画：干净、柔和、精致。
  - 叶片圆润、有厚度，带次表面散射 (subsurface scattering) 的柔和透光，叶片向上倾斜约 35°，正面看不能是侧边的细线。
  - 配色：清新的绿色（深浅渐变），珊瑚橙作为红色水草的强调色；石头是光滑的雕塑感造型，不要粗糙的写实纹理。
  - 柔和的影棚打光，背景纯白透亮。
  - **只用正面视角**（像造景比赛照片那样正对着缸），不要侧视图和斜 45° 视角。
  - 叶片要密、层次要多，不能稀疏，不能像火柴棍。
- **已否决，不要再用**：墨绿深色主题 (ink-green)、纸白留白风 (A)、浅水蓝清透风 (B)、石墨灰仪器风 (C)、照片级写实、侧视图和斜视角。插件里的 ink-green-design-system skill 和 app-agent 的 ink-green 设定已弃用。
- **视觉参考**：Design 画布 https://claude.ai/artifact/37R49gWPpKrP32C6ppm4Sa 的 "v3 · 超白玻璃" 页面。
  - 从中提取的令牌 (design tokens)：背景 `#FFFFFF`，正文 `#1D1D1F`，次要文字 `#6E6E73`，卡片 `#F5F5F7`（圆角 28px），主色 `#0A6F78`（hover `#064C52`），健康绿 `#1D7A3E`，警示珊瑚 `#B2441F`；字体 Inter + Noto Sans SC；H1 `clamp(44px,6.6vw,88px)` 700 字距 -0.04em；磨砂导航 52px `rgba(255,255,255,.78)` + `blur(20px) saturate(180%)`；胶囊控制条 + 44px 圆形播放键 + 分段跳转 10/35/60/90 天。
- **上线规则**：首页、编辑器的视觉改动先放在功能开关 (feature flag) 后面，部署时默认关闭；截图请 Xiaopu 确认，认可后下一轮再打开。

## 交互原则
首页即水缸；画布优先 (canvas-first) 编辑器，底部时间轴叠加生长曲线；⌘K 命令面板一句话造景；动效有功能 (functional motion)，尊重 prefers-reduced-motion；对比度 4.5:1，触控目标 ≥44px。

## 功能开关 (feature flags)

| 开关 | 环境变量 | 默认 | 状态 |
|------|----------|------|------|
| `growthTimelapse` | `NEXT_PUBLIC_FF_GROWTH_TIMELAPSE` | 关 | 等 Xiaopu 确认截图（见 docs/iterations/2026-11-12.md） |

## 契约 (contracts, orchestrator-only)
- `aquascape-sim/ts/src/contract.ts` — `@aquascape/sim-core` v1.0.0（SimInput / Frame / SimResult / Worker 协议）
- `aquascape-botany/schema/plant.schema.json`
