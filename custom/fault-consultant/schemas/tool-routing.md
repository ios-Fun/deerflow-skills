# 工具路由表

## 公共前置：获取诊断单上下文

### 路由规则（按 type 字段互斥执行）

| type 值 | 工具 | 参数 | 短路条件 |
|---------|------|------|---------|
| 设备 | `cg_device_healthy` | name=user_enrichment.name | 空结果→回复"运行正常"，终止 |
| 机组 | `unit_select_incidents` | unitName=user_enrichment.name | 空结果→回复"运行正常"，终止 |
| 其他（测点等） | `unit_select_incidents` | unit_name留空 | 选择最匹配诊断单 |

### 公共规则

- 未提供时间 → 禁止臆造，调用 `ask_clarification` 澄清
- 提及"今天/现在/当日" → 自动转换为 `[今日 00:00:00+08:00, 次日 00:00:00+08:00]`
- 返回"未匹配到设备/机组" → 展示可用列表让用户选择，确认后重试
- `closed` 参数默认不传；仅当用户明确要求"未关闭/仍存在"时才传 `closed=false`

## 各意图工具序列

| 意图 | 必需工具 | 可选工具 | 说明 |
|------|---------|---------|------|
| A | `unit_select_incidents` 或 `cg_device_healthy` | `unit_graph_show` | 列清单+推导图 |
| B | `cg_device_healthy` + `default_graph_detail` | `get_tag_statistic_data` | 先诊断单，再无诊断单走图推断 |
| C | `default_graph_detail` | `get_tag_statistic_data` | 围绕诊断单和用户输入测点分析 |
| D | `get_tag_statistic_data` + `get_tags_of_instance` | `deviceRag` | 对比实际vs预估 |
| E | `cg_device_healthy` + `deviceRag` | `get_tag_statistic_data` | 严重程度+建议 |
| F | `cg_device_healthy` | `get_tag_statistic_data` | 历史对比或效果评估 |
| G | `match_for_best` + `default_graph_detail` | - | 查故障模式知识 |
| H | `cg_device_healthy` 或 `unit_select_incidents` | `unit_graph_show` | 解释诊断单 |

## 补充工具说明

| 工具 | 用途 |
|------|------|
| `unit_graph_show` | 获取故障模式推导图 |
| `get_tag_statistic_data` | 获取测点历史/趋势数据 |
| `get_tags_of_instance` | 获取测点描述信息 |
| `default_graph_detail` | 获取故障模式推导图（按设备或测点） |
| `deviceRag` | 获取 RAG 知识库/规程内容 |

## 工具调用失败处理

- 说明原因并继续用已有数据分析
- 结论必须基于工具返回数据，不得臆造
- C类特殊：接口调用失败时，直接告知用户该链路不可通，之后查阅别的链路