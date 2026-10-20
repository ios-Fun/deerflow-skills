---
id: fact_3bb5f69d
category: knowledge
confidence: 0.9
createdAt: '2026-10-19T03:18:40.322191Z'
source:
  type: conversation
  threadId: b45a1e29-0ef9-44bb-9956-1741a9b42776
expected_valid_days: 120
schemaVersion: 2
status: active
topics: []
consolidatedFrom: []
updatedAt: '2026-10-19T03:18:40.322191Z'
revision: 1
user_id: 420623b9-d8b3-4571-9fd1-1f1fabd66181
agent_name: plant-main-operator
---

# The health-evaluation workflow enforces an entity-confirmation gate, and its unit-entity registry was first disclosed (2026-10-19): the unit name must be confir

The health-evaluation workflow enforces an entity-confirmation gate, and its unit-entity registry was first disclosed (2026-10-19): the unit name must be confirmed by the user per system-registered names (以系统注册名称为准) — the responder will not self-extract or guess unit names from question text (even when the registered name '京燃机组' appears verbatim in the query); the registry currently contains only '京燃机组' (京燃电厂) as a registered unit entity and '#2机组' cannot match; fallback design: if the provided name matches, the health evaluation runs for that entity (诊断单+报警统计+故障分析); if not, the responder feeds back the list of actually registered units for confirmation and performs no speculative queries.
