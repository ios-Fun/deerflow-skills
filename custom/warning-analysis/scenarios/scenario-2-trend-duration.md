# 场景2：趋势和持续时间问题

## 适用问题

- 某测点/设备的趋势如何？
- 预警持续了多久？
- 参数偏差是否持续扩大？
- 参数持续偏差问题

## 允许工具

- `get_alarm_list`（用于获取 tagIds，若已有 alarmList 可跳过）
- `get_tag_statistic_data`
- `get_tag_values`

## 执行步骤

1. 若尚无 `alarmList`，先调用 `get_alarm_list` 获取有效预警（closed=false）涉及的测点ID，存入 `tagIds`。
2. 使用第一步确定的时间范围（中国时区），设置 `startTime` 与 `endTime`。
3. 对 `tagIds` 中的每个 `tagId` 并发调用 `get_tag_statistic_data`，查询指定时间范围内的预警历史及测点趋势统计。
   - 记录：系统/子系统/测点名/单位、实际值最小/最大/平均值、估计值最小/最大/平均值、严重度最小/最大/平均值、超限偏低/正常/偏高个数。
4. 对 `tagIds` 中的每个 `tagId` 并发调用 `get_tag_values`，查询测点具体趋势。
   - 返回：时间 `date`、实际值 `RealTimeData`、估计值 `Estimate`、严重度 `TagSeverity`。
5. 若问题涉及**持续时间**，结合 `alarmList` 中的 `firstTouchTime`、`lastTouchTime`、`total` 分析。

## 输出要求

- 突出趋势描述：偏差由【A】扩大至【B】，持续【X】分钟，目前趋势为扩大/稳定/恢复，已重复触发【X】次预警。
- 需要展示趋势图（调用过 `get_tag_values` 时必须）。
- 按公共输出格式给出结论和数据证据。