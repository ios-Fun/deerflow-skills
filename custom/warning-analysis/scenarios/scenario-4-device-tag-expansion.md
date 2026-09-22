# 场景4：设备和测点数据扩展问题

## 适用问题

- 需要进一步判断设备状态、异常参数或预警原因
- 查询设备整体状态（涉及多个测点）
- 查询设备下关联测点的数据
- 测点问题还是设备问题

## 允许工具

- `get_tag_statistic_data`
- `get_tag_values`
- `get_alarm_list`（若需要获取有效预警，但已有 alarmList 可跳过）

## 执行步骤

1. 若尚无报警单，先调用 `get_alarm_list` 获取有效预警（closed=false）涉及的测点ID。
2. 调用 `get_tag_statistic_data` 获取设备下关联测点统计信息：
   - 参数 `parentName` 为设备名称
   - 使用第一步确定的时间范围设置 `startTime` 与 `endTime`（中国时区）
3. 对涉及的测点ID并发调用 `get_tag_values` 查询测点具体趋势。
   - 返回：时间 `date`、实际值 `RealTimeData`、估计值 `Estimate`、严重度 `TagSeverity`。
4. 结合 `alarmList` 数据一起分析：
   - 确认设备关联有效测点报警单
   - 获取关键测点实时值趋势
   - 综合判断设备整体状态
   - 默认展示 5 个关键测点
   - 如果存在异常，重点分析异常最大的测点及其他测点是否同步偏离

## 输出要求

- 先给出设备整体状态结论。
- 展示关键测点数据表格及趋势图（因为调用了 `get_tag_values`）。
- 若存在异常，说明异常测点及同步偏离情况。