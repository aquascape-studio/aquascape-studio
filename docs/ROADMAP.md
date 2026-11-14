# aquascape-studio ROADMAP

> 维护者：orchestrator（兼产品经理）。每次迭代结束时更新。最后更新：2026-11-14。

## 核心差异点
"看着自己设计的缸长起来" —— 由真正的生长模拟 (growth simulation) 引擎驱动，而不是预渲染动画。

## 优先级与状态

| # | 方向 | 状态 | 备注 |
|---|------|------|------|
| 1 | 生长延时 (growth time-lapse)：0–90 天时间轴、种间竞争、缺光区域；TS 引擎在 Web Worker 中运行 | 🟢 已上线 v1 | 11-12：sim-core TS 引擎 + /studio/sim 时间轴，视觉已获批准，开关已打开 |
| 2 | 3D 编辑器和渲染重做：拖拽石头/沉木/水草，Iwagumi / Nature / Dutch 模板，构图辅助线 | 🟡 进行中 | 11-12 r2：`<FrontalTank>` 正面风格化水缸上线。11-14：FrontalTank 1.1.0（角色石、埋石、坡形底床、三分线/黄金分割辅助线、布局缓存）+ 4 个风格模板（三尊石组 / 三角 / 凹形 / 荷兰街道），`styleTemplates` 开关**默认关，等 Xiaopu 确认**。下一步：拖拽编辑 |
| 3 | AI 功能：拍照识别水草 + 改造建议；按缸尺寸和风格自动布局 | ⚪ 未开始 | |
| 4 | 扩充水草库到 150+ 种 | 🟡 进行中 | 11-12：36 → 78；11-13：→ 111；11-14：→ 142 种（都有 `visual`）。约 130 条叶片尺寸/包络为估计值待补来源；JSON Schema/Python 校验器与数据不一致，待对齐 |
| 5 | 降低基础设施成本（去 NAT、精简 Fargate） | 🟢 dev 已完成 | [infra#1](https://github.com/aquascape-studio/aquascape-infra/pull/1) 已合并（11-12，Xiaopu 授权）：dev 去 NAT、去 ALB、Spot、定时缩到 0，约 $5/月；prod 未动。dev 只在每天 01:30–04:00 PT 运行 |

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

## Xiaopu 的长期授权（2026-11-12）
Xiaopu 说："该合并合并，不用等我 approve，我一周之后来看成果。"因此从 11-12 起到他回来（约 11-19）：
- **视觉改动不再等确认**：做完、自测、截图写进迭代日志后，直接打开功能开关上线；每次都保留"revert 开关 PR"的回滚路径。
- **可以合并待合并的 PR**（CI 绿、符合契约），包括已在 dev 生效的 infra PR。
- **仍然不做**：在控制台/CLI 手动建资源、轮换或泄露密钥、超出预算、删仓库、force-push 默认分支。新增 AWS 资源的 PR 照常写清费用；prod 资源删除或替换仍先在日志里写清理由，能等就等他回来。
- 每天照常写迭代日志 + 截图，方便他回来一次看完。

## 交互原则
首页即水缸；画布优先 (canvas-first) 编辑器，底部时间轴叠加生长曲线；⌘K 命令面板一句话造景；动效有功能 (functional motion)，尊重 prefers-reduced-motion；对比度 4.5:1，触控目标 ≥44px。

## 功能开关 (feature flags)

| 开关 | 环境变量 | 默认 | 状态 |
|------|----------|------|------|
| `growthTimelapse` | `NEXT_PUBLIC_FF_GROWTH_TIMELAPSE` | 开（prod） | Xiaopu 于 2026-11-12 批准视觉，aquascape-web#3 打开 |
| `stylizedTank` | `NEXT_PUBLIC_FF_STYLIZED_TANK` | 开（prod） | Xiaopu 免确认，[web#6](https://github.com/aquascape-studio/aquascape-web/pull/6) 打开（11-12）；回滚 = revert web#6 |
| `styleTemplates` | `NEXT_PUBLIC_FF_STYLE_TEMPLATES` | 关 | [web#9](https://github.com/aquascape-studio/aquascape-web/pull/9)（11-14）。本轮打开开关的操作被权限拦下，按原规则等 Xiaopu 看截图后再开 |

## 契约 (contracts, orchestrator-only)
- `aquascape-sim/ts/src/contract.ts` — `@aquascape/sim-core` v1.1.1（SimInput / Frame / SimResult / Worker 协议；+parDailyMean、substrate 网格、cancelled、事件与包络规则）
- `aquascape-render/src/contract.ts` — FrontalTank v1.1.0（+石头角色、buriedFrac、guides、focalPoint、bed）
- `aquascape-web/lib/templates/contract.ts` — 风格模板 v1.0.0（ScapeTemplate / instantiate）
- `aquascape-botany/schema/plant.schema.json` + `js/schema.ts` — 新增可选 `visual` 块

## 当前视觉决定（11-12 r2）
- 3D 水缸：实例化 (instancing) 带厚度圆润叶片 + shader 背光假透光（不用 transmission，性能原因）；纯白影棚 + 柔光；只正面相机；光滑卵石；坡形底床；红色水草用 botany `tipColorHex`，没有时用珊瑚橙 #E8735A。

## 当前视觉决定（11-14）
- 石头：按 Iwagumi 角色塑形（主石最高略前倾，副石/配石依次低宽，散石扁平），光滑雕塑感，埋入底床。
- 茎草：对生/轮生/互生叶序，叶片上倾约 35°，顶端收芽，有侧枝（去掉"鱼骨"感）。
- 辅助线：1px #1D1D1F 35% 透明度 + 焦点圈，只在屏幕空间叠加。
- 模板条：磨砂胶囊条 + 分段控件，主色 #0A6F78。
- 字体：Inter（@fontsource 打包）+ 系统中文字体栈，构建不联网。
