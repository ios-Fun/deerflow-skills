---
id: fact_a59fe177
category: knowledge
confidence: 0.85
createdAt: '2026-10-19T01:44:40.030785Z'
source:
  type: conversation
  threadId: 86fbe4d0-1e75-4f26-af61-bfe618dd97cf
expected_valid_days: 180
schemaVersion: 2
status: active
topics: []
consolidatedFrom: []
updatedAt: '2026-10-19T01:44:40.030785Z'
revision: 1
user_id: 420623b9-d8b3-4571-9fd1-1f1fabd66181
agent_name: equip-deputy-director
---

# Cold-state attribution re-issue finding (startup/shutdown record library): after the 2026-10-16 cold-state inquiry was refused for interface unavailability, the

Cold-state attribution re-issue finding (startup/shutdown record library): after the 2026-10-16 cold-state inquiry was refused for interface unavailability, the re-issued '最近一个#2机组启动是冷态的吗' was answered definitively — 热态启动 (not cold) citing the 启停闭环记录库 '最近一次核验口径（2026-10-16 第六轮查询，连续六轮结果一致）' while explicitly stating 本轮未能重新执行启停记录接口查询, with staleness caveats (caliber predates 2026-10-16; '最近一次' may have changed if newer closed-loop startups exist; re-verify before formal citation; production closed-loop events only, 试验/调试/检修试转 filtered). Findings: (1) no-data refusals occur only when no previously verified caliber exists — otherwise the responder answers from cached caliber with explicit staleness disclaimers; (2) first cross-round citation of a specific prior query round ('第六轮查询') as verification basis (轮次记账对应关系未明); (3) the definitive 热态启动 leaves the 启动类型 field still oscillating (热态启动 cached/rounds 8-9 vs 无采集数据 rounds 7/10) with the backend caliber unresolved; (4) 12-18 14:27 启动发起时间 presented without the 口径偏差 caveat; milestone node set varied again (added 余热锅炉上水完成 07:09 / 盘车脱扣点火升速 08:24, omitted 汽机发电机并网 12:31, no 建单 info); 循环水 05:02 长期挂起 + 技术推测 flag retained
