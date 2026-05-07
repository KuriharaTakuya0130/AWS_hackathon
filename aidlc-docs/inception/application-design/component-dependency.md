# Component Dependency

## Dependency Principle

依存関係は **Orchestrator-centered** とする。Procrastination Deadline Engineなどの中核コンポーネントが他コンポーネントを直接呼び出す構造にはしない。

これにより、以下を実現する。

- 各コンポーネントの責務を明確にする。
- Unit分解しやすくする。
- LLM失敗時のフォールバックをOrchestratorで扱いやすくする。
- MVPデモのBFF APIを単純にする。

## Dependency Matrix

| From | To | Dependency Type | Reason |
|---|---|---|---|
| Procrastination BFF API | Demo Experience Orchestrator | Calls | フロントエンド要求を内部体験へ変換する |
| Demo Experience Orchestrator | Task History Repository | Calls | 類似履歴取得と結果保存 |
| Demo Experience Orchestrator | Self-Deception History | Calls | 履歴シグナル生成 |
| Demo Experience Orchestrator | Workload Estimator | Calls | 作業時間推定 |
| Demo Experience Orchestrator | Procrastination Deadline Engine | Calls | 最遅着手時刻、成功確率、リスク帯算出 |
| Demo Experience Orchestrator | Guilt-Free Idle Planner | Calls | サボってよい時間計算 |
| Demo Experience Orchestrator | Idle Entertainment Recommender | Calls | 娯楽・休憩・現実逃避プラン推薦 |
| Demo Experience Orchestrator | Risk Explanation Composer | Calls | ユーザー向け説明生成 |
| Demo Experience Orchestrator | Excuse Generator | Calls | 言い訳生成 |
| Workload Estimator | LLM Adapter | Calls | LLM作業時間推定 |
| Idle Entertainment Recommender | LLM Adapter | Calls | LLM娯楽提案生成 |
| Workload Estimator | Self-Deception History | Receives data | 履歴シグナル利用 |
| Excuse Generator | LLM Adapter | Calls | LLM言い訳生成 |
| Self-Deception History | Task History Repository | Receives data | 類似履歴と結果履歴を解釈する |

## Data Flow: Analyze Task

```text
Frontend
-> Procrastination BFF API
-> Demo Experience Orchestrator
-> Task History Repository
-> Self-Deception History
-> Workload Estimator
-> LLM Adapter
-> Workload Estimator fallback if needed
-> Procrastination Deadline Engine
-> Guilt-Free Idle Planner
-> Idle Entertainment Recommender
-> Guilt-Free Idle Planner integrates suggestions
-> Risk Explanation Composer
-> Task History Repository save analysis
-> Procrastination BFF API response
-> Frontend result screen
```

## Data Flow: Generate Excuse

```text
Frontend
-> Procrastination BFF API
-> Demo Experience Orchestrator
-> Excuse Generator
-> LLM Adapter
-> Excuse Generator fallback if needed
-> Procrastination BFF API response
-> Frontend excuse draft screen
```

## Data Flow: Record Outcome

```text
Frontend
-> Procrastination BFF API
-> Demo Experience Orchestrator
-> Task History Repository
-> Frontend history snapshot
```

## Coupling Controls

| Rule | Rationale |
|---|---|
| Engine does not call LLM Adapter | Engine remains pure and PBT-friendly |
| Engine does not call Repository | Engine remains independent from persistence |
| Planner does not call Engine | Orchestrator passes Engine result to Planner |
| Idle Entertainment Recommender does not call Engine | Orchestrator passes risk and idle context to the recommender |
| Excuse Generator does not save history | Persistence remains Repository responsibility |
| BFF does not call domain components directly except Orchestrator | Demo flow remains consistent |

## Unit Generation Implications

| Component | Likely Unit | Notes |
|---|---|---|
| Procrastination Deadline Engine | Deadline Decision Unit | Core logic, PBT-heavy |
| Workload Estimator + LLM Adapter | Workload Estimation Unit | LLM + fallback boundary |
| Guilt-Free Idle Planner + Idle Entertainment Recommender | Idle Planning Unit | MVPでは同一Unit。後続でIdle Entertainment Recommendation Unitとして独立可能 |
| Excuse Generator | Excuse Generation Unit | Demo-critical creative feature |
| Self-Deception History + Repository | History Unit | Persistence and personalization |
| BFF + Orchestrator | Demo Experience Unit | End-to-end MVP flow |

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | Dependency design preserves pure-function candidates for later PBT enforcement. |
