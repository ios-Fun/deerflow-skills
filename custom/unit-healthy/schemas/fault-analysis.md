# 故障分析前端渲染协议

## 适用位置

用于《机组健康状态评估报告》的：

> 2.设备异常及故障分析 → 故障模式及原因分析

## 输出要求

在进行故障分析前输出以下固定结构，其中incidentId为诊断单id：

```json
{
  "type": "fault_analysis",
  "version": "1.0",
  "incidentId": 0
}
```