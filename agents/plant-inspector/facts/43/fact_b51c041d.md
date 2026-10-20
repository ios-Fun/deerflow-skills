---
id: fact_b51c041d
category: knowledge
confidence: 0.9
createdAt: '2026-10-19T02:28:48.243020Z'
source:
  type: conversation
  threadId: 8f50fcbd-ae72-4bcc-b701-7a2012e1c325
expected_valid_days: 180
schemaVersion: 2
status: active
topics: []
consolidatedFrom: []
updatedAt: '2026-10-19T02:28:48.243020Z'
revision: 1
user_id: 420623b9-d8b3-4571-9fd1-1f1fabd66181
agent_name: plant-inspector
---

# 用户电厂平台的启停技能自报标识 'startstop-assistant' (启停助手)， 工具绑定含 match_for_best (疑为机组实体解析/最优匹配)、startStopStatic (启停统计)、startStopDetails (启停详情)； 场景路由带分支编号 (B3 = 「最近一次启动详情」)，

用户电厂平台的启停技能自报标识 'startstop-assistant' (启停助手)， 工具绑定含 match_for_best (疑为机组实体解析/最优匹配)、startStopStatic (启停统计)、startStopDetails (启停详情)； 场景路由带分支编号 (B3 = 「最近一次启动详情」)， 查询算法 = 机组实体解析 → 启动事件按开始时刻倒序取第一条 → 读取 '启动类型' 字段 (冷/温/热， 依缸温/停机冷却时间标签判定， 实体解析结果优先于 '京燃机组' 固定映射)； 工具未接入时按数据完整性约束拒答并请用户在平台侧核对绑定。
