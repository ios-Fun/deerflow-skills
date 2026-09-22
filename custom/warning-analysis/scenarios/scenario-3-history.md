# 场景3：历史统计问题

## 适用问题

- 明确要求查询历史预警统计
- 过去某段时间的预警次数
- 已关闭预警等

## 允许工具

- `unit_alarm_list_statistics`

## 执行步骤

1. 调用 `unit_alarm_list_statistics` 查询相关测点报警单统计，不携带`closed`参数。
   - 对象为机组：`unitId` 设置为机组ID
   - 对象为设备：`assetId` 设置为设备ID
   - 对象为测点：`tagId` 设置为测点ID
   - 结合所给时间段设置 `startTime` 与 `endTime`（中国时区）
   - 若问题涉及到设备分组，设置参数 `group_by_asset_name` 为 `true`
2. 将获取结果存入 `alarmListStatistic`。
3. 若结果为空，直接回复“未查询到相关历史预警记录”。

## 输出要求

- 统计预警类型、预警总条数（count）、涉及的测点名称（tagName）。
- 可列表展示，按报警次数排序。