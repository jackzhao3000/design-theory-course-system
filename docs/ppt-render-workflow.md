# 《设计概论》AI-PPT 渲染工作流｜ppt-render-workflow

> 目标：让 lesson03–lesson08 可半自动/自动转化为正式PPT。

## Workflow-0：输入清单
- `lessonXX/page-script.md`
- `lessonXX/narration.md`
- `lessonXX/prompts.md`
- `course-freeze-v1.md`
- `course-rhythm-system.md`
- `shared/design-system.md`（或 `course-system/shared/design-system.md`）
- `shared/visual-system.md`（或 `course-system/shared/visual-system.md`）

---

## Workflow-1：页面脚本 → PPT
1. 解析页码与节奏标签（慢/中/快）。
2. 依据模板引擎选版式（概念/案例/讨论/总结）。
3. 填充标题、核心问题、互动区、图表区。
4. 自动写入页码与章节标识。

输出：`lessonXX_slides_draft.pptx`

---

## Workflow-2：AI提示词 → 背景图
1. 从 `prompts.md` 抽取 Prompt + Negative Prompt。
2. 生成任务清单（Task ID, Slide Ref, Priority）。
3. 批量出图（16:9，建议 1920×1080 或 2560×1440）。
4. 自动打回不合格图（模糊/水印/风格偏离）。

输出：`/assets/bg/*.png`

---

## Workflow-3：信息图生成逻辑
1. 从 page-script 抽取图表建议。
2. 按白名单映射图型（双轴、矩阵、泳道等）。
3. 将数据字段绑定到SVG模板。
4. 输出可编辑SVG并嵌入PPT。

输出：`/assets/svg/*.svg`

---

## Workflow-4：备注讲稿嵌入逻辑
1. 将 narration 按页切片。
2. 映射为“开场句/证据句/互动句/收束句”。
3. 写入PPT Notes区。
4. 检查单页备注时长是否匹配节奏。

输出：带备注的 `lessonXX_slides_notes.pptx`

---

## Workflow-5：页面编号系统
- 编号格式：`L03-01 ... L03-44`、`L04-01 ... L04-44`
- 页面角标固定右下角。
- 每次导出前自动校验编号连续性。

---

## Workflow-6：进度条系统
- 顶部细线进度条：按章节比例推进。
- 章节节点：开场 / 教材线 / 文明线 / AI线 / 总结。
- 当前节点高亮，已完成节点次高亮。

---

## Workflow-7：动画节奏系统
- 慢节奏页：1步动画（标题或结论淡入）
- 中节奏页：2步动画（结论→证据）
- 快节奏页：3步以内（冲突→判断→反馈）
- 动效参数：0.25s / 0.4s / 0.6s，禁止夸张转场

---

## Workflow-8：导出规范
- 编辑版：`pptx`（保留可编辑图层）
- 放映版：`pdf`（打印/评审）
- 资产包：`/assets/bg` + `/assets/svg` + `/notes`
- 命名：`DT_L03_v1.0_2026-05-14.pptx`（课程_课次_版本_日期）

---

## 自动化建议（供后续脚本化）
- Step A：Markdown解析器（页码、字段、节奏标签）
- Step B：模板渲染器（PPT版式引擎）
- Step C：图像任务调度器（Prompt批量生成）
- Step D：QA检查器（结构/视觉/叙事/导出）

## 成功判定
- lesson03–lesson08 任一课可在同一流程下生成一致风格PPT。
- 不改模板也能稳定输出“可讲、可评、可复用”课件。
