# Unit of Work Dependency

## Dependency Principle

Unit 間依存は **Demo Experience Unit が他 Unit を呼び出す orchestrator-centered structure** とする。Deadline Decision、Workload Estimation、Idle Planning の純粋ロジックは、できるだけ他 Unit を直接呼ばない。これにより、MVP デモの統合を簡単にしつつ、PBT 対象を分離しやすくする。

## Dependency Matrix

| From Unit | To Unit | Type | Reason |
|---|---|---|---|
| Demo Experience Unit | History and Persistence Unit | Calls | 類似履歴取得、分析結果保存、結果保存を行うため |
| Demo Experience Unit | Workload Estimation Unit | Calls | 作業量推定を取得するため |
| Demo Experience Unit | Deadline Decision Unit | Calls | 最新着手時刻、成功確率、リスク帯、説明を得るため |
| Demo Experience Unit | Idle Planning Unit | Calls | サボってよい時間と娯楽提案を得るため |
| Demo Experience Unit | Excuse Generation Unit | Calls | 言い訳文を生成するため |
| Workload Estimation Unit | History and Persistence Unit | Reads via Orchestrator-provided signal | 履歴補正に使うため。直接 Repository を持たず、Orchestrator から `HistorySignal` を受け取る |
| Idle Planning Unit | Workload Estimation Unit | Uses LLM adapter port in MVP | 娯楽提案に LLM を使う場合があるため。後続で LLM Assistance Unit に分離予定 |
| Excuse Generation Unit | Workload Estimation Unit | Uses LLM adapter port in MVP | 言い訳生成に LLM を使うため。後続で LLM Assistance Unit に分離予定 |
| History and Persistence Unit | None | Provides data | 保存と履歴シグナル生成に責務を限定する |
| Deadline Decision Unit | None | Pure domain logic | PBT 対象として独立性を保つ |

## Recommended Implementation Sequence

| Sequence | Unit | Why First |
|---:|---|---|
| 1 | Workload Estimation Unit | LLM と fallback の境界を先に作ると、以降の計算が安定する |
| 2 | Deadline Decision Unit | サービスの中核価値で、PBT 対象として早めに固める |
| 3 | Idle Planning Unit | Deadline Decision の出力を使い、デモでテーマ性を強く見せる |
| 4 | Excuse Generation Unit | 失敗時・危険時の創造性を追加する |
| 5 | History and Persistence Unit | MVP の分析結果保存と補正材料を実装する |
| 6 | Demo Experience Unit | BFF API と画面導線を統合する |

## Runtime Flow: Analyze Task

```text
Web screen
-> Demo Experience Unit
-> History and Persistence Unit
-> Workload Estimation Unit
-> Deadline Decision Unit
-> Idle Planning Unit
-> History and Persistence Unit
-> Demo Experience Unit response
-> Web result screen
```

## Runtime Flow: Generate Excuse

```text
Web screen
-> Demo Experience Unit
-> Excuse Generation Unit
-> Demo Experience Unit response
-> Excuse Draft screen
```

## Runtime Flow: Record Outcome

```text
Web screen
-> Demo Experience Unit
-> History and Persistence Unit
-> History Snapshot screen
```

## Coupling Controls

| Control | Reason |
|---|---|
| Deadline Decision Unit does not call LLM or DB | Pure functions and PBT applicabilityを守るため |
| Workload Estimation Unit returns uncertainty, not certainty | AI 推定を断定に見せないため |
| Idle Planning Unit consumes decision result, not raw deadline logic | サボり計画と締め切り判定の責務を分けるため |
| Excuse Generation Unit returns draft only | 責任ある送信判断はユーザー側に残すため |
| History Unit owns persistence | AWS DB への差し替えを簡単にするため |
| Demo Experience Unit owns BFF integration | MVP デモの画面/API 導線を単純に保つため |

## Later Split Points

| Current MVP Location | Later Unit | Split Trigger |
|---|---|---|
| Idle Planning Unit | Idle Entertainment Recommendation Unit | 嗜好学習、推薦カテゴリ、テンプレート数が増えた場合 |
| Workload Estimation Unit | LLM Assistance Unit | LLM 呼び出しが見積もり、言い訳、娯楽提案で共通化される場合 |
| Excuse Generation Unit | Failure Recovery Unit | 期限超過後の状態整理、再計画、結果保存を本格化する場合 |

## Dependency Risk Assessment

| Risk | Mitigation |
|---|---|
| Demo Experience Unit が大きくなりすぎる | Orchestrator は順序制御だけに限定し、各判断は domain package に置く |
| LLM Adapter が Workload Estimation Unit に偏る | `LLMAdapterPort` を共有型として定義し、後続で LLM Assistance Unit に分離可能にする |
| Idle Planning が普通の休憩提案に見える | リスク帯ごとに entertainment, escapism, excuse-prep を明示し、テーマ性を維持する |
| 履歴がただの CRUD になる | `Self-Deception History` として、推定補正と先延ばし傾向のシグナル生成を責務に含める |

## Extension Rule Compliance

| Extension | Status | Rationale |
|---|---|---|
| Security Baseline | Skipped | Disabled in Requirements Analysis. |
| Property-Based Testing | N/A | Dependency design preserves pure-function candidates for later PBT enforcement. |
