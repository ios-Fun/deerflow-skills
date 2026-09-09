# 故障分析前端渲染协议

## 适用位置

用于填充在文末的附录处

## 输出要求

在对应flow明确说明需要输出 【故障推导图】 的地方，都输出如下json格式的固定结构，其中incidentId为对应的诊断单id，数字类型：

```json
{
  "type": "fault_analysis",
  "version": "1.0",
  "incidentId": 0
}
```