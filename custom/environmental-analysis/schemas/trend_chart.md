# 环保趋势图前端渲染协议

## 适用位置

环保指标分析中的：
> 环保指标趋势分析
适用于 NOX、SOX、粉尘、氨逃逸等环保指标的时间趋势展示。

## 输出要求
AI完成环保趋势分析并获取趋势数据后，必须输出以下固定结构：

```json
{
  "type": "environmental_trend",
  "title": "趋势图",
  "timeRange": {
    "start": "2026-08-13 13:00:00",
    "end": "2026-08-13 14:00:00"
  },
  "series": [
    {
      "tagName": "全厂NOX排放浓度（日平均）",
      "unit": "Mg/Nm3",
      "lowerLimit": 20.0,
      "upperLimit": 10.0
    }
  ]
}