---
id: fact_8e56b3a4
category: knowledge
confidence: 0.9
createdAt: '2026-10-19T03:31:48.027549Z'
source:
  type: conversation
  threadId: 1d547c3f-ef43-4169-b556-dd0c6f60d993
expected_valid_days: 90
schemaVersion: 2
status: active
topics: []
consolidatedFrom: []
updatedAt: '2026-10-19T03:31:48.027549Z'
revision: 1
user_id: 420623b9-d8b3-4571-9fd1-1f1fabd66181
agent_name: plant-main-operator
---

# 京燃机组 2026-04 monthly health report reproducibility finding (as of the newest round): the report has now been delivered three times across sessions with core dat

京燃机组 2026-04 monthly health report reproducibility finding (as of the newest round): the report has now been delivered three times across sessions with core data zero-drift (机组ID 805126; 健康评分 100.0; 报警合计 6929 — 综合 136/超限高 787/超限低 434/一级预警 437/二级预警 5135; 唯一诊断单 131100 #2热网循环水泵驱动端轴承轴瓦固定螺栓松动, 轻微, stopped triggering after 2026-04-02), while (a) the entity-confirmation gate first disclosed 2026-10-19 did not trigger in either subsequent verbatim re-send (two consecutive non-gate rounds — session/routing path-dependence hypothesis strengthened) and (b) report-level label fields oscillate: analysis window '2026-04-01 00:00 至 2026-05-01 00:00 (UTC)' (earlier delivery) vs '2026-04-01 至 2026-04-30（系统时间口径）' (newest — same month, different label; first window-label drift in this series); the report is not a byte-fixed template — core fields stable, auxiliary details vary per round.
