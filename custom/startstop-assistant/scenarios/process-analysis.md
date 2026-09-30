---
name: startstop-process-analysis
parent: startstop-assistant
allowed-tools:
  - startStopStatic
  - startStopDetails
  - getStartStopTotalMaterial
  - get_event_status
  - get_event_report
  - get_best_record
  - match_for_best
---

# 场景 D：启停过程分析与复盘

## 分支路由

| 分支 | 触发语义 |
| ---- | ---- | ---- |
| D1 启停过程优化建议 | 怎么优化 / 缩短 / 压缩空间 / 偏高归因 |
| D2 历史启停对比评价 | 最近N次对比 / 哪次最好 / 最容易拖时间 / 重复问题 |
| D3 启停事件复盘 | 总结本次 / 复盘 / 还原时段 / 过程异常 |

> 同一问句命中多个分支时按优先级取最高。例如“最近3次哪一次最好、哪一次油耗偏高” → D2（评价优先于诊断）。
> 分支隔离：单轮只进入一个分支。


## 场景D公共逻辑
- 调用工具`match_for_best`, 参数`match_type`为'机组'，`match_string`为用户原语句，获取返回的实体id。
- 启动/停机类型由问句关键词判定。
- 根据用户语义获取时间段`startTime`与`endTime`。

## D1：启停过程优化建议

### D1处理逻辑（公共）
- 调用 `startStopStatic` 取本事件 `result_id`
	- 参数时间段`startTime`与`endTime`，参数`nodeId`为实体id
- 若 `startStopStatic` 返回的 `data` 为空数组 `[]` 或为空，严禁调用任何后续工具，直接告知用户无启停数据，不得尝试扩展时间范围、不得自行容错补偿查询
   
### **D1-a 时间、优化**（如 #1机组冷态启动还可以怎么缩短时间？/ 哪个阶段最有压缩空间？/ 本次滑参数停机怎么优化？ ）：
- 调用 `startStopDetails`（参数`resultId`）取本事件阶段耗时与异常标记
- 调用 `get_best_record` 获取最优事件，选取与本事件启动方式一致的resultId
- 调用 `startStopDetails`（参数为最优`resultId`）获取最优事件事件阶段耗时与异常标记
- 逐阶段对比：本事件阶段耗时 vs 历史同工况同阶段均值
- 计算可压缩量 = 本阶段耗时 − 历史均值（仅正值参与排名）
- 按可压缩量降序输出 TOP 阶段，标注每阶段压缩依据
- 结论句：可压缩空间集中在 X、Y 阶段，合计约 Z min。
   
### **D1-b 物耗偏差归因**（如：本次启动油耗偏高，主要高在哪个阶段？/ 本次启动除盐水消耗为什么比历史高？）：
- 调用 `getStartStopTotalMaterial`（`resultId`）获取本事件阶段物耗（currentMaterialList） + 上次阶段消耗（lastMaterialList）。
- 阶段对比：本事件物耗量 vs 上次消耗；
- 计算偏差率（保留 1 位小数），按偏差绝对值降序；
- 输出 TOP 阶段 + 偏差值 + 偏差率，定位主要高耗阶段；
- 上次消耗若为空则不进行对比。
- 指出可优化阶段、关键参数和等待环节；建议需在规程允许范围内，不建议突破安全约束。

## D2：历史启停对比评价

### 处理逻辑（公共）
- 调用 `startStopStatic` 取最近 N 条符合条件的事件（按完成时间倒序）。
- 逐事件调用 `startStopDetails` + `getStartStopTotalMaterial` 补充耗时与物耗。
- 调用 `get_event_report` （参数为`resultId`，和`startTime`与`endTime`）获取异常测点情况

### D2-a 多事件对比（如：最近3次冷态启动哪一次最好？）
- 对比：评价维度显式列出：总耗时、点火至并网耗时、启动物耗；
- 若部分接口返回为空，直接根据已有数据分析，禁止额外调用工具。

### D2-b 阶段耗时聚合分析（如：最近几次启动最容易在哪个阶段拖时间？）
- 跨 N 条事件，按阶段聚合耗时；
- 计算每阶段：平均耗时、最大耗时、超均值次数；
- 按“超均值次数 × 平均超幅”降序，输出最易拖时间的 TOP 3 阶段；
- 结论句：最近 N 次启动中，XX 阶段最易拖时间。

### D2-c 重复问题识别（如：本次启停有没有重复出现上次的问题？）
- 取本事件与上一次同类型事件的异常测点情况；
- 求交集：两次均出现的异常 → 标记为“重复问题”；
- 输出：重复问题清单 + 出现次数 + 对应事件 ID；
- 无重复问题 → 输出“本次未发现与上次重复的异常项”；
- 异常字段为空 → 标注“无采集数据，无法比对”。

### D2-d 当前启机预测（如：何时并网？何时达到负荷？何时打闸？）
- 额外调用`get_best_record`获取最优的result_id
	- 调用 `startStopDetails`（参数为最优的result_id）获取各阶段耗时
- 根据用户目标判断当前的阶段时间与最优的事件时间进行比较

## D3：启停事件复盘

### 处理逻辑（公共）
1. 定位本次事件 `result_id`（上下文复用）。
2. 调用 `startStopDetails`（`resultId`）取全流程事件序列与异常标记。

### D3-a 全过程总结（如：帮我总结一下本次启动过程）
- 额外调用 `getStartStopTotalMaterial`（参数为`resultId`）取物耗用于总结补充。
- 额外调用 `get_event_status` （参数为`resultId`）获取整个流程的成功运行的事件
- 额外调用 `get_event_report` （参数为`resultId`，和`startTime`与`endTime`）获取异常测点情况
- 输出：启停类型、关键节点时间轴（按照Details返回数据进行梳理）、总耗时、物耗概况、异常概况；
- 总体评价，生成简报。

### D3-b 异常提取\问题诊断（如：本次停机过程中发生过哪些异常、问题诊断）
- 额外调用 `get_event_report` （参数为`resultId`，和`startTime`与`endTime`）获取异常测点情况
- 输出：异常发生时间、异常简述、是否确认（closed）；
- 若涉及到问题详情，结合基本事件区间（start_event_name、end_event_name）与报警值（value）分析
- 若`get_event_report` 返回为空 → 输出“本次启停过程未采集到异常事件”；

### D3-c 时段还原（如：帮我还原 14:00 到 16:00 这段启动过程。）
- 解析起止时间，筛选 `startStopDetails` 中该时段内的事件；
- 按时间正序输出：时刻、阶段、事件、关键异常（若有）；
- 时段内无事件 → 正常整理流程回复用户；
- **不做插值补全**，只输出已采集事件。
