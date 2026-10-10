---
name: startstop-assistant
description: >
  火电机组启停领域统一入口 Skill，覆盖四大类场景：
  A. 启停物耗查询（水/煤/油/厂用电、分阶段油耗、物耗对比）
  B. 启停记录与统计（启停次数、分类统计、最近一次详情、N次列表、停机时长）
  C. 启停阶段识别（当前阶段、并网剩余步骤、冷/温/热态判定、阶段切换时间）、停机xx、停止xx判定
  D. 启停过程（启停过程的优化、对比、复盘、状态）
  本 Skill 只做意图路由与结果汇总，具体分支逻辑委托对应场景文件执行。
  业务口径：仅处理生产闭环启停事件；试验/调试/检修试转类事件一律过滤。
allowed-tools:
  - startStopRecord
  - startStopStatic
  - startStopDetails
  - startStopCondition
  - getStartStopTotalMaterial
  - get_material_statistics
  - match_for_best
  - stageRecord
  - get_event_status
  - get_event_report
  - get_best_record
  - get_tag_values
  - standardChart
  
---

# 火电机组启停统一 Skill

## 0. 加载顺序（每次调用必执行）
1. `rules/global-rules.md`      —— 全局约束
2. `rules/data-filter-rules.md` —— 数据过滤与业务口径
3. 依据 §2 路由结果，加载 `scenarios/` 下**唯一一个**场景文件

> 约束：单轮问句只允许加载一个场景文件，禁止跨场景混合输出。

## 1. 门禁检查（Must Pass，先于路由）

| 编号 | 门禁条件                     | 不通过动作                            |
| ---- |--------------------------|----------------------------------|
| P1 | 业务领域为火电机组启停              | 退出本 Skill                        |
| P2 | 事件类型为生产闭环启停（非试验/调试/检修试转） | 按 data-filter-rules 过滤；全部被过滤则走退出本 Skill |
| P3 | 意图落在 A/B/C/D 四大类场景范围内      | 退出本 Skill                        |

## 2. 场景路由（优先级从高到低，命中即停）

### Route-A → `scenarios/material-query.md`
**命中条件**
- 关键词命中任一物耗指标：水、煤、油、厂用电、物耗、油耗、耗油、消耗量、阶段耗油；
- 物耗对比 / 物耗历史对比 / 差异率 / 占比语义。

### Route-B → `scenarios/record-stats.md`
**命中条件**
- 关键词命中次数/时长/记录/列表/详情/分类统计：启停了多少次、停机时长、启停记录、列出最近N次、最近一次详情、冷/温/热态分类统计、非计划启停。

### Route-C → `scenarios/stage-identify.md`
**命中条件（同时满足）**
- 语义指向“当前 / 进行中 / 现在 / 刚才”的机组状态识别；
- 关键词命中任一：当前阶段、启动到哪个阶段、停机到哪个阶段、并网还差、阶段切换时间、现在是什么启动类型、冷态/温态/热态判定、具备条件、停止xxx。

### Route-D → `scenarios/process-analysis.md`
**命中条件（任一）**
- 优化类关键词：怎么优化、怎么缩短、压缩空间、还有多少余量、因素趋势分析、参数状态核查与异常判定；
- 评价类关键词：哪一次最好 / 最差 / 最优、最容易拖时间、有没有重复问题、比上次高多少；
- 复盘类关键词：总结本次、复盘、还原时间段、过程异常、最值得复盘的问题。
- 曲线对比： 启停过程曲线对比、参数分析、参数异常评估、原因排查；

### 2.1 冲突消解规则

1. **“冷态/温态/热态”判定类问句**：
   - 含“当前 / 本次 / 现在” → Route-C；
   - 含“历史上 / 去年 / 上个月”等历史时间 → Route-B。
2. **诊断 / 评价 / 优化语义优先于查询语义**：
   - “最近 3 次哪一次最好 / 除盐水为什么比历史高” → Route-D；
3. **“报告”一词**：
   - 单事件“详情” → Route-B；多事件“报告” → Route-D。

## 3. 上下文与多轮
- 代词「本次 / 这次 / 现在 / 刚才 / 这两次」优先复用对话上下文中已识别的 `unitId` 与启停事件 `result_id`；
- 跨场景下钻时，允许复用已缓存的事件 ID，但仍只加载目标场景文件；
- 上下文缓存的字段：`unitId`、`timeRange`、`eventIndexMap`（业务事件ID ↔ result_id）。
